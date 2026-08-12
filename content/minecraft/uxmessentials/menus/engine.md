---
title: Custom Menu Engine
order: 1090
description: 'Writing a menu file: layout, items, sources, pagination and input.'
---

A custom menu is a single HOCON file in `plugins/uxmEssentials/menus/`. The file name is the menu's name:
`menus/shop.conf` opens with `/menu open shop`.

The same engine that draws every [built-in menu](built-in.md) loads your files, so
everything on this page applies equally to both. A menu that fails to parse is skipped
with a warning in the log: one bad file never hides the rest.

## The smallest menu

```hocon
title = "<dark_aqua>Hello Menu"
rows = 3

items {
  greet {
    slot = 13
    material = OAK_SIGN
    name = "<yellow>Say hello"
    lore = ["<gray>Click me."]
    click {
      left = ["message:<gold>Hello from a custom menu!"]
    }
  }
}
```

Save it as `menus/hello.conf`, run `/menu reload`, then `/menu open hello`. That is the
whole loop.

<Callout type="warning" title="Titles and text are verbatim MiniMessage, not catalog keys">

Names, lore and titles in a custom menu are written as **MiniMessage** and rendered
exactly as you type them. They are **not** run through the message catalogs, so style
them however you like: `"<gradient:#55ffff:#5555ff>Shop</gradient>"`. This is the one
place in the plugin where inline text is expected rather than a `@catalog.key`.

</Callout>

## Menu-level keys

These sit at the top of the file, outside `items { }`.

| Key | Meaning |
|---|---|
| `title` | The window title (MiniMessage; may hold `%placeholders%`). |
| `rows` | Chest height, `1` to `6`. |
| `inventory-type` | Optional non-chest shape (`hopper`, `dispenser`, …). Falls back to a `rows` chest if the shape rejects the window. |
| `items { }` | The item map: each key is an item id. |
| `open-requirement` | Conditions that must pass before the menu opens at all (see below). |
| `open-actions` / `close-actions` | Action lists run when the menu opens / closes. |
| `refresh { enabled, interval-ticks }` | Auto re-render on a timer. `update-interval` is the shorthand. |
| `placeholders { }` | Menu-scoped custom placeholders: `name = template`. A menu-scoped token overrides the global registry for this menu only. |
| `click-cooldown` | Anti-spam window in milliseconds; two clicks closer than this count as one. |
| `layout` / `patterns` / `fill-item` | Grid-string layout, reusable slot patterns, and the filler placed in empty slots. |
| `bottom-inventory` | Extends the canvas into the player's own 36 slots (raw slots `rows*9 …`). Chest-only by construction. |
| `chest-only` | Keep this menu on the chest path even for Bedrock viewers (see [Bedrock Forms](bedrock.md)). |
| `bedrock { }` | An explicit native Bedrock form for this menu. |
| `command { }` | Give the menu its own open-command (see below). |

## Item keys

Each entry under `items { }` is one tile, keyed by an id you choose.

| Key | Meaning |
|---|---|
| `material` | The icon. A literal (`DIAMOND_SWORD`), a `%placeholder%`, or a prefixed icon spec (`skull:`, `basehead:`, `hdb:`, `itemsadder:`, `oraxen:`, `nexo:`, `mmoitems:`). |
| `slot` / `slots` | Where it sits: a single slot, a list, or ranges. |
| `priority` | Tie-break when two items claim the same slot; higher wins. |
| `name` | Display name (MiniMessage). |
| `lore` | Lore lines (a list of MiniMessage strings). |
| `lore-mode` | How this lore combines with the base icon's own lore (`REPLACE` is the default). |
| `amount` | Stack size shown on the icon. |
| `decor { }` | Rich extras: `model-data`, `glow`, item flags, enchantments, potion/banner/trim, damage, data-components. |
| `view` | Visibility gate: hide the tile unless conditions pass (see below). |
| `update` | `true` to re-render this tile on every refresh tick. |
| `type` | Pagination role: `NONE`, `NEXT`, `PREVIOUS`, `JUMP`. |
| `list { }` | Expand one template tile across a data source (online players, worlds, …). See below. |
| `click { }` | What each click gesture does (see below). |

## List-backed items

A `list { }` block turns one tile into a whole grid: the engine asks a **source** for
entries and stamps the `template` item once per entry across the item's slots.

```hocon
players {
  slots = ["0-44"]
  list {
    source = "online-players"
    template {
      material = "skull:%entry_name%"
      name     = "<white>%entry_name%"
      click { left = ["command:msg %entry_name%"] }
    }
  }
}
```

| Key | Meaning |
|---|---|
| `source` | The data source id. |
| `template { }` | A normal item spec, stamped once per entry. |
| `page-size` | Entries per page. Omit it (or `0`) to derive the page from the item's slot count. **Paged sources only.** |
| `sorts` | The orderings the `list-sort` button cycles through. **Paged sources only.** |

Built-in sources include `online-players`, `worlds`, `self-inventory`, `self-enderchest`
and `luckperms-groups`; every module contributes its own (`kits:browse`, `warps:browse`,
`homes:invited-players`, `moderation:history`, `economy:transactions`, `worlds:list`, …).
The [Menu API](../developer/menu-api.md) registers more.

### Plain sources vs paged sources

There are two kinds, and the difference matters once a list gets long.

A **plain source** hands its whole collection over and the engine slices it into pages in
memory. That is fine for the online players or the loaded worlds.

A **paged source** is asked for **one page at a time**, off the tick thread, with the
viewer's current page, sort and filters pushed down to whatever holds the data. Only that
page comes back, so a browse over ten thousand rows costs the same as a browse over ten.
`playerwarps:browse` (behind `/pwarp`) is the reference example.

**`page-size` and `sorts` are paged-only.** Set either on a plain in-memory source and the spec is **rejected at load** with a
message naming the item. That is deliberate: a knob that silently does nothing is
worse than a loud error.

### Sorting, filtering and searching

A paged list is steered from ordinary buttons, each naming the source id it targets
(a menu may hold more than one list):

| Action | Does |
|---|---|
| `list-sort:<source>` | Advance to the next sort in `sorts`. Add `:prev` or `:reset` for the other directions. |
| `list-filter:<source>:<key>=<value>` | Set a filter. An empty value clears it. |
| `list-search:<source>:<key>` | Open a text prompt and store the typed line as that filter. |

The engine does not interpret sort or filter values, only the source does, so the
vocabulary is whatever that source documents. From the shipped player-warp browse:

```hocon
sort   { click { left = ["list-sort:playerwarps:browse"] } }
search { click { left = ["list-search:playerwarps:browse:search"] } }
mine   { click { left = ["list-filter:playerwarps:browse:scope=mine"] } }
all    { click { left = ["list-filter:playerwarps:browse:scope="] } }
```

State is per viewer and per open menu, so two players browsing the same menu sort and
filter independently.

## Asking the player to type something

Two click steps pause the action chain, ask the player for something, and resume with the
answer:

- `input:<key>` opens a text prompt; the typed line continues the chain.
- `confirm:<key>` asks for a yes/no before the rest of the chain runs.

Both are **top-level click steps**. Written inside an `else`, a `deny`, or a
per-requirement list they cannot resume anything, so they are ignored with a one-line
warning in the console rather than failing silently.

How the prompt actually appears is operator config, not menu config. `text-input.conf`
sets a global `default-mode` and per-point overrides:

| Mode | The player sees |
|---|---|
| `anvil` | A small anvil window; type in the rename field and click the result |
| `chat` | The menu closes and the next chat message is the answer |
| `sign` | A transient sign opens for editing |
| `dialog` | A native dialog screen (Minecraft 1.21.6+) |

Every prompt works in every mode, so switching one is always safe. `dialog` on an older
server falls back to `sign` (or `anvil`), logged once so the substitution is not silent.
Bedrock viewers always get a native form regardless of the mode.

## Click blocks

`click { }` binds behaviour to gestures. Each gesture is either a **bare action list**
or a **block** with `actions`, `requirements`, `minimum`, `deny` and an `else` fallback.

```hocon
click {
  left  = ["command:spawn", "close"]
  right = ["open:warps"]
}
```

The gesture keys are `left`, `right`, `shift-left`, `shift-right`, `middle`, `drop`,
`control-drop`, `double-click`, and `any` (a catch-all that fires alongside whichever
gesture was used). The underscore forms (`shift_left`, `double_click`) are accepted too.

The full grammar of actions, conditions, per-gesture requirements and `else` ladders
lives on the [Actions & Requirements](actions-requirements.md) page.

## A complete worked example

A three-row VIP shop tile: it only shows to players with a permission, charges money on
click, and gives a diamond block, with a fallback message when they cannot afford it.

```hocon
# menus/shop.conf   to   /menu open shop   (or /shop, see the command block)

title = "<gradient:#ffd700:#ff8c00>VIP Shop</gradient>"
rows = 3

# Gate the whole menu: only VIPs may open it at all.
open-requirement = ["perm:uxmessentials.vip"]

# A tidy grey border around the edges.
fill-item {
  material = GRAY_STAINED_GLASS_PANE
  name = " "
}

items {
  buy-block {
    slots = [13]
    material = DIAMOND_BLOCK
    name = "<aqua>Diamond Block"
    lore = [
      "<gray>Price: <white>500</white>",
      "",
      "<yellow>Left-click to buy."
    ]
    decor { glow = true }

    # Hide this tile entirely from anyone in the 'spawn' world.
    view {
      requirements = ["!world:spawn"]
      minimum = 1
    }

    click {
      left {
        requirements = ["has-money:500"]
        actions = [
          "take-money:500",
          "give-item:DIAMOND_BLOCK",
          "sound:BLOCK_NOTE_BLOCK_PLING",
          "message:<green>Purchased a diamond block!"
        ]
        # Runs instead of 'actions' when the requirement fails.
        deny = ["message:<red>You need 500 to buy that."]
      }
    }
  }

  close {
    slot = 22
    material = BARRIER
    name = "<red>Close"
    click { left = ["close"] }
  }
}
```

**Bare `id:value`, never brackets.** Every action, condition and placeholder is a **bare `id:value` reference**:
`message:hi`, `perm:vip`, `has-money:500`, `open:shop`. uxmEssentials does **not** use
the bracketed `[message] hi` style some other menu plugins use. If you are converting
an old file, this is the single most common thing to fix (the
[converters](converters.md) do it for you).

## Giving a menu its own command

Add a top-level `command { }` block and the menu registers its own open-command. Now
`/shop` opens `menus/shop.conf` directly: no `/menu open` needed.

```hocon
command {
  name = "shop"                       # registers /shop
  aliases = ["store", "vipshop"]      # extra literals
  permission = "uxmessentials.shop.open"
  deny-message = "<red>The shop is closed."
  console = false                     # players only
  usage = "Open the VIP shop"
}
```

`name` must be a single lowercase word. A malformed or duplicate alias is dropped rather
than aborting the command, and the menu still opens through `/menu open shop` even if the
command block is invalid. You can also declare typed positional `arguments` here for
commands like `/gift <target> <amount>`: the argument values become placeholders the
menu can read.

## The `/menu` command

`/menu` is the operator surface over the engine. `open` and `list`/`last` are for
everyone (`uxmessentials.menu.use`); the rest are admin-gated with
`uxmessentials.menu.admin`.

| Command | What it does | Permission |
|---|---|---|
| `/menu open <name> [target]` | Open a menu for yourself, or for another player | `.menu.use` (target: `.menu.open.others`) |
| `/menu list` | List every loaded menu | `.menu.use` |
| `/menu last` | Reopen the last menu you had open | `.menu.use` |
| `/menu reload [menu]` | Re-read the whole `menus/` folder, or just one spec | `.menu.admin` |
| `/menu dump <menu>` | Print a menu's parsed structure (items, slots, clicks) | `.menu.admin` |
| `/menu meta <menu>` | Print a menu's metadata summary | `.menu.admin` |

**Reload after every edit.** Menu files are read on load and on `/menu reload`, never on a hot path, so nothing
you type takes effect until you reload. `/menu reload shop` re-reads one file and
tells you in the log exactly where a syntax error is; a bad file is skipped and the
others keep working.

There are two more `/menu` subcommands covered elsewhere: `/menu convert` (see
[Converting Other Menus](converters.md)) and `/menu execute` (run a single menu action
for a player, admin-only).

Related: [Actions & Requirements](actions-requirements.md), [Bedrock Forms](bedrock.md), [Converting Other Menus](converters.md), [Menu API](../developer/menu-api.md)

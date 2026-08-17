---
title: gui.yml
order: 303
description: The layout grammar, click actions, and every menu you can re-skin.
icon: layout-dashboard
---

Every menu is described in `gui.yml`. The grammar is the same everywhere: an ASCII layout, a map of
items keyed by the characters in it.

```yaml
mainMenu:
  menu:
    title: '<gradient:#6F3B12:#E0B96A><bold>Blacksmith Menu</bold></gradient>'
    layout:
      - 'bgggggggb'
      - 'gb s a bg'
      - 'gb i p bg'
      - 'gb y   bg'
      - 'bo     ob'
      - 'bgggggggb'
    items:
      b:
        key: b
        material: BLACK_STAINED_GLASS_PANE
        displayName: '<dark_gray><bold><!i>!</!i></bold></dark_gray>'
```

## The layout grammar

Each string is one row of nine. Each character is a key into `items`. A space is an empty slot.

Rows beyond nine characters are truncated; the inventory size follows the number of rows. The same
character may appear many times — every matching slot gets that item, which is how borders are
filled with one definition.

## An item

| Key | Default | What it does |
|---|---|---|
| `key` | `x` | The layout character it fills |
| `material` | `BLACK_STAINED_GLASS_PANE` | Bukkit material |
| `displayName` | `" "` | MiniMessage or `&` codes |
| `lore` | `[]` | Lore lines |
| `customModelData` | unset | Model data |
| `forceGlowing` | `false` | Enchantment glint |
| `customItemData` | unset | A hook item instead of a material — same shape as a requirement |
| `permission` | unset | Hide the item from players without it |
| `clickActions` | `[]` | What clicking it does |

## Click actions

| Action | Effect |
|---|---|
| `[MAIN_MENU]` | Open the main menu |
| `[CATEGORIES_MENU]` | Open the category list |
| `[PERK_MENU]` | Open the perk tree |
| `[MASTERY_MENU]` | Open the mastery screen |
| `[OPEN_CATEGORY] <key>` | Open one category |
| `[NEXT_PAGE]` / `[PREVIOUS_PAGE]` | Paginate, wrapping around |
| `[CONFIRM_TRADE]` | Start the trade being viewed |
| `[DONE_TRADE]` | Claim a finished trade |
| `[CLOSE]` | Close the inventory |
| `[SOUND] <SOUND_NAME>` | Play a sound |
| `[MESSAGE] <text>` | Send a message |
| `[CONSOLE] <command>` | Run a command from console |
| `[PLAYER] <command>` | Run a command as the player |

`{player}` is substituted in `[CONSOLE]`, `[PLAYER]` and `[MESSAGE]`. Several actions may be listed
on one item and run in order.

<Callout type="danger" title="[CONSOLE] runs with full console authority">

An item with `[CONSOLE] op {player}` ops whoever clicks it. Menu items are not permission-checked
beyond the item's own `permission` key — set one on anything that runs a console command, and treat
`gui.yml` as a file only trusted staff may edit.

</Callout>

## The sections

| Section | Screen |
|---|---|
| `mainMenu` | The hub: slot icons, perks, masteries, categories |
| `categoriesMenu` | The category list |
| `defaultCategoryMenu` | The trade list inside a category |
| `chooseSlotMenu` | Pick which slot to start a trade in |
| `confirmTradeMenu` | Only used when `disableSlotModule` is true |
| `perkMenu` | The perk tree |
| `masteryMenu` | The mastery list |
| `navigation` | The shared next/previous/back row |
| `progressBar` | The characters `{progress_bar}` is drawn from |
| `generalText` | Fallback title and the unknown-requirement label |
| `tradeItemLores` | The lore template appended to every trade icon |
| `requirementLineFormat` | How one requirement line renders |
| `guiClickCooldown` | Milliseconds between accepted clicks — `400` |

A category may override `defaultCategoryMenu` with its own `guiLayout` in `config.yml`.

## Trade lore

`tradeItemLores` is the template every trade icon gets. `requirementLineFormat` renders each
requirement inside it:

```yaml
requirementLineFormat: '<dark_gray>  ▪</dark_gray> <#CCCCCC>{amount}x <white>{req_name}</white>'
```

The placeholders available in these templates are listed in
[Placeholders](../placeholders.md#gui-placeholders) — `{success_chance}`, `{time_remaining}`,
`{progress_bar}`, `{player_mastery_level}` and the rest.

## Slot positions

```yaml
mainMenu:
  tradingIconSlotsAndPermissions:
    1: { slot: 29, permission: uxmblacksmith.slot.1 }
    …
```

<Callout type="danger" title="Do not renumber the keys of this map">

The **key** — `1`, `2`, `3` — is what `pending_trades` stores as a player's slot. Changing `slot`
(the inventory position) or `permission` is safe. Changing a key orphans every pending trade in it.

</Callout>

## Click cooldown

`guiClickCooldown: 400` throttles clicks per player. Lower it if your menus feel sluggish; raise it
if you see double-starts on a laggy server.

---
title: Menus from config
order: 237
description: A menu laid out in HOCON, with named actions and conditions owned by code, plus an in-game config editor.
icon: file-cog
---

The layout, the icons and their text live in a config file. The behaviour lives in code and is
referenced by name. An operator can re-skin a menu freely and can only ever reference actions you
implemented.

## The file

```hocon
title = "<gold>Shop"
rows = 3
mask = [
  "XXXXXXXXX"
  "X   C   X"
  "XXXXXXXXX"
]
items {
  X { material = "GRAY_STAINED_GLASS_PANE", name = " " }
  C { material = "BARRIER", name = "<red>Close", action = "close" }
}
```

| Key | Meaning |
|---|---|
| `title` | MiniMessage window title |
| `rows` | Row count, inferred from the mask when absent |
| `mask` | A list of nine-character rows; each character indexes `items` |
| `items` | The legend, keyed by mask character |
| `locks` | Interaction classes the menu lets through, by name |

Each item takes `material`, optional `name` and `lore` in MiniMessage, an `amount`, an optional
`action`, and optionally `name-key` and `lore-key` to pull text from a lang file instead.

A space, or any character with no legend entry, leaves the slot empty.

## Loading it

```java
MenuActions actions = new MenuActions()
        .register("close", event -> event.getWhoClicked().closeInventory())
        .register("buy", event -> openShop(event))
        .withBack(navigator);

SimpleGui menu = MenuConfig.load(node, actions);
menu.open(player);
```

`withBack(navigator)` registers a `back` action wired to a `GuiNavigator`, since that is the one
action nearly every config menu wants.

An action name the config references but code never registered simply does nothing on click. It
cannot reach code you did not expose.

## Per-viewer states in config

An item can declare ordered named states instead of a single icon. The first state whose condition
passes for the viewer renders.

```hocon
items {
  S {
    material = "PLAYER_HEAD"
    states {
      online  { condition = "is-online",  material = "LIME_DYE", name = "<green>Online" }
      offline { condition = "always",     material = "GRAY_DYE", name = "<gray>Offline" }
    }
  }
}
```

```java
MenuConditions conditions = new MenuConditions()
        .register("is-online", ctx -> isOnline(ctx.effectivePlayer()));

SimpleGui menu = MenuConfig.load(node, actions, conditions);
```

An `always` condition is registered out of the box for the catch-all last state.

A state only repeats the fields that differ from the item's base icon. Everything else is inherited,
which is why the example above does not repeat `PLAYER_HEAD` in each state unless it wants to
override it.

<Callout type="warning" title="A menu with states is single-viewer">

Config states compile down to a stateful `GuiItem`, so the menu resolves per viewer. Load a fresh
instance per player, or open it through a `GuiNavigator`.

</Callout>

## Translated menus

Pass a `MessageCatalog` and an icon can draw its text from a lang file by key:

```hocon
items {
  B { material = "EMERALD", name-key = "shop.buy.name", lore-key = "shop.buy.lore", action = "buy" }
}
```

```java
SimpleGui menu = MenuConfig.load(node, actions, conditions, catalog);
```

Translation then lives in the lang file and the layout in the menu file, so a translator never has to
open a menu config. An inline `name` still works with no catalog present, so a menu needs a lang file
only when it references one.

## Config migration

`MenuConfig.load` runs the tree through `MenuConfig.migrate` first, which renames keys that older
releases used to their current names, in place. A rename only fires when the legacy key is present
and the current one is not, so a file already on the current names is untouched and a file carrying
both is left alone.

Call `MenuConfig.migrate(node)` yourself if you want to migrate a tree once and write it back to disk
rather than migrating it on every load.

## The in-game config editor

`ConfigEditorGui` browses a `HoconConfig`'s keys as a paginated menu and lets an admin retype a
scalar.

```java
new ConfigEditorGui(config, playerInput).open(admin);
new ConfigEditorGui(config, playerInput, InputType.ANVIL, "homes").open(admin);
```

A section opens a child editor one level deeper. A scalar opens a prompt; the typed value is coerced
to the key's existing type, validated, set and saved before the menu refreshes.

`ConfigValueEditor` is that coercion on its own, if you want it without the menu:

```java
Object coerced = new ConfigValueEditor(config).setAndSave("homes.limit", "5");
```

This is worth having on servers where opening a file is a support burden. It is not a replacement for
a config file, and the icons snapshot the config at build time, so reopen the editor after an
external reload.

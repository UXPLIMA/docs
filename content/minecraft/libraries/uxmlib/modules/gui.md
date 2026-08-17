---
title: uxmlib-gui
order: 23
description: Menu types, per-viewer items, navigation, text input and menus from config.
icon: layout-grid
---

An inventory-menu framework. Depends on `uxmlib-item`.

## Installing

```java
Guis.install(plugin, scheduler);
```

Once, in `onEnable`. The `Scheduler` overload enables animated and auto-refreshing items; the
overload without it works with those inert.

## A menu

```java
SimpleGui menu = Guis.gui().title(Text.mini("<dark_aqua>Menu")).rows(3).build();

menu.filler().fillBorder(GuiItem.display(pane));
menu.set(2, 5, GuiItem.button(icon, event -> click()));
menu.onClose(event -> persist());
menu.open(player);
```

Positions are 1-indexed row and column. `filler()` offers border, row, column, rect and full fills.

<Callout type="tip" title="Clicks are cancelled by default">

An unconfigured menu can never leak items. Interaction is something you opt into — with a
`StorageGui`, or by allowing it through `GuiInteractions` — rather than something you have to
remember to prevent.

</Callout>

## Menu types

| Builder | Shape |
|---|---|
| `Guis.gui()` | A fixed chest menu |
| `Guis.paginated()` | Pages, with `addPageItem` |
| `Guis.scrolling(ScrollType)` | Scrolls horizontally or vertically |
| `Guis.storage()` | Real items, take and place allowed, contents kept across opens |
| `Guis.typed(GuiType)` | Non-chest inventories — hopper, dispenser, dropper, and the rest |

```java
PaginatedGui shop = Guis.paginated().title(Text.mini("Shop")).rows(6).build();
products.forEach(p -> shop.addPageItem(GuiItem.button(p.icon(), e -> buy(p))));

ScrollingGui list = Guis.scrolling(ScrollType.VERTICAL).rows(4).build();

StorageGui vault = Guis.storage().rows(3).build();
vault.onClose(e -> save(vault.contents()));
```

Paginated is the right choice for a list whose length you do not know. Scrolling suits a grid that
should feel continuous — a warp map, a skin picker.

## Items

| Kind | Behaviour |
|---|---|
| `GuiItem.display(stack)` | Static, no click |
| `GuiItem.button(stack, handler)` | Static, with a click handler |
| `GuiItem.dynamic(ctx -> stack)` | Computed per viewer, per render |
| `GuiItem.stateful()` | The first state whose predicate matches |
| `GuiItem.animated(frames, interval)` | Cycles frames |

```java
menu.set(4, GuiItem.dynamic(ctx -> headOf(ctx.viewer())));

menu.set(5, GuiItem.stateful()
        .display(ctx -> ctx.viewer().hasPermission("vip"), vipIcon)
        .display(ctx -> true, normalIcon)
        .build());

menu.set(6, GuiItem.animated(List.of(frame1, frame2), Duration.ofMillis(250)));
```

`dynamic` and `stateful` are per-viewer: one menu instance can be open for twenty players and show
each of them something different. `stateful` picks the **first** matching state, so order the
predicates from most specific to a `true` fallback.

## Navigation

```java
GuiNavigator nav = new GuiNavigator();
nav.open(player, mainMenu);
subMenu.set(8, GuiItem.back(nav, backArrow));
```

A back-stack across screens, so a back button does not have to know which menu it came from.

## Text input

```java
AnvilInput.prompt(player, Text.mini("Enter a name"), result -> rename(result.text()));
```

Anvil, chat and sign input behind one `InputType` contract, routed by `InputRouter`. Switching a
prompt from an anvil to a sign is a parameter, not a rewrite — which matters because each has
different failure modes on different clients.

## Menus from config

```java
MenuActions actions = new MenuActions().register("buy", e -> openShop(e));
SimpleGui fromFile = MenuConfig.load(configNode, actions);
```

The layout, the items and their names live in HOCON; the code owns the actions and registers them by
name. Operators re-skin the menu freely and can only ever reference actions you implemented.

`MenuConditions` gates items on conditions, and `MenuConfigMigration` moves older menu files forward.

## Dialogs

`DialogScreen` and `DialogInputScreen` are a facade over Paper's server-side Dialogs — a native
confirmation or form, not an inventory pretending to be one.

`ConfirmMenu` is the inventory-based equivalent when you want the same idea in a chest.

## Config editing

`ConfigEditorGui` and `ConfigValueEditor` build a menu that edits a config node in game. Useful for
letting an owner change settings without opening a file, on servers where that is a support burden.

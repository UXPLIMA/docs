---
title: uxmlib-gui
order: 23
description: "An inventory-menu framework: five menu types, per-viewer items, a safe-by-default click policy, navigation, text input and menus from config."
icon: layout-grid
---

An inventory-menu framework built on the public Paper API. No packets, no NMS. Depends on
`uxmlib-item`.

The module is deliberately larger than a menu builder. A menu that survives contact with real
players has to answer questions a builder alone does not: what happens when two hundred people open
the same shop, what happens when a player shift-spams a button, what happens when the icon changed
between the render and the click, what happens when the click needs a database round trip. Each of
those has a named answer here.

## Installing

```java
Guis.install(plugin, scheduler);
```

Once, in `onEnable`. This registers the single `GuiListener` that routes every click, drag, open,
close and quit to the right menu.

The `Scheduler` overload is what enables animated items and auto-refreshing menus, and it is what
lets an asynchronous click handler marshal its result back onto the viewer's region thread. The
overload without a scheduler works, with those three features inert.

```java
Guis.uninstall();          // in onDisable
Guis.isInstalled();        // whether install() has run
Guis.clickLog();           // the rolling click log, or null when not installed
```

<Callout type="warning" title="One install per plugin, not per menu">

`Guis.install` is idempotent per plugin but the listener it registers is owned by the plugin you
pass. Calling it from a second plugin gives that plugin its own listener and its own click debounce
table, which is what you want. Calling it twice from the same plugin is not.

</Callout>

## A first menu

```java
SimpleGui menu = Guis.gui()
        .title(Text.mini("<dark_aqua>Menu"))
        .rows(3)
        .build();

menu.filler().fillBorder(GuiItem.display(pane));
menu.set(2, 5, GuiItem.button(icon, event -> click()));
menu.onClose(event -> persist());
menu.open(player);
```

`set(int row, int col, ...)` is 1-indexed. `set(int slot, ...)` takes a raw slot when you want one.

<Callout type="tip" title="Clicks are cancelled by default">

An unconfigured menu can never leak items. Every interaction class starts denied and you opt in, per
class, with `allow(...)`. A `StorageGui` opts into take and place for you. Nothing is left to
remembering to cancel an event.

</Callout>

## Builder options

Every builder except `typed()` shares these:

| Method | Effect |
|---|---|
| `title(Component)` | The window title, MiniMessage through `Text.mini` |
| `rows(int)` | 1 to 6 rows of nine slots |
| `allow(InteractionModifier...)` | Opt into interaction classes |
| `apply(Consumer<Gui>)` | Run a block against the menu at build time |
| `autoRefresh(Duration)` | Re-resolve every item on a timer while open |
| `clickSound(Sound)` | Feedback sound on an accepted click |
| `openSound(Sound)` | Feedback sound when the menu opens |

`Guis.paginated()` adds `contentSlots(List<Integer>)` to choose which slots hold page content.
`Guis.typed(GuiType)` takes only a title, because its shape fixes its size.

## What the whole module holds

| Page | Covers |
|---|---|
| [Menu types](menu-types.md) | Simple, paginated, scrolling, storage, typed; the shared `Gui` surface |
| [Items](items.md) | Static, dynamic, stateful and animated icons; render context; display modifiers |
| [Clicks and safety](clicks.md) | The cancel policy, interaction classes, debounce, the anti-desync re-check, declarative and async handlers |
| [Layout and animation](layout.md) | Fillers, masks, adaptive slot layouts, slot patterns |
| [Navigation](navigation.md) | The back-stack, bound menus, confirmations |
| [Text input](input.md) | Anvil, chat and sign prompts behind one contract |
| [Menus from config](config-menus.md) | HOCON layouts, named actions and conditions, the in-game config editor |
| [Dialogs](dialogs.md) | Paper's native server-side dialogs |

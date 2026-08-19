---
title: Navigation
order: 235
description: A back-stack across screens, menus bound to a domain object, and confirmation prompts.
icon: route
---

## The back-stack

`GuiNavigator` remembers which menu a player came from, so a back button does not have to.

```java
GuiNavigator nav = new GuiNavigator();
nav.install(plugin);          // once, so the stack clears on quit

nav.open(player, mainMenu);   // push and open
subMenu.set(8, GuiItem.back(nav, backArrow));
```

| Method | Effect |
|---|---|
| `open(player, gui)` | Push the current screen and open a new one |
| `openRoot(player, gui)` | Clear the stack and open this as the new root |
| `back(player)` | Pop to the previous screen; `false` when there is none |
| `canGoBack(player)` | Whether a back button should be shown |
| `current(player)` | The screen the player is on, or `null` |
| `clear(player)` | Forget this player's stack |

`openRoot` is what a top-level command should call. `open` is what a button inside a menu should
call. Getting that split right is what stops a back button walking a player through six screens of
history they did not mean to keep.

<Callout type="tip" title="The navigator opens a fresh instance per player">

Which means a menu with dynamic or stateful items behaves correctly through navigation without you
managing instances. See the viewer model in [Menu types](menu-types.md).

</Callout>

## Menus bound to an object

A "delete home X?" menu needs to know which `X` it is acting on. `Linked<T>` carries the object
through render and re-render.

```java
Linked<Home> editor = Linked.of(gui, (menu, home) -> {
    menu.clear();
    menu.set(13, GuiItem.button(home.icon(), e -> delete(home)));
    menu.set(15, GuiItem.button(renameIcon, e -> rename(home)));
});

editor.bind(selectedHome).open(player);
```

| Method | Effect |
|---|---|
| `bind(T)` | Swap the bound value and re-render |
| `value()` | The current value, or `null` |
| `rerender()` | Re-run the renderer against the current value |
| `gui()` | The backing `SimpleGui` |
| `open(viewer)` | Open it |

One menu instance then serves a whole list of objects rather than being rebuilt for each. The
renderer owns clearing and laying out the slots, which is why it usually starts with `clear()`.

## Confirmations

An inventory-based yes or no.

```java
ConfirmMenu.of(Text.mini("<red>Delete this home?"), confirmed -> {
    if (confirmed) delete(home);
}).open(player);
```

```java
ConfirmMenu.of(Text.mini("<red>Delete?"), () -> delete(home), () -> reopen(player)).open(player);
```

The builder is there when the default icons do not suit:

```java
ConfirmMenu.builder(Text.mini("<red>Delete?"))
        .confirmIcon(ItemBuilder.of(Material.LIME_CONCRETE).name(Text.mini("<green>Yes")).build())
        .cancelIcon(ItemBuilder.of(Material.RED_CONCRETE).name(Text.mini("<red>No")).build())
        .onResult(confirmed -> handle(confirmed))
        .build()
        .open(player);
```

`gui()` exposes the backing `SimpleGui` if you want to decorate it further.

For the native, non-inventory equivalent, see [Dialogs](dialogs.md). A dialog is the better choice
when the question is a question, and a `ConfirmMenu` is the better choice when it should feel like
part of the menu the player is already in.

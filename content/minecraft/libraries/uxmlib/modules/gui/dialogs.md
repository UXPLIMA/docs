---
title: Dialogs
order: 238
description: A facade over Paper's native server-side dialogs, for notices, confirmations and single-line text input.
icon: message-square
---

A dialog is a real client screen, not an inventory pretending to be one. Paper added server-side
dialogs in Minecraft 1.21.6, and this is a thin fluent facade over them.

## Notices and confirmations

```java
DialogScreen.notice(Text.mini("<gold>Server restart"))
        .body(Text.mini("<gray>The server restarts in five minutes."))
        .button(Text.mini("<green>Understood"), audience -> {})
        .show(player);
```

```java
DialogScreen.confirmation(Text.mini("<red>Delete this home?"))
        .body(Text.mini("<gray>This cannot be undone."))
        .yes(Text.mini("<green>Delete"), audience -> delete(home))
        .no(Text.mini("<gray>Keep"), audience -> {})
        .show(player);
```

`body` may be called several times, once per line. `build()` returns the raw Paper `Dialog` if you
want to show it yourself.

## Single-line text input

```java
DialogInputScreen.create(Text.mini("<gold>Rename"), "name", Text.mini("New name"))
        .prompt(player, typed -> rename(home, typed), () -> reopenMenu(player));
```

The three arguments are the window title, the key the typed line is read back under, and the label
shown beside the field. Submit runs the first callback with the typed line; cancel runs the second.
Closing with escape runs neither.

The field defaults to 128 characters and 200 pixels wide, both adjustable with the fluent setters,
along with the initial value and the submit and cancel button labels.

## Version gating

The Dialog API does not exist before 1.21.6.

```java
DialogScreen.isSupported();
DialogInputScreen.isSupported();
```

Both facades degrade rather than explode. `DialogScreen.show` is a no-op on an older server and never
throws. `DialogInputScreen.prompt` runs the cancel callback instead of touching the absent API. Only
`build(...)` throws, because there is nothing to build.

So a call site does not have to guard, but a good one still does: if dialogs are unsupported you
probably want the anvil or sign path from [Text input](input.md) rather than silently nothing.

## Threading

Paper delivers a dialog action on the main server thread, which is where every other dialog button
callback runs, so your callbacks run there too. The facade never schedules. Do not block a callback:
hop to async yourself for any I/O the answer drives.

## Dialog or inventory

| Question | Use |
|---|---|
| A confirmation that should feel native and unmistakable | `DialogScreen.confirmation` |
| A confirmation inside a menu flow the player is already in | `ConfirmMenu` |
| One typed value, server on 1.21.6 or newer | `DialogInputScreen` |
| One typed value, older server or mixed client base | `PlayerInput` |

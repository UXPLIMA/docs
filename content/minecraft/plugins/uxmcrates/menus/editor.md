---
title: Editor
order: 511
description: The editor windows, their slots, and how each button writes a value.
icon: pencil
---

`/uxmcrates editor` (`uxmcrates.admin.editor`). See [The editor](../features/editor.md) for what it writes.

## Home: editor-root.conf

Three rows.

| Slot | Tile | Click |
|---|---|---|
| 11 | Chest, `The crates` | The crate list of the editor |
| 15 | Tripwire hook, `The keys` | The key list of the editor |

## Crates: editor.conf

| Slot | Tile | Click |
|---|---|---|
| 10 to 16, 19 to 25, 28 to 34 | One crate each | Left: its settings. Right: its rewards |
| 45 | Writable book | A new crate: type its id |
| 48, 50 | Arrows | Pages |
| 49 | Feather | Home |

## A crate: editor-crate.conf

Every setting is a button in `property-slots` (10 to 16, 19 to 25, 28 to 34, 37 to 43). `back-slot` 49 returns to the
list, `delete-slot` 53 deletes the crate after a confirmation.

## A crate's rewards: editor-reward-list.conf

| Slot | Does |
|---|---|
| 10 to 16, 19 to 25, 28 to 34 | One reward each, with its chance and what it gives. Click to edit it |
| 48, 50 | Pages |
| 53 | Makes a reward of the item in your hand |
| 45 | Back |

## A reward: editor-reward.conf

Settings in 10 to 16, 19 to 25, 28 to 34; back at 49; delete at 53.

## Keys and a key: editor-keys.conf, editor-key.conf

The key list has a tile per key (left click edits it), a writable book at 45 for a new key, arrows at 48 and 50 and
back at 49. A key's settings are in 10 to 16, 19 to 25, 28 to 34, back at 49 and delete at 53.

## A list of lines: editor-list.conf

Lore, commands, money, cost, requirements and effects open here, one line per slot in 10 to 16, 19 to 25, 28 to 34.

| Gesture | Does |
|---|---|
| Shift left click a line | Write it again |
| Shift right click a line | Take it out, after a confirmation |
| Slot 53 | Write another line |
| Slot 45 | Back |

## Buttons

| Kind | Click |
|---|---|
| A toggle | Steps through the values |
| A value | Asks you to type the new value |
| A list | Opens the list window |
| The item | Takes the item in your hand |

Related: [The editor](../features/editor.md), [Menu files](../config/menu-files.md)

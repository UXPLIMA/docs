---
title: In-game editor
order: 414
description: Making, changing and deleting minion types from inside the game.
icon: pencil
---

`/uxmminions editor` (`uxmminions.admin.editor`) opens a [window](../menus/editor.md) with every type and a button that
makes another. A new type starts as the shipped template, comments and all.

| Button | Writes |
|---|---|
| Name | `display-name.en` |
| Job | `job` |
| Icon | `icon` |
| Interval | `action-interval` |
| Targets | `targets`, in a list window |
| Blacklist | `blacklist`, in a list window |
| Effects | `effects`, in a list window |

The levels have no button, on purpose: a level is ten named fields, a requirement list and a reward list, and one button
that flattened it would lie about the file. They stay in the file.

Every save writes the file and reads the types again; comments survive, keys come back in alphabetical order. The
barrier deletes a type's file. `editor.enabled = false` registers no editor windows and no input listener; it takes a
restart.

Related: [Editor windows](../menus/editor.md)

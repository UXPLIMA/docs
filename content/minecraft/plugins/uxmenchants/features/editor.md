---
title: In-game editor
order: 412
description: Making, changing and deleting enchantments from inside the game, and what the editor writes.
icon: pencil
---

`/uxmenchants editor` (`uxmenchants.admin.editor`) opens a [window](../menus/editor.md) with every enchantment and a
button that makes another.

## Making one

Click the lime dye and type an id in the anvil, chat or sign prompt. The id is the file name: letters, digits and
hyphens (`editor.bad-id` otherwise). A new enchantment starts as the shipped template, comments and all, with one
working effect, and its property editor opens. An id a file already carries answers `editor.exists`.

## The properties

Each button writes one line of the enchantment file:

| Button | Writes | How |
|---|---|---|
| The name a player reads | `name.en` | Typed |
| What it does, in one sentence | `description.en` | Typed |
| How hard it is to find | `rarity` | Picked from `rarities.conf` |
| How it is drawn | `type` | Picked from `types.conf` |
| The highest level any route hands out | `max-level` | A number, 1 to 100; shift steps by ten |
| How far an anvil may push it | `overpower-max` | A number, 1 to 100 |
| The node a player needs | `permission` | Typed |
| What it goes on | `targets` | A list window |
| The items an anvil takes | `anvil-items` | A list window |
| The items a table offers it on | `table-items` | A list window |
| What it will not sit beside | `conflicts` | A list window |
| A way of finding it | `obtaining.<route>` | One toggle per route: table, villager, loot, anvil, fishing, mob-drops, raids |

A blank line clears the key rather than writing an empty one. Effects are not on a button; they stay in the file.

Every save writes the file and reads every content file again, so the next draw shows it. Comments survive a save, but
the keys come back in alphabetical order: that is how the configuration library writes a file.

## Deleting

The barrier deletes the file after a second click, and says `editor.deleted`.

## Off

`editor.enabled = false` is for a server that keeps its content in version control. The editor's windows are not
registered, no input listener is installed, and the command says `editor.off`. It takes a restart.

Related: [Editor windows](../menus/editor.md), [Enchantment files](enchantment-files.md)

---
title: Overview
order: 501
description: How a window is written, tiles, lists and Bedrock forms.
icon: info
---

Every window is a file under `menus/`. Slots, materials, titles, lore and sounds are read from the file, never decided
in code: move a slot, run `/is reload`, and the slot has moved. A release that restyles a window updates the copy on
your server only if you never edited it.

| Key | What it sets |
|---|---|
| `title` | The window title: a language key written with a leading `@`, or your own words |
| `rows` | One to six |
| `fill-item` | The pane behind every free slot |
| `items.<name>.slot`, `slots` | Where an item sits; a range is `10-16`, and a list mixes both |
| `items.<name>.material`, `name`, `lore` | What it looks like |
| `items.<name>.click.any`, `left`, `right` | What a click does, as a list of actions |
| `items.<name>.list` | A tile for each entry of a list, such as members or warps |
| `open-actions` | What happens when the window opens |

## Tiles

Most items are tiles: a title, the category under it, a description, the facts and the line that says what a click
does. A tile is written as a blank name and one lore line, `tile:<colour> @<catalogue block> <facts>`. The words come
from the language files, so a tile reads in each player's language.

## Actions

| Action | What it does |
|---|---|
| `open:<menu>` | Opens another window |
| `close` | Closes the window |
| `command:<line>` | Runs a command as the player |
| `message:<line>` | Sends a line |
| `sound:<name> <volume> <pitch>` | Plays a sound |
| `skyblock:island:<word>` | Runs `/is <word>` |
| `skyblock:...` | The verbs this plugin adds, named in each window's file |

## Bedrock players

With floodgate installed, a Bedrock player sees forms in place of chest windows where a form reads better, such as
starting an island and the shop. See [Integrations](../integrations/overview.md).

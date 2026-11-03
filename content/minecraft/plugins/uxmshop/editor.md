---
title: "The editor"
order: 40
description: "Changing the catalogue and the menu tiles from in game with /shopedit."
---

`/shopedit` changes the catalogue and the position of menu tiles from in game. It writes
`shops/<section>.conf` and `menus/<window>.conf`, and never `config.conf`. It is registered only while
`editor.enabled` is `true` in `config.conf`, every subcommand needs `uxmshop.editor`, and it can be renamed
in `commands.conf` like any other command.

## What the shop sells

| Command | What it does |
|---|---|
| `/shopedit section <name>` | Starts a section, with the item in your hand as its icon. |
| `/shopedit add <section> <line> <buy> <sell>` | Adds the item in your hand to a section. |
| `/shopedit price <section> <line> <buy> <sell>` | Changes what a line charges. |
| `/shopedit match <section> <line> <mode>` | Changes how strictly a line matches. |
| `/shopedit remove <section> <line>` | Takes a line out. |

## Where a tile sits

| Command | What it does |
|---|---|
| `/shopedit tiles <window>` | Names the tiles of a window and the slots they hold. |
| `/shopedit move <window> <tile> <slot>` | Moves a tile to another slot. |
| `/shopedit icon <window> <tile>` | Draws a tile from the item in your hand. |

The window is one of the windows in `menus/` and completion offers them. The tile is a name from
`/shopedit tiles`.

## What the editor refuses

- A tile that draws into more than one slot is not moved, because one slot cannot hold it. Give it other
  slots in a text editor.
- The material of a tile the plugin fills, such as `sections` in `menus/hub.conf`, is not set, because it is
  never drawn.

What a tile does, which facts its tile names and how many rows a window has stay text. Those are the grammar
of the window.

## What a save costs

A save rewrites the file from the model. The block of comments at the top of a shipped file is written again
on every save, so the file still explains itself. These do not survive:

- A note you wrote beside your own line or your own tile.
- Your key order; the writer has its own.
- A `fill-item` block. It comes back as an ordinary item under `__fill__`, holding the slots it claimed.
- An `update-interval` and a `pattern` with `vars`. The first comes back as the `refresh` block it means, the
  second already written out.

Nothing else is dropped: the window you get back is the window you had. Write your own notes somewhere the
editor does not rewrite.

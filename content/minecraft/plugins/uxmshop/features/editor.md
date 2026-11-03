---
title: The editor
order: 310
description: Changing the catalogue and window tiles in game, what it refuses and what a save costs.
icon: pencil
---

`/shopedit` changes the catalogue and where window tiles sit, from in game. It writes `shops/<section>.conf` and
`menus/<window>.conf`, never `config.conf`. It is registered only while `editor.enabled` is true, every branch needs
`uxmshop.editor`, and it can be renamed in `commands.conf`. See [Editor commands](../commands/editor.md).

## What it refuses

- A tile that draws into more than one slot is not moved; give it other slots in a text editor.
- The material of a tile the plugin fills, such as `sections` in `menus/hub.conf`, is not set, because it is never
  drawn.

What a tile does, which facts it names and how many rows a window has stay text: they are the grammar of the window.

## What a save costs

A save rewrites the file from what the plugin holds. The comment block at the top of a shipped file is written again on
every save, so the file still explains itself, and a window file gains a second block saying what the save cost. These
do not survive:

- A note you wrote beside your own line or tile.
- Your key order.
- A `fill-item` block; it comes back as an ordinary item under `__fill__`, holding the slots it claimed.
- An `update-interval`, which comes back as the `refresh` block it means, and a `pattern` with `vars`, written out.

Write your own notes somewhere the editor does not rewrite.

Related: [Sections and lines](sections-and-lines.md)

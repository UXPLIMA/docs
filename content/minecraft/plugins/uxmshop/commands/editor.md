---
title: Editor commands
order: 305
description: Every /shopedit branch.
icon: pencil
---

Registered only while `editor.enabled` is true in `config.conf`. Every branch needs `uxmshop.editor`.

| Command | Does |
|---|---|
| `/shopedit` | Says what the editor is for |
| `/shopedit section <name>` | Starts a section, with the item in your hand as its icon; players only |
| `/shopedit add <section> <line> <buy> <sell>` | Adds the item in your hand to a section; players only |
| `/shopedit price <section> <line> <buy> <sell>` | Changes what a line charges |
| `/shopedit match <section> <line> <mode>` | Changes how strictly a line matches |
| `/shopedit remove <section> <line>` | Takes a line out |
| `/shopedit tiles <window>` | Names the tiles of a window and the slots they hold |
| `/shopedit move <window> <tile> <slot>` | Moves a tile to another slot |
| `/shopedit icon <window> <tile>` | Draws a tile from the item in your hand; players only |

Related: [The editor](../features/editor.md)

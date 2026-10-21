---
title: Getting started
order: 10
description: Install it, give the colours out, and change the picker.
icon: rocket
---

## Install

Drop the jar into `plugins/` and start the server. The plugin writes its own folder on the first
boot: `config.conf`, `commands.conf`, the language files under `messages/` and the picker under
`menus/`. Nothing else is needed and nothing else has to be installed.

The requirements are the server itself: Paper 26.2 and Java 25. Folia is supported, and every
piece of work the plugin does is already on the thread that owns the player or the region it
belongs to.

## Give the colours out

A new player may open the picker and sees every colour, and may use the ones you granted. One
node per colour, so a rank can carry two colours and another rank twelve:

```
uxmglow.colour.red
uxmglow.colour.aqua
uxmglow.cycle
```

There is no list of ranks anywhere in the plugin. It reads the node a player holds, and you
decide which rank holds which node.

## Change the picker

`menus/glow.conf` is the window: the rows, the slot every colour sits in, the material of each
square, the sound a click makes, and which lines of the language file each tooltip reads. Edit
the file and run `/uxmglow reload`.

Delete the file and this server has no picker: `/glow` answers in chat instead and every other
command still works. A window is a file, so an operator can take one away.

## What a player types

`/glow` opens the picker. `/glow set red` skips it. `/glow cycle red,blue,gold 400` steps through
three colours every 400 milliseconds, `/glow off` stops, and `/glow colours` lists what they may
use. The full list is on the [commands](commands.md) page.

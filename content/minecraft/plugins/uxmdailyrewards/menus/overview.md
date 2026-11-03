---
title: Overview
order: 501
description: The one window, what opens it, Bedrock players, and the click sounds.
icon: mouse-pointer-click
---

uxmDailyRewards has one window, the calendar. It is a file, `menus/calendar.conf`: see
[Menu files](../config/menu-files.md) to change it.

| Command | Window | File | Permission |
|---|---|---|---|
| `/dailyrewards` | [Calendar](calendar.md) | `menus/calendar.conf` | `uxmdailyrewards.use` |

The window reads the player's progress from the database as it opens. A read that fails answers `menu.unavailable`
instead of a window, and a missing file answers `menu.missing`.

## Bedrock players

A player who joins through Geyser or Floodgate gets the calendar as a Bedrock form: a button per tile, in the same
order. Nothing needs setting up.

## Sounds

The window plays the book page sound when it opens, from its own `open-actions`. A click on the ready day plays its
own pling; the page arrows play the shared page sound, and a refused click the denied sound. The three shared sounds
are under `menu.sounds` in [`config.conf`](../config/config-conf.md#menu) and are read again on reload.

## Conventions

- **A glowing day is a live one**: the ready day, and the days whose file says `glow = true`.
- **Words come from the messages files.** Every title and tooltip is a line under `menu` in the
  [messages files](../config/messages.md), in the viewer's language, drawn in the house shape: a title, a crumb, a
  description, details and the click line.

Related: [Calendar window](calendar.md), [Menu files](../config/menu-files.md)

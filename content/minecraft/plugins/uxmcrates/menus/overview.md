---
title: Overview
order: 401
description: Every window uxmCrates draws, its file, what opens it, and the shape every tile shares.
icon: layout-dashboard
---

Every window is a file under `menus/`, so its title, size, slots, materials, sounds and clicks are yours to change.
See [Menu files](../config/menu-files.md).

| Window | File | Rows | Opened by |
|---|---|---|---|
| [Crate list](crates.md) | `crates.conf` | 6 | `/crates` |
| [Preview](preview.md) | `preview.conf` | 6 | `/crates preview`, a left click on a crate |
| [Keys](keys.md) | `keys.conf` | 6 | `/crates keys`, the tripwire hook in the crate list |
| [Waiting rewards](pending.md) | `pending.conf` | 6 | `/crates claim`, the minecart in the crate list |
| [Milestones](milestones.md) | `milestones.conf` | 6 | `/crates milestones [crate]` |
| [Cost](cost.md) | `cost.conf` | 3 | `/crates cost <crate>` |
| [Bulk summary](bulk.md) | `bulk.conf` | 6 | `/crates massopen <crate>` |
| [Reroll](reroll.md) | `reroll.conf` | 3 | A reveal, for a player with a reroll to spend |
| [Log](log.md) | `log.conf` | 6 | `/uxmcrates log` from a player |
| [Editor](editor.md) | `editor-root.conf` and seven more | 3 or 6 | `/uxmcrates editor` |

The reveal and pick windows are not menu files: an [animation file](../config/animation-files.md) lays out its own,
and `opening.window` sets their panes.

## A tile

Every tile that stands for something draws the house tooltip:

| Part | Example |
|---|---|
| Title | The crate, key or reward name, in the colour the window file gives that kind of tile |
| Crumb | What the tile is: `Crate`, `Reward`, `Key`, `Waiting` |
| Description | One sentence under the description header |
| Details | Facts as rows, such as `Keys you hold 3` |
| Action | What a click does, such as `Right click to open.` |

The words are in the `menu` block of the [messages files](../config/messages.md), so every window reads in the
player's language.

## Sounds

Each window plays its own `open-actions` sound when it opens. A button that does something plays
`menu.sounds.click`, a page arrow `menu.sounds.page` and a refused click `menu.sounds.denied`, from
[config.conf](../config/config-conf.md#menu). A button whose own actions name a sound plays that one alone.

## Bedrock

With Geyser or Floodgate, a Bedrock player gets each window as a native form.

Related: [Menu files](../config/menu-files.md), [Messages](../config/messages.md)

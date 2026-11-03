---
title: Overview
order: 401
description: The thirteen window files, who opens them, and the shared shape.
icon: layout-template
---

| Window | File | Opened by | Rows |
|---|---|---|---|
| [Job board](jobs.md) | `menus/jobs.conf` | `/jobs` | 5 |
| [One job](job.md) | `menus/job.conf` | `/jobs menu <job>`, a left click on the board | 6 |
| [Levels](levels.md) | `menus/levels.conf` | The bottle in one job | 6 |
| [Leave](quit.md) | `menus/quit.conf` | The barrier in one job, a right click on a held job | 3 |
| [Leaderboard](top.md) | `menus/top.conf` | `/jobs board` | 6 |
| [Boosters](boosters.md) | `menus/boosters.conf` | `/jobs boosts` | 3 |
| [Contracts](contracts.md) | `menus/contracts.conf` | `/contracts menu` | 5 |
| [Contract](contract.md) | `menus/contract.conf` | A click on a contract | 5 |
| [Shop](shop.md) | `menus/shop.conf` | `/jobs shop` | 5 |
| [Report](report.md) | `menus/report.conf` | `/uxmjobs report menu` | 6 |
| [Editor](editor.md) | `menus/editor.conf` | `/uxmjobs editor` | 6 |
| [Property editor](editor.md#properties) | `menus/editor-job.conf` | A click on a job in the editor | 6 |
| [Editor lists](editor.md#lists) | `menus/editor-list.conf` | A list button of the editor | 6 |

Every window is drawn from its file, with gray panes behind every free slot and a tile tooltip on every square (a blank
name and one `tile:` lore line). A Bedrock player through Floodgate gets a native form built from the same file.

Every window is a file; see [Menu files](../config/menu-files.md). The sounds of every window are `menu.sounds` in
`config.conf`.

Related: [Menu files](../config/menu-files.md)

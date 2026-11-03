---
title: Overview
order: 401
description: Every window, its file, its size and what opens it.
icon: layout-grid
---

Every window is a file in `menus/`. Move a slot, change a material, take a button out, and the plugin draws what you
wrote; see [Menu files](../config/menu-files.md). The words are in the language files under `menu.*`, so a player reads
the window in their own language.

| Window | File | Rows | Opened by |
|---|---|---|---|
| [Panel](panel.md) | `panel.conf` | 3 | `/plot` on a plot |
| [Flags](panel.md#flags) | `flags.conf` | 6 | The panel's flags button |
| [Members](panel.md#members) | `members.conf` | 6 | `/plot members`, the panel |
| [Member](panel.md#member) | `member.conf` | 3 | A member on the members window |
| [Gallery](gallery.md) | `gallery.conf` | 6 | `/plot browse` |
| [Boards](gallery.md#boards) | `board.conf` | 6 | `/plot leaderboard` |
| [Guestbook](community.md#guestbook) | `guestbook.conf` | 6 | `/plot guestbook` |
| [Sessions](community.md#sessions) | `sessions.conf` | 6 | `/plot sessions` |
| [Shop](shop.md) | `shop.conf` | 6 | `/plot shop`, the panel's decoration button |
| [Contests](contests.md) | `contest.conf` | 6 | `/contest` |
| [Scorecard](contests.md#scorecard) | `scorecard.conf` | 3 | `/contest vote` alone |
| [Hall of fame](contests.md#hall-of-fame) | `hall.conf` | 6 | `/contest hall`, the contests window |
| [Report queue](staff.md) | `staff.conf` | 6 | `/uxmplots reports` |
| [Inactive plots](staff.md#inactive) | `inactive.conf` | 6 | `/uxmplots inactive`, the queue |
| [Staff bench](staff.md#bench) | `bench.conf` | 3 | `/uxmplots staff`, the queue |

A Bedrock player gets each window as a form through Floodgate. A list window keeps its bottom row for paging: the
previous arrow at 45 and the next at 53, drawn only when there is a page to go to.

Related: [Menu files](../config/menu-files.md)

---
title: Panel, members and flags
order: 402
description: The plot panel, the flag switches, the member list and one member's grades.
icon: layout-dashboard
---

## Panel

`/plot` on a plot. Three rows over the plot you stand on.

| Slot | Tile | Click |
|---|---|---|
| 4 | The plot: id, owner, name, members | |
| 6 | Level, points and what is left to the next | Drawn on a claimed plot |
| 11 | Flags | Opens the [flags window](#flags) |
| 13 | Snapshot | Takes a picture; the owner only |
| 15 | Reset | Resets the plot, asking twice; the owner only |
| 20 | Decoration | Opens the [shop](shop.md) over this plot |
| 22 | Members | Opens the [members window](#members) |

Conditions: `plots:claimed`, `plots:owner`, `plots:may-set`, `plots:flag flag=<name>`. Actions: `plots:snapshot`,
`plots:reset`, `plots:flags` (in chat), `plots:flags-window`, `plots:members-window`, `plots:shop-window`.

## Flags

Every flag as a lever, 45 to a page, glowing when on. Left click turns the flag over on this plot
(`plots:flag-throw`); right click lets the world decide again (`plots:flag-clear`). A row is drawn by the block named
for its state: on, off or locked. Tokens: `%flag_name%` (in the reader's language), `%flag_key%`, `%flag_state%`,
`%flag_on%`.

## Members

Everybody trusted on or banned from the plot, as heads. A click on a member opens their [grades](#member). The clock at
49 opens a 30 minute [session](../features/members.md#sessions) on a left click (`plots:session-open:30`) and closes it
early on a right click.

| Token | Means |
|---|---|
| `%member_name%` | Who it is |
| `%member_kind%` | `member` or `banned`, which picks the row's block |
| `%member_grades%` | What a member may do here |
| `%plot_session%`, `%plot_session_left%` | Whether a session is open, and its minutes left |

A row's click names both `plots:member-open` and `plots:ban-lift`, and each acts only on its own kind of row: a member's
opens their grades, a banned player's lets them back in with no grade.

## Member

| Slot | Button |
|---|---|
| 10 | `build` |
| 11 | `interact` |
| 12 | `container` |
| 13 | `settings` |
| 15 | Remove them from the plot |
| 16 | Ban them, which takes their grades |
| 22 | Back to the list |

Each grade button throws that grade (`plots:grade:<grade>`) and reads `%grade_build%` and the like, on or off.

Related: [Members and visitors](../features/members.md), [Flags](../features/flags.md)

---
title: Leaderboard
order: 560
description: Islands compete on a server-wide leaderboard, ranked by level (and points).
---

---

## `/is level`

Shows your island's current level and total points. Aliases: `seviye`, `lvl`.

---

## `/is top`

Prints the top islands in chat. How many lines appear is set by `top.chat-lines`
(default 10).

```
/is top
```

Each line shows the rank, owner, level, and points.

---

## `/is top holo`

Toggles a **personal floating hologram** of the leaderboard. It:

- Follows you around as you move
- Is **only visible to you**
- Works in **any world** (including Chunklock worlds)

Run it again to turn it off.

### Hologram settings

Configured under `top.hologram` in [config.yml](../configuration/config-yml.md):

```yaml
top:
  chat-lines: 10
  hologram:
    lines: 10            # how many ranks to show
    height: 2.8          # blocks above your head
    refresh-ticks: 40    # how often the text refreshes
    title: "&6&lTOP ISLANDS"
    entry: "&7{rank}. &f{owner} &8- &a{level} &7({points})"
    empty: "&7No islands yet."
```

`{rank}`, `{owner}`, `{level}`, and `{points}` are filled in per row.

---

## How Ranking Works

Islands are ordered by **level**, with **points** breaking ties. Points come from
the blocks placed on the island — see [Points & Levels](../progression/points-levels.md).

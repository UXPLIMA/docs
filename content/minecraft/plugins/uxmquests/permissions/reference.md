---
title: Reference
order: 601
description: Every permission node of uxmQuests with its default and what it allows.
icon: list-checks
---

A player branch's node is held by everybody, so the branch stays visible and a player without it is refused with
`command.no-permission`. An operator branch is hidden from anybody who lacks its node.

## Players

| Node | Default | Allows |
|---|---|---|
| `uxmquests.use` | true | `/quests`, `start`, `claim`, `pass`, `pass claim` |
| `uxmquests.reroll` | true | `/quests reroll` |
| `uxmquests.language` | true | `/quests lang`, `lang set`, `lang reset` |

## Numbers on a node

| Node | Default | Gives |
|---|---|---|
| `uxmquests.count.<n>` | op (`count.*`) | A board of `n` quests in every group |
| `uxmquests.count.<group>.<n>` | op (`count.*`) | A board of `n` in that group |
| `uxmquests.rerolls.<n>` | op (`rerolls.*`) | `n` rerolls per board |

The highest number wins and the group file's number is the floor. The prefixes are `ranks.board-size` and
`ranks.rerolls` in `config.conf`.

## The premium track

The node a pass file names in `premium`, such as `uxmquests.pass.season.premium` for the shipped `season.conf`. It is
not declared, so operators hold it until you give it to others.

## Staff

| Node | Default | Allows |
|---|---|---|
| `uxmquests.bypass` | op | Counts progress the anti-glitch rules would refuse; the node is `anti-glitch.bypass-permission` |
| `uxmquests.admin.language` | op | `lang set-player`, `lang reset-player` |
| `uxmquests.admin.finish` | op | `finish` |
| `uxmquests.admin.reset` | op | `reset board`, `reset totals` |
| `uxmquests.admin.inspect` | op | `types`, `problems`, `totals`, `pass top` |
| `uxmquests.admin.trigger` | op | `trigger` |
| `uxmquests.admin.pass` | op | `pass xp`, `pass booster` |
| `uxmquests.admin.doctor` | op | `doctor` |
| `uxmquests.admin.version` | op | `version` and the bare command |
| `uxmquests.admin.reload` | op | `reload` |
| `uxmquests.admin.*` | op | Declared with no children: grant the nodes above, or use your permission plugin's wildcard |

Related: [Commands](../commands/overview.md)

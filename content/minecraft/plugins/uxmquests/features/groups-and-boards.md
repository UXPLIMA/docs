---
title: Groups and boards
order: 301
description: The group block, modes, claiming, sharing, rerolls, rules and rarities, with the shipped daily group.
icon: layout-list
---

A group file under `groups/` is one set of quests; the file name is the id, so `weekly.conf` is the group `weekly`.
Copy `daily.conf` to start; a new file needs nothing else.

```hocon
group {
  name        = "@group.daily.name"
  description = "@group.daily.description"
  mode        = cycle
  count       = 4
  claim       = automatic
  sharing     = solo
  rerolls     = 1
  rule        { required = 2, order = any }
  reset       { type = wall-clock, at = "04:00", zone = "Europe/Istanbul" }
}
rarities = [
  { name = common, weight = 10 }
  { name = rare,   weight = 2, least = 1, most = 2 }
]
quests = [ ... ]
```

| Key | Meaning |
|---|---|
| `name`, `description` | What players read; `@key` is a line of `messages/`, anything else is shown as written; no name is the file name |
| `mode` | `cycle`: a board of `count` quests is drawn for each player and thrown away on every reset. `catalogue`: every quest is on offer and the player starts the ones they want |
| `count` | Quests on a board. A rank node may raise it; see [Ranks](ranks.md) |
| `claim` | `automatic` pays a finished quest at once; `manual` waits for `/quests claim` or a click |
| `sharing` | `solo` or `contribution`; see [Shared boards](shared-boards.md) |
| `rerolls` | Quests a player may swap per board, the floor a rank node raises |
| `rule.required` | How many of a quest's tasks finish it; left out, all of them |
| `rule.order` | `any`, or `in-order` for a quest whose second task waits for the first |
| `reset` | See [Resets](resets.md) |
| `rarities` | The buckets a quest is drawn from: `weight` is how often a bucket is picked, `least` how many of it every board is guaranteed, `most` a ceiling |

A quest writes its own `rule` to override the group's. Quests, rarities and milestones are lists, never blocks, because
a block loses the order and an in-order quest runs in it.

## The shipped daily group

Four quests a board, paid at once, one reroll, drawn again at 04:00 Istanbul time, one or two of them rare:

| Quest | Rarity | Asks for |
|---|---|---|
| `mine-stone` | common | 64 to 128 stone, deepslate or cobblestone |
| `mine-coal` | common | 16 to 32 coal ore |
| `mine-iron` | common | 12 to 24 iron ore |
| `chop-wood` | common | 32 to 64 logs of one tree drawn per player |
| `smelt-iron` | common | 16 to 32 iron ingots smelted |
| `bake-bread` | common | 8 to 16 bread crafted |
| `cast-the-line` | common | 8 to 16 catches, anything the rod brings up |
| `raise-the-walls` | common | 128 to 256 blocks placed |
| `hunt-the-night` | rare | 10 to 20 zombies and 10 to 20 skeletons |
| `clear-the-caves` | rare | 15 to 25 creepers, spiders, cave spiders or witches |
| `glass-and-panes` | rare | 32 to 64 glass smelted, then 16 to 32 panes crafted, in order |
| `the-deep-dark` | rare | 3 to 6 diamond ore |

Every shipped quest has `rewards = []`, and the group's `rewards.set` and `milestones` are empty: write what your
server pays.

## The board

`/quests` opens the [quests window](../menus/quests.md); a group's board opens from it. A board is read from memory: it
is loaded when the player joins and written on the async lane.

Related: [Quests and tasks](quests-and-tasks.md), [Group files](../config/group-files.md)

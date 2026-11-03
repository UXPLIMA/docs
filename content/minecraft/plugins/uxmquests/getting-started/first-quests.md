---
title: Your first quests
order: 104
description: A weekly group with two quests and rewards, step by step.
icon: footprints
---

1. Copy `groups/daily.conf` to `groups/weekly.conf`. The file name is the group's id: `weekly`.
2. Set the group:

```hocon
group {
  name    = "Weekly"
  mode    = cycle
  count   = 2
  claim   = manual
  sharing = solo
  rerolls = 1
  reset { type = cron, cron = "0 4 * * 1", zone = "Europe/Istanbul" }
}
rarities = [ { name = common, weight = 1 } ]
```

3. Write the quests as a list:

```hocon
quests = [
  {
    id = miner
    name = "Deep miner"
    rarity = common
    tasks = [ { id = mine, type = block-break, block = [DIAMOND_ORE, DEEPSLATE_DIAMOND_ORE], amount = "8-16", reverse = true } ]
    rewards = [ "[give-money] {500-1000}", "[message] Well dug, {player}." ]
  }
  {
    id = hunter
    name = "Hunter"
    rarity = common
    tasks = [ { id = kill, type = mob-kill, mob = ZOMBIE, amount = 50 } ]
    rewards = [ "[console] give {player} diamond 2" ]
  }
]
```

4. Run `/uxmquests reload`, then `/quests`. The window shows the `weekly` group; open it and the two quests are there.
5. Finish one and click it, or run `/quests claim weekly miner`, to be paid.
6. To feed a battle pass, add `weekly` to `tasks.groups` in `passes/season.conf` and set `battlepass.enabled = true`.

Plain words are shown as written. For a server with several languages, write `@quest.miner.name` and put the line in
every `messages/` file; see [Languages](../features/languages.md).

Related: [Quests and tasks](../features/quests-and-tasks.md), [Rewards and milestones](../features/rewards.md)

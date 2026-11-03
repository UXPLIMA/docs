---
title: Rewards and milestones
order: 307
description: Reward action lines, spans, set rewards, and milestones per group and over a record.
icon: gift
---

A reward is a list of action lines, the grammar every UXM plugin reads. A single line without a list is read as one.

| Verb | Does |
|---|---|
| `[message]`, `[broadcast]`, `[actionbar]`, `[title]`, `[subtitle]`, `[bossbar]` | Words |
| `[particle]`, `[sound]`, `[effect]` | Particles, a sound, a potion effect |
| `[console]`, `[player]` | A command as the console or the player |
| `[give-money]`, `[take-money]` | Through Vault |

Any line may carry `{delay=, chance=, repeat=, every=, if=}` between the verb and the payload.

- `{player}`, `<player>`, `%player%` and `%player_name%` are the player's name, filled in before the line is read. A
  name an account could not have is never filled in: the line is skipped and the console says so.
- `{50-200}` is a span drawn once per payment, so the number shown is the number paid.
- `@some.key` is a line of `messages/`, read in the paid player's language.
- PlaceholderAPI is applied after the plugin's own tokens when it is installed.

## Set rewards and group milestones

A group file's top-level `rewards` block:

```hocon
rewards {
  set = [ "[message] Every quest on the board is done." ]
  milestones = [
    { at = 50, rewards = [ "[console] give {player} diamond 5" ] }
  ]
}
```

`set` pays once when every quest on a board is finished. A group's `milestones` count finished quests of that group.

## Overall milestones

`rewards.milestones` in `config.conf` counts a player's whole record:

```hocon
rewards {
  milestones = [
    { at = 100, rewards = [ "[message] One hundred quests." ] }
    { at = 500, rewards = [ "[console] lp user {player} parent add veteran" ] }
  ]
}
```

`/uxmquests totals <player>` reads the record and `/uxmquests reset totals <player>` forgets it.

Related: [Feedback](feedback.md)

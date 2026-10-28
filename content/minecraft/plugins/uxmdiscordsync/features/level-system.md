---
title: Level system
order: 34
description: Text and voice XP in Discord, the level table, and rewards claimed in game.
icon: trophy
---

Members earn XP for writing in chosen Discord channels and for talking in chosen voice channels.
Text and voice each have their own level, and each level can pay rewards in game, claimed with
`/discordreward`. It gives players a reason to spend time in your Discord.

Everything is in `level-system.yml`, and it ships turned off.

```yaml
enabled: false
linked-only: true
```

With `linked-only: true`, only Discord accounts linked to a Minecraft account earn XP. Turn it off to
let everyone level up; the rewards still need a link to be claimed.

XP belongs to the Discord account. A member who unlinks keeps their levels, and the rewards they
already claimed are not paid again on another Minecraft account.

## Text XP

```yaml
text-level:
  enabled: true
  xp-cooldown: 60
  channels:
    "123456789012345678": 10
    "987654321098765432": 15
```

A message in a listed channel earns that channel's XP. A channel that is not listed earns nothing, and
a thread counts as its own channel, with its own id. `xp-cooldown` is the time in seconds before the
same member can earn text XP again, in any channel; `0` gives XP for every message. Bots earn
nothing.

Giving channels different values is how you decide where activity counts: the main chat can be worth
10 and a spam channel 1.

## Voice XP

```yaml
voice-level:
  enabled: true
  xp-interval: 60
  rules:
    no-xp-when-deafened: true
    no-xp-when-alone: true
    no-xp-in-afk-channel: true
  channels:
    "123456789012345678": 20
```

Every `xp-interval` seconds, each member in a listed voice channel earns that channel's XP. `0` turns
voice XP off.

| Rule | Who earns nothing |
|---|---|
| `no-xp-when-deafened` | Deafened members, by themselves or by a moderator |
| `no-xp-when-alone` | A member without at least one other member in the channel who also earns XP |
| `no-xp-in-afk-channel` | Members in the Discord server's AFK channel |

A muted member still earns XP: listening counts. Bots, and unlinked members while `linked-only` is
on, earn nothing and do not count as company.

## The level table

```yaml
xp-per-level:
  1: 100
  2: 250
  5: 2000
  10: 10000
  20: 50000
```

Each value is the total XP needed to reach that level, not the XP between levels, so the values must
grow. A level that is not listed follows the formula level² × 100, counted on from the nearest listed
level below. With the table above, level 21 needs 50000 + 21² × 100 - 20² × 100 = 54100.

Write out the early levels by hand, where the pace matters most, and let the formula carry the rest.
If you lower the values, members get the level their XP now reaches the next time they are looked up.

## Rewards

```yaml
text-level:
  rewards:
    5:
      - "give %player% diamond 5"
    15:
      - "lp user %player% parent addtemp vip 30d"
voice-level:
  rewards:
    5:
      - "eco give %player% 2000"
```

Console commands, by level, with a separate table for text and voice. `%player%` is the Minecraft
name; no other placeholder is filled in here.

Rewards are not given at the level-up itself. The player types `/discordreward` in game, and every
reward they have earned and not taken yet runs, for both tables. Each level's reward is paid once per
Discord account and once per Minecraft account.

<Callout type="warning" title="A rank given as a reward is given forever">

`lp user %player% parent add vip` has no matching removal: the rank stays even if the member stops
being active. Use a timed grant such as `parent addtemp vip 30d` unless a permanent rank is what you
mean.

</Callout>

## Level-up messages

```yaml
notifications:
  level-up: true
  level-up-message: "Congratulations! You've reached {type} level {level}!"
```

Sent as a direct message. `{type}` is `Text` or `Voice` and `{level}` the new level. Members who block
direct messages from server members do not get it.

## In Discord

| Command | What it shows |
|---|---|
| `/level [user]` | A member's levels and XP, and how much is left to the next level. Without `user`, your own |
| `/leaderboard <text or voice>` | The top 10 |

Both are written in `embeds.level` and `embeds.leaderboard`, with the keys from
[Concepts](../getting-started/concepts.md#embeds). `/level` takes `%user%`, `%user_mention%`,
`%user_id%`, `%user_avatar%`, `%text_level%`, `%text_xp%`, `%text_xp_needed%`, `%voice_level%`,
`%voice_xp%` and `%voice_xp_needed%`; its `text-field` and `voice-field` can be edited or switched off.
`/leaderboard` takes `%type%`.

In game and in other plugins, `%discordsync_text_level%` and the related placeholders show the same
numbers; see [Placeholders](../placeholders.md).

## Several servers

Each message earns XP once, whichever server sees it, and voice XP is counted by one server. Keep
`xp-interval` and the channel lists the same on every server.

Related: [Commands](../commands.md), [Boost rewards](boost-rewards.md)

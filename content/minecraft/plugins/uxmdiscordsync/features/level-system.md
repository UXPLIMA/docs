---
title: Level system
order: 34
description: Text and voice XP, the level curve, and rewards paid in Minecraft.
icon: trophy
---

Players earn XP for talking in Discord text channels and for sitting in Discord voice channels, and
levels pay out as Minecraft commands. It is a way to make the Discord worth being in.

Configured in `level-system.yml`, off by default.

## Text levels

```yaml
text-level:
  enabled: true
  xp-cooldown: 60
  channels:
    "123456789012345678": 10
    "987654321098765432": 15
    "111222333444555666": 5
```

XP is granted per message, at the rate set for that channel, no more than once per `xp-cooldown`
seconds. A channel not in the list grants nothing.

Per-channel rates are the anti-spam mechanism as much as the cooldown: the general chat can be worth
10 and a memes channel worth 1, so activity where it matters counts for more.

Text XP requires the **Message Content** intent.

## Voice levels

```yaml
voice-level:
  enabled: true
  xp-interval: 60
  channels:
    "123456789012345678": 20
    "987654321098765432": 30
```

Every `xp-interval` seconds, everyone in a listed voice channel gets that channel's XP.

## The XP curve

```yaml
xp-per-level:
  1: 100
  2: 250
  3: 500
  4: 1000
  5: 2000
  10: 10000
  15: 25000
  20: 50000
```

A level not listed falls back to `level² × 100`. So you can specify the early levels by hand, where
the pacing matters most, and let the formula take over afterwards.

## Rewards

```yaml
text-level:
  rewards:
    5:
      - "give %player% diamond 5"
      - "eco give %player% 1000"
    15:
      - "lp user %player% parent add vip"
```

Console commands, keyed by level. `%player%` is the linked Minecraft username. Text and voice levels
have separate reward tables, so voice time can pay differently from chatting.

A player must be linked for a reward to mean anything — there is no Minecraft account to run the
command against otherwise. Rewards are claimed with `/discordreward` in game.

<Callout type="warning" title="Rank grants through rewards">

`lp user %player% parent add vip` is permanent and has no matching removal. If a level reward grants
a rank, it grants it forever. Use a timed grant — `lp user %player% parent addtemp vip 30d` — if that
is not what you mean.

</Callout>

## Notifications

```yaml
notifications:
  level-up: true
  level-up-message: "🎉 Congratulations! You've reached {type} level {level}!"
```

Sent as a DM. `{type}` is `text` or `voice` and `{level}` is the new level. Note these use braces,
not percent signs — the level system's own message placeholders differ from the `%...%` form used
elsewhere in the plugin.

## Viewing levels

From Discord:

```
/level [user]
/leaderboard <text|voice>
```

`/level` with no argument shows your own.

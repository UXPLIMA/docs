---
title: Prestige
order: 304
description: Resetting a finished track for a permanent multiplier and a badge, what it costs, and the rules.
icon: star
---

```hocon
prestige {
  required-level = 100
  money-cost = 0.0
  experience-cost = 0
  maximum = 10
  bonus-per-prestige = 0.1
  default-badge = "<warn>[<prestige>]"
  badges {
    "10" = "<warn>[X]"
  }
}
```

| Key | Default | What it does |
|---|---|---|
| `required-level` | `100` | The level a player must reach. A track whose maximum is lower uses its own maximum |
| `money-cost` | `0.0` | What a prestige costs, in `economy.currency`. `0` asks nothing of an economy |
| `experience-cost` | `0` | Experience the player must hold. The reset clears it anyway, so this is a floor, not a price |
| `maximum` | `10` | How many times one player may prestige one track |
| `bonus-per-prestige` | `0.1` | Added to the track's multiplier per prestige, not compounded: the third earns 1.3 times |
| `default-badge` | `"<warn>[<prestige>]"` | The badge for a count `badges` does not name; `<prestige>` is the count |
| `badges` | `"10" = "<warn>[X]"` | A badge for one exact count |

The rule is one for every track. A player prestiges each track on its own.

## What a prestige does

1. Checks, in order: the count is below `maximum`, the level is at least `required-level`, the experience at least
   `experience-cost`, and, when `money-cost` is above 0, the economy is there and the player can pay. The payment is
   taken here.
2. Sets the track back to level 1 with no experience, and adds one to its prestige count.
3. Clears the player's reward claims on that track, so every reward can be taken again.

| Reply | Key |
|---|---|
| `You prestiged Mining to 1. Your multiplier is now 1.10.` | `prestige.done` |
| `Prestige needs level 100. You are 64.` | `prestige.level-too-low` |
| `You have prestiged this track as far as it goes.` | `prestige.maximum` |
| `Prestige needs 5000 experience. You have 0.` | `prestige.not-enough-experience` |
| `This prestige costs money and this server has no economy.` | `prestige.no-economy` |
| `Prestige costs 1000.` | `prestige.cannot-afford` |

The all time board ranks by experience, so a player who prestiges drops down it and climbs back.

## Where a player prestiges

From `/levels prestige <track>`, which needs `uxmlevels.prestige` (`op` by default), or from the
[prestige window](../menus/prestige.md), reached by the star in the levels window. The window checks no node: any
player who can open `/levels` can prestige from it. Take the star out of `menus/main.conf` to keep prestige to the
command.

## The badge

`%uxmlevels_<track>_badge%` is the badge as plain text and `%uxmlevels_<track>_badge_raw%` as the MiniMessage written,
for a chat format or a tab list. A track never prestiged has no badge.

## The money

`economy.currency` names the economy: `exp`, `levels`, `vault`, `vault-unlocked`, `playerpoints`, `ecobits:<currency>`
or `treasury`. See [Economies](../integrations/economies.md).

Related: [Prestige window](../menus/prestige.md), [Multipliers and boosts](multipliers-and-boosts.md)

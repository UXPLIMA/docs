---
title: Rewards and groups
order: 302
description: Groups, weights, rarities, what a reward gives, limits, guarantees, linked and fallback rewards, luck and shared rewards.
icon: gift
---

## Groups

A crate's `groups` block holds one or more groups. **Exactly one reward is drawn from each group** per opening. One
group is a flat weighted table; three groups give one of each in a single opening. A group has a `name` and its
`rewards`.

`no-repeat = true` keeps one opening from giving the same reward twice. A crate that draws more rewards than it has
must repeat one; it repeats rather than hanging.

## Weights

A reward's chance is its `weight` over the sum of the weights in its group. `70`, `25` and `5` are 70, 25 and 5
percent; add a fourth reward of `5` and the first becomes 66.7 percent. A weight is a number, never a percentage and
never an expression. The [preview](../menus/preview.md) always shows the reader the real number.

A reward with `weight = 0` never rolls. It still arrives as a [guarantee](#guarantees), a [linked](#linked-rewards)
reward or a [fallback](#fallback).

## Rarities

```hocon
rarities {
  common { name = "Common", colour = "muted", order = 1 }
  rare   { name = "Rare",   colour = "info",  order = 3 }
  epic   { name = "Epic",   colour = "level", order = 5, weight = 5, effects = ["[sound] ui.toast.challenge_complete 1 1"] }
}
```

A rarity is a name, a colour role from [theme.conf](../config/theme.md) and a place in an `order`. It never multiplies
a weight. Its `weight` is a default for a reward that writes none. Its `order` is what the broadcast `threshold`
compares, and its `effects` fire after the crate's win effects.

## What a reward gives

| Key | Gives |
|---|---|
| `items` | Item lines such as `"DIAMOND 3"`, or item blocks with a name, lore, enchantments and everything the uxmLib item format reads |
| `commands` | Console lines. `%player%`, `{player}` or `<player>` is the winner's name |
| `money` | Payments such as `"vault 500"`, `"ecobits:gems 3"`, `"exp 500"` or `"levels 5"`, named like a cost |

A name an account could not have is never put into a command: that line is skipped and the console says so. A reward
that only runs a command needs a face: give it `display.icon`, or the preview has nothing to draw.

A reward that does not fit waits in the [pending rewards](../menus/pending.md). Nothing a crate decided is lost.

## How a reward looks

```hocon
display {
  name   = "@content.crates.common.groups.loot.rewards.diamond.display.name"
  rarity = "rare"
  lore   = [ "<subtext>Three of them." ]
  icon   { material = "DIAMOND", glow = true }
  order  = 1
}
```

`order` places the reward on the preview page, lowest first; a reward without one sits after those with one, and ties
are settled by id. It changes nothing about the roll.

## Limits

```hocon
limits {
  player { amount = 3 }
  global { amount = 10, reset = "7d" }
}
```

`player` counts this player's wins of the reward, `global` the whole server's (on a shared database, the whole
network's). At the limit the reward weighs zero and its tile reads `Used up`. With `reset`, a span such as `12h` or
`7d`, the limit comes back that long after the win that reached it; without one, it never does. A one-time reward is a
limit of one. `/uxmcrates reset player-limit` and `reset global-limit` clear the counts of every reward of a crate.

## Guarantees

```hocon
guarantee { mode = "every", opening = 25 }
```

| `mode` | Fires |
|---|---|
| `once` | At that opening of the crate, and never again |
| `every` | At that opening, twice that, and forever after |

The count is the player's own openings of the crate. A guarantee is checked against the limits and the permission, not
the weight, so a `weight = 0` reward with a guarantee is a pity prize that never rolls on its own.

## Linked rewards

`linked = ["scabbard"]` grants the named rewards of the same crate along with this one. A reward that is only ever
linked is never rolled on its own: its effective weight is zero.

## Fallback

`fallback = true` marks the reward a group falls back to when it can produce nothing for this player: every other
reward used up, or locked behind a permission. It is in the preview and still rolls on its weight; write `weight = 0`
for one that arrives only when nothing else can.

## Permission

`permission = "rank.vip"` on a reward means only a holder can win it. For anyone else it weighs zero and its tile
reads `Locked`.

## Announce

`announce = true` broadcasts the win whatever the crate's threshold says.

## Luck

```hocon
luck {
  permission {
    "uxmcrates.luck.tier-one" = 1.25
    "uxmcrates.luck.tier-two" = 1.5
  }
}
```

In [config.conf](../config/config-conf.md#luck), a node you invent maps to a multiplier on the weight of every reward
for a player who holds it. A player who holds several takes the highest, never the product. The preview shows a lucky
player their own, higher chance. `luck = false` on a reward keeps it out, which suits a fallback or a guarantee-only
reward.

## Shared rewards

A reward written once in `rewards/<id>.conf` is used by any crate with `from`:

```hocon
sword {
  from   = "legendary-sword"
  weight = 20
}
```

Whatever the crate writes wins over the file, so the sword can weigh 20 in one crate and 5 elsewhere. A shared reward
cannot name another shared reward. See [Reward files](../config/reward-files.md).

Related: [Crate files](../config/crate-files.md), [Preview](../menus/preview.md), [Placeholders](../placeholders/reference.md)

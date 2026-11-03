---
title: Rerolls and milestones
order: 307
description: Rolling an opening again from a rank ladder, a grant or a price, and rewards for opening enough crates.
icon: repeat
---

## Rerolls

After a reveal, a player with a reroll to spend gets the [reroll window](../menus/reroll.md) instead of the reward:
look at what was won, keep it, or roll again. Until they choose, the reward waits in their pending rewards, so closing
the window or logging out loses nothing. A player with no reroll never sees the window.

A reroll draws the whole table again and replaces the rewards of that opening. A reroll is spent whether the new draw
is better or not.

### Where rerolls come from

They are spent in this order, per opening:

| Source | Set by |
|---|---|
| The rank ladder | `rerolls.groups` in [config.conf](../config/config-conf.md#rerolls): the rerolls of the player's highest group, for each opening |
| Granted rerolls | `/uxmcrates reroll give <player> <amount>`: a balance, kept until spent |
| Bought rerolls | `rerolls.price`: paid for each one past the two above |

```hocon
rerolls {
  price { currency = "vault", amount = "reroll * 250", maximum = 3 }
  groups {
    silver { permission = "uxmcrates.reroll.silver", amount = 1, priority = 1, refusal = "This is for silver and above." }
    gold   { permission = "uxmcrates.reroll.gold",   amount = 3, priority = 2, refusal = "This is for gold and above." }
  }
}
```

- **Groups do not stack.** A player who holds several takes the one with the highest `priority`, not the sum.
- **`refusal`** is the line shown to a player below that group, naming what the next group up would give.
- **The price climbs.** `amount` is an expression over `reroll`, the number of the bought reroll counting from one:
  `reroll * 250` costs 250, then 500, then 750. `maximum` caps how many may be bought in one opening; `0` is no cap.
  Leave `currency` or `amount` empty and a reroll is never bought.
- The window opens only when the ladder or a grant gives at least one reroll; a purchase is offered from inside it.

The nodes in `rerolls.groups` are yours to invent. `%uxmcrates_rerolls%` is the granted balance.

## Milestones

A milestone rewards a player for reaching a number of openings. Two ladders, both in
[config.conf](../config/config-conf.md#milestones):

| Ladder | Counts | Window |
|---|---|---|
| `per-crate` | Each crate on its own: ten openings of `common` and ten of `elite` are two rungs | `/crates milestones <crate>` |
| `every-crate` | Every opening of any crate | `/crates milestones` |

```hocon
milestones {
  per-crate {
    "10"  { name = "Ten openings", items = ["DIAMOND 1"], commands = [], automatic = true }
    "100" { name = "A hundred openings", items = ["NETHERITE_INGOT 1"], automatic = false }
  }
  every-crate {
    "500" { name = "Five hundred openings of anything", items = ["NETHERITE_BLOCK 1"], automatic = false }
  }
}
```

- Each rung is keyed by the count that reaches it, and gives `items` and runs `commands`.
- `automatic = true` hands the rung over the moment it is reached; `false` waits in the milestones window for a click.
- A player who reaches a rung on both ladders takes both.
- The counts already exist, so adding a ladder resets nobody: a player with four hundred openings is at four hundred.
- A file written with the numbers straight under `milestones` still reads, as the per-crate ladder.

The player reads `milestone.reached` or `milestone.reached-every-crate`. `%uxmcrates_milestone_<crate>%` and
`%uxmcrates_milestone%` count the openings left to the next rung.

Related: [Reroll window](../menus/reroll.md), [Milestones window](../menus/milestones.md)

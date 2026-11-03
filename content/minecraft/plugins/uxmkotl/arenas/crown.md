---
title: The crown
order: 302
description: Claiming the plate, the claim cooldown, how a reign ends, the ring over the king, and the push off a protected plate.
icon: crown
---

Each arena has one crown. It belongs to the player whose feet are in the plate block, and it is what every board
counts.

## Claiming

A claim happens when a player in the arena moves into the plate block. Standing still on it does nothing new; moving
within the block does not claim again.

| The plate is | What happens |
|---|---|
| Free | The player is king at once. The `crowned` moment runs and the claim reward is paid |
| Held by this player | Nothing |
| Held by another king, inside the protection | Refused. The `claim-blocked` moment runs with the time left; with `plate.repel` on, the player is pushed off |
| Held by another king, after the protection | The player is king. The old king's reign ends and `dethroned` runs for them |

The protection is `crown.claim-cooldown-ms`, 1500 by default, counted from the sitting king's claim. It guards a
sitting king only: a free plate is always claimed at once, so a player who waits for the king to step off does not
wait again. Without it, two players standing in one block would trade the crown on every step, and the record would
fill with reigns a moment long.

## How a reign ends

| Event | `dethroned` runs | Notes |
|---|---|---|
| The king steps off the plate, still inside the box | yes | |
| The king steps out of the box | yes | Then `left` |
| `/kotl leave` | yes | Then `left` |
| Another player claims after the protection | yes, for the old king | The new king's `crowned` runs too |
| The king dies | no | The `fell` moment runs instead |
| The king quits | no | |

When a reign ends, its length is added to the king's time held, and it becomes their longest reign if it beats the
one before. `dethroned` fills `<time>` with the length, such as `1m 12s`. A reign still running when the server
stops is not added.

## The ring

While a player is king, a ring of dust particles circles above their head. It is set under `crown.ring` in
[config.conf](../config/config-conf.md#crown):

| Key | Default | Meaning |
|---|---|---|
| `enabled` | `true` | `false` draws no ring |
| `colour` | `"#FFE66D"` | The colour of the dust |
| `radius` | `0.32` | How far from the middle of the head, in blocks. Above `0.5` it reads as a circle on the floor around the player |
| `height` | `0.45` | How far above the eyes |
| `points` | `12` | How many particles make the ring |
| `size` | `0.7` | How large one particle is drawn |
| `period-ms` | `250` | How often it is drawn again |

The ring is drawn for the arenas that were on at the last start or reload; an arena switched on after that gets its
ring at the next `/uxmkotl reload`.

## The push off a protected plate

```hocon
plate {
  repel {
    enabled  = false
    strength = 0.6
    upward   = 0.35
  }
}
```

With `enabled = true`, a player whose claim was refused is pushed away from the middle of the plate with
`strength` sideways and `upward` up. A player exactly on the middle is pushed backwards from where they face. Off
by default: a refused player can then stand in the plate block without the crown until the protection runs out and
they step into it again.

## Notes

- **The plate is a position.** A pressure plate, a gold block or air: the plugin reads where the player's feet are,
  not what is there. A pressure plate also sends a redstone signal, which nothing here uses.
- **Crowns count claims.** Every successful claim is one crown on the record and on the `claims` board, however long
  the reign lasted.

Related: [Fights](fights.md), [Rewards](rewards.md), [Moments](moments.md)

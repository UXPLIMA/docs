---
title: Player commands
order: 202
description: Every /crates subcommand, what it opens or does, and how it refuses.
icon: user
---

`/crates`, aliases `/crate` and `/cr`. Every subcommand needs a player and carries no permission node.

| Command | What it does |
|---|---|
| `/crates` | Opens the [crate list](../menus/crates.md) |
| `/crates open <crate>` | Opens a crate by its id, with nothing placed in the world |
| `/crates preview <crate>` | Opens the [preview](../menus/preview.md): every reward with its weight and the player's chance. Costs nothing |
| `/crates keys` | Opens the [keys window](../menus/keys.md): every key held and the crates each opens |
| `/crates claim` | Opens the [waiting rewards](../menus/pending.md) |
| `/crates send <player> <key> <amount>` | Sends keys to another player, when the key file says `tradeable = true` |
| `/crates massopen <crate>` | Opens as many keys as the cap allows at once, with one [summary window](../menus/bulk.md) |
| `/crates cost <crate>` | Opens the [cost window](../menus/cost.md): every line the crate takes, and whether the player can pay |
| `/crates milestones [crate]` | Opens the [milestones](../menus/milestones.md) of a crate, or with no crate the ladder that counts every crate |

## open

`/crates open <crate>` runs the same checks as a click on a placed crate, in this order, and answers the first that
fails:

| Check | Refusal |
|---|---|
| No other opening of this player is running | `crate.already-opening` |
| The crate's world rule | `open.wrong-world` |
| The crate exists | `crate.unknown` |
| The crate's `permission` | `crate.no-permission` |
| The requirements | `crate.requirements` |
| The claim rule, at a placed crate | `crate.protected-land` |
| The cooldown | `crate.on-cooldown` with the time left, or `crate.once-only` for a crate that opens once |
| The table | `crate.nothing-to-give`: every reward is used up or locked, and nothing was taken |
| The key | `crate.no-key` with the amount and the key's name |
| The cost | `crate.cannot-pay`; the key is given back |

The rewards are drawn before the key is taken, so a crate with nothing left for a player costs them nothing.
`uxmcrates.cooldown.bypass` skips the cooldown check.

A refusal at a placed crate also fires `pushback`: the player is knocked back and the actions run. A crate opened by
command is opened where the player stands, so a world reveal falls back to its window reveal.

## preview

Opens one page per group, every reward in `display.order`. Each tile shows the weight, the chance for the reader with
`odds.decimals` places, the rarity and the group, and `Locked` or `Used up` on a reward the reader cannot win. With
`odds.show = false` the two numbers are replaced by one line saying the odds are not shown. A player opens at most one
preview per `opening.preview-cooldown-ms`; a faster one answers `crate.preview-too-fast`.

## send

The sender must hold the amount as a balance. The key file must have `tradeable = true`, or the answer is
`key.not-tradeable`. A player cannot send to themselves (`key.send-yourself`) or send more than they hold
(`key.send-none`). The receiver may be away; they read `key.sent-to-you` if they are online. Both sides are rows in the
key ledger as a transfer.

## massopen

Opens up to the crate's `bulk.cap`, else `opening.bulk-cap` (25), with no animation. Every check of a single opening
holds for each one, and a refusal before the first opening is answered with its own reason. The summary window counts
each reward won. The crate's `bulk.announce` decides the broadcast: `each`, `once` naming the best win, or `never`.

## milestones

`/crates milestones <crate>` shows the per-crate ladder counted for that crate. `/crates milestones` with no crate
shows the ladder that counts every opening of any crate.

Related: [Overview](overview.md), [Menus](../menus/overview.md), [Opening](../features/opening.md)

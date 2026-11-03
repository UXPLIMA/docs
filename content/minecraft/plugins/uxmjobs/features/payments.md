---
title: Payments
order: 404
description: The one order every wage passes, the six multipliers, batching, and /jobs why.
icon: coins
---

Every wage passes one order, once, so a booster is never applied before a cap in one path and after it in another.

1. **Ledger.** Whether anything is owed at all: a placed block, or a spot already paid, pays nothing.
2. **Expression.** The row's expression at the player's level, held by `soft-limit`.
3. **Multipliers.** Six, multiplied together:

   | Multiplier | Is one when |
   |---|---|
   | Decay | The work has not been repeated past the threshold |
   | Grade | The protections had nothing to say |
   | Booster | No booster applies |
   | Zone | No zone covers the block, or the player fails its requirements |
   | Claim | No claim covers the block |
   | Contract | No contract is signed for this job |

4. **Bound.** The job's `bounds` for that currency clamp it; with a stacker, per creature, then times the stack.
5. **Income cap.** Over the cap the player keeps a shrinking share.
6. **Tax.** The player's rate is taken off.

A negative wage (a floor below zero) is neither capped nor taxed. The bound sits before the cap on purpose: an
operator reasons about the bound per action and about the cap per hour.

## When it is paid

Wages are gathered for `economy.batch-seconds` (5) and handed to the economy off the region thread, because a call per
broken block would stall the server. Money goes to the server's economy, with the [server account](income-cap-and-tax.md#server-account)
in front of it when that is on. Experience goes to the job, points to the balance.

## Two jobs, one block

When a player works two jobs that both price the same work, each pays its own row.

## /jobs why

The whole order for the last piece of work, step by step: the ledger verdict, the protection grade and its reason, each
multiplier, the bound, the cap and the tax, then what it paid. It is the one command that answers "why did I get so
little".

Related: [Protections](protections.md), [Income cap and tax](income-cap-and-tax.md)

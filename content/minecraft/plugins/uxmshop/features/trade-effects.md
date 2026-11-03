---
title: Trade effects and the log
order: 306
description: What every trade says and plays, and the audit trail.
icon: bell
---

Each moment of a trade is a list in `config.conf` under `effects`.

| Moment | When | Filled in |
|---|---|---|
| `bought` | A purchase went through | `<item>`, `<amount>`, `<price>` |
| `sold` | A sale went through | `<item>`, `<amount>`, `<price>` |
| `taxed` | On top of those, when the trade was taxed | `<tax>` |
| `cannot-afford` | The player has not got the money | `<price>`, `<balance>` |
| `out-of-stock` | The line has less than was asked | `<left>` |
| `quota-reached` | The player has traded their share for now | `<cap>`, `<time>` |
| `no-room` | The player has nowhere to put it | none |

The shipped lines are a message for each, with the orb pickup sound on a trade and a bass note on a refusal. Text
starting with `@` is a line of the language files. The verbs are `[message]`, `[broadcast]`, `[actionbar]`, `[title]`,
`[subtitle]`, `[bossbar]`, `[particle]`, `[sound]`, `[effect]`, `[console]` and `[player]`, each able to carry
`{delay=, chance=, repeat=, every=, if=}`. An empty list is silence. These run for every trade; a line's `actions` run
for that line only.

## The log

Every trade of both halves is a row. `log.keep-days` (30; 0 keeps everything) is how long a row is kept, and
`/uxmshop prune <days>` deletes older rows by hand. Operators read it with `/uxmshop logs`; an owner reads their profit
report with `/myshop profits`.

Related: [config.conf](../config/config-conf.md#effects)

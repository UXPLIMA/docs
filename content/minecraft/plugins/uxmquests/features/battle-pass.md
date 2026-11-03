---
title: Battle pass
order: 411
description: Pass files, the premium track, seasons, unclaimed rewards and tiers, with the shipped season pass.
icon: ticket
---

A pass is a ladder of tiers over two tracks, fed by finished quests and by raw play. It is off until
`battlepass.enabled = true` in `config.conf`; nothing is read, registered or drawn while it is off. A server runs as
many passes at once as it likes, such as a season pass beside event passes; each has its own ladder, tasks and claims,
and every command and window row names its pass.

## A pass file

One file under `passes/`; the file name is the pass id.

```hocon
pass {
  name        = "@pass.season.name"
  description = "@pass.season.description"
  premium     = "uxmquests.pass.season.premium"
  season    { started = "2026-09-01T00:00:00Z", length = "90d" }
  unclaimed { rule = "hold", for = "7d" }
  tasks     { groups = [ daily ], per-quest = 50 }
}
tiers = [
  { tier = 1, at = 100,  free = ["[message] @pass.tier.reached tier=1"] }
  { tier = 3, at = 500,  free = ["..."], premium = ["..."] }
]
```

| Key | Meaning |
|---|---|
| `premium` | The node that opens the premium track. Leave it empty for a pass with one track |
| `season.started`, `season.length` | When the season began and how long it runs; no `length` is a pass that never ends, no `started` begins at the epoch |
| `unclaimed.rule` | What happens to a reward earned and never taken when the season closes: `pay` pays it, `hold` keeps it for `for` into the next season, `lose` drops it |
| `tasks.groups`, `tasks.per-quest` | The quest groups whose finished quests feed the pass, and what each is worth |
| `tiers` | A list: `tier`, `at` (the experience it asks for), `free` and `premium` action lines |

- **Premium is a permission and nothing else.** The plugin never learns about money: a timed sale is a timed
  permission your permission plugin grants and expires. The premium node is not declared, so operators hold it.
- **The season resets when it is looked at.** Nothing sweeps a table at midnight. The season experience and the claims
  go; the all-time total stays.

## The shipped season pass

`passes/season.conf`: started 1 September 2026, 90 days, unclaimed rewards held 7 days, 50 experience per finished
daily quest. Five tiers at 100, 250, 500, 900 and 1500; tiers 3 and 5 have premium lines. Every line is a message, so
write the real rewards.

## Players

`/quests pass` lists every pass, the tier reached, the experience and the track. `/quests pass claim <pass>` takes every
reward reached and not yet taken, all tiers at once. The [passes and pass windows](../menus/passes.md) draw the list and
the ladder.

Related: [Pass experience and boosters](pass-experience.md), [Pass files](../config/pass-files.md)

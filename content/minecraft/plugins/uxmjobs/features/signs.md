---
title: Signs
order: 412
description: Join signs and top list signs, how to hang one, and how they are drawn.
icon: signpost
---

Write `[uxmjobs]` (`signs.marker`) on the first line of a sign and one of these on the second:

| Second line | Sign |
|---|---|
| `join <job>` | A door into a job: a click takes it, through the same rule as `/jobs join` |
| `top <job> <place>` | One place of that job's board; place 1 when none is written |

Hanging and taking down a job sign needs `uxmjobs.admin.sign`; without it the sign is refused with `sign.no-permission`.
A line the plugin cannot read is answered with `sign.unreadable`.

The four lines each kind then shows are `sign.join.1` to `4` and `sign.top.1` to `4` in the messages:

```
[Jobs]          [Top]
MINER           MINER
12 working      #1 Steve
Click to join   level 64
```

A sign is one text for everyone who walks past, so it is written in the default language. Every sign is written again
every `signs.refresh-seconds` (60), after a reload, and on `/uxmjobs sign update`; a top sign reads the leaderboard cache
and never the database. `signs.enabled = false` turns them off.

Related: [Leaderboards](leaderboards.md)

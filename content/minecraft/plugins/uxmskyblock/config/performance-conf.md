---
title: performance.conf
order: 635
description: How fast islands are cleared and put back.
icon: cpu
---

How fast an island is cleared and put back, so a reset or a restore never stalls the server.

Deleting, resetting or recycling an island clears its blocks a batch at a time, and restoring a backup puts them back the same way. While the server keeps up, the normal sizes below are used; once its ticks per second fall under tps-threshold, the throttled ones are, until it recovers.

The file is `plugins/uxmSkyblock/modules/performance.conf`. Every default below is the shipped value.

| Key | Default | What it does |
|---|---|---|
| `adaptive-throttle` | `true` | false always uses the normal sizes, however far behind the server is. |
| `tps-threshold` | `19.5` | Ticks per second under which the throttled sizes take over. 20 is a server keeping up. |
| `normal-blocks-per-tick` | `128` | Blocks cleared or put back each tick. Higher finishes sooner and costs more per tick. |
| `throttled-blocks-per-tick` | `16` |  |
| `normal-deletion-chunks-per-sec` | `100` | Chunks of an island whose clearing starts each second. Must be above 0. |
| `throttled-deletion-chunks-per-sec` | `20` |  |

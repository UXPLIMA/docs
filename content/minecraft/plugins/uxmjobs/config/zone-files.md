---
title: Zone files
order: 506
description: The keys of zones/<id>.conf, written by the wand.
icon: map
---

One zone per file under `zones/`, written whole by `/uxmjobs zone define`; the file name is the id.

| Key | Meaning |
|---|---|
| `name`, `description` | What a menu calls it; a block of language tags for one per language |
| `icon` | The material (`beacon`) |
| `box` | `world`, `min-x`, `min-y`, `min-z`, `max-x`, `max-y`, `max-z`; both corners are inside |
| `multipliers` | Per currency; one where none is written, zero is a no pay zone |
| `requirements` | Condition lines to be paid the bonus |
| `renewable` | `blocks`, `fallback`, `reset-seconds` (60), `drops` (true) |

A file edited by hand that is wrong fails loudly rather than becoming a zone with a multiplier of one. Delete the file to
delete the zone.

Related: [Zones](../features/zones.md)

---
title: Reference
order: 701
description: Every %uxmbackup_...% placeholder and what it answers.
icon: list
---

PlaceholderAPI is required; the identifier is `uxmbackup`. None is about the reader: a backup belongs to the server.

| Placeholder | Answer |
|---|---|
| `%uxmbackup_last_run%` | When the last run started, in the machine's time zone, or empty |
| `%uxmbackup_last_result%` | How it ended: `done`, `partial`, `failed` or `refused`, or empty |
| `%uxmbackup_last_size%` | What it wrote, such as `1.2 GB`, or empty |
| `%uxmbackup_next_run%` | When the soonest scheduled set is due, or empty when nothing is scheduled |
| `%uxmbackup_total_size%` | Everything every storage holds |
| `%uxmbackup_total_count%` | How many archives are kept, over every storage |

Related: [Integrations](../integrations/overview.md)

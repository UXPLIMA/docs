---
title: Reference
order: 801
description: Every %uxmsimpleclaims_...% placeholder.
icon: braces
---

With PlaceholderAPI. `integrations.placeholders = false` turns them off.

## Where the reader stands

| Placeholder | Gives |
|---|---|
| `%uxmsimpleclaims_here%` | The claim you stand in, or the word for wilderness |
| `%uxmsimpleclaims_here_owner%` | Its owner; empty in the wilderness |
| `%uxmsimpleclaims_here_role%` | Your role there |
| `%uxmsimpleclaims_here_size%` | Its chunks |
| `%uxmsimpleclaims_here_colour%` | Its map colour |
| `%uxmsimpleclaims_in_claim%` | `true` or `false` |

## What the reader owns

| Placeholder | Gives |
|---|---|
| `%uxmsimpleclaims_claims%` | Claims owned |
| `%uxmsimpleclaims_chunks%` | Chunks owned |
| `%uxmsimpleclaims_limit_claims%` | The claims limit |
| `%uxmsimpleclaims_limit_chunks%` | The total chunks limit |

## Boards

| Placeholder | Gives |
|---|---|
| `%uxmsimpleclaims_top_<board>_alltime_<n>_name%` | The player at place `n` |
| `%uxmsimpleclaims_top_<board>_alltime_<n>_value%` | Their score |
| `%uxmsimpleclaims_top_<board>_alltime_<n>_head%` | Their head |
| `%uxmsimpleclaims_rank_<board>_alltime%` | Your place |
| `%uxmsimpleclaims_value_<board>_alltime%` | Your score |

`<board>` is `chunks`, `members` or `visits`; see [Boards](../features/boards.md).

Related: [Integrations](../integrations/overview.md)

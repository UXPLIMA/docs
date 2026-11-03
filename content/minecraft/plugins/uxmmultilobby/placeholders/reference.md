---
title: Reference
order: 701
description: Every %uxmmultilobby_...% placeholder and what it answers.
icon: list
---

PlaceholderAPI is required; the identifier is `uxmmultilobby`. `<id>` is a lobby's id from `lobbies.conf`.

| Placeholder | Answer |
|---|---|
| `%uxmmultilobby_players_<id>%` | The count on that lobby |
| `%uxmmultilobby_maximum_<id>%` | The maximum that lobby reports, or the configured one |
| `%uxmmultilobby_status_<id>%` | `online`, `full`, `offline` or `unknown` |
| `%uxmmultilobby_total%` | The count over every configured lobby |
| `%uxmmultilobby_lobbies%` | How many lobbies are configured |

Every value is read from the cache, so a scoreboard does not open a socket. An unknown id leaves the placeholder as
written.

## Examples

```
Lobby 1: %uxmmultilobby_players_lobby-1%/%uxmmultilobby_maximum_lobby-1%
In the lobbies: %uxmmultilobby_total%
```

Related: [Live counts](../features/live-counts.md)

---
title: Overview
order: 501
description: Every window, its file and what opens it.
icon: layout-grid
---

| Window | File | Opened by |
|---|---|---|
| [Claims](claims.md) | `claims.conf` | `/claim menu` |
| [Map](claims.md#map) | `map.conf` | `/claim map`, the claims window |
| [Market](claims.md#market) | `market.conf` | `/claim market`, the claims window |
| [Inbox](claims.md#inbox) | `inbox.conf` | `/claim inbox`, the claims window |
| [Claim](claim.md) | `claim.conf` | `/claim` inside a claim you are in |
| [Flags](claim.md#flags) | `flags.conf` | The claim window |
| [Rules](claim.md#rules) | `rules.conf` | The claim window |
| [History](claim.md#history) | `audit.conf` | The claim window, `/claimadmin audit` |
| [Members](members.md) | `members.conf` | The claim window |
| [Bans](members.md#bans) | `bans.conf` | The members window |
| [Roles](members.md#roles) | `roles.conf` | The claim window |
| [Role](members.md#role) | `role.conf` | A role on the roles window |
| [Sub areas](areas.md) | `areas.conf` | The claim window |
| [Area](areas.md#area) | `area.conf` | A sub area on the sub areas window |

Every window is six rows, with paging at 48 and 50 and the back feather at 49. The words are not in the files: every
title and tile names a key of the language files. A tile is `tile:<colour> @menu.<block> <fact> <fact>`; take a fact
word off the line and that fact is not drawn. A Bedrock player gets forms.

Related: [Menu files](../config/menu-files.md)

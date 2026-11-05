---
title: temporary-access.conf
order: 648
description: "Trust: letting a visitor build for a while."
icon: key-round
---

Trust: an island's owner lets a player who is no member build and use things there for a while, with /is trust.

The file is `plugins/uxmSkyblock/modules/temporary-access.conf`. Every default below is the shipped value.

## `temporary-access`

| Key | Default | What it does |
|---|---|---|
| `enabled` | `true` | false turns /is trust off. |
| `default-duration` | `1h` | How long a trust lasts when the owner names no time. |
| `max-duration` | `24h` | The longest a trust may last. |
| `purge-interval` | `1m` | How often trusts that ran out are deleted. |
| `grant-lookup-ttl` | `30s` | How long a node may answer "who has a grant on this island" from memory. Every block a player who is not a member touches asks that question, so reading it from the database each time is a query per click. A node always knows the grants it made itself; this window is only how long it may take to notice one another node made. Set it to 0 to read every time. |
| `trust-permissions` | a list of 11 | What a trust grant lets the trusted player do, for as long as the grant lasts. Rewrite this list to decide what /is trust hands over. A key that is not a permission is skipped and the rest are kept. An empty list falls back to the shipped one. A trusted visitor is never a member: nothing here changes the island's settings, its membership or its money, and the ruleset rules still apply on top. |

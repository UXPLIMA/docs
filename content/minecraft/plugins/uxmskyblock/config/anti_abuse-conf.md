---
title: anti_abuse.conf
order: 612
description: "Starter item farming: new islands and second accounts."
icon: shield-alert
---

The file is `plugins/uxmSkyblock/modules/anti_abuse.conf`. Every default below is the shipped value.

## `anti-abuse`

Rules that keep players from farming the starter items with new islands or second accounts.

| Key | Default | What it does |
|---|---|---|
| `purge-inventory-on-reset` | `true` | true empties the inventory, armour, off hand, ender chest and experience of the island's members when it is reset or deleted, so a reset never brings the starter items twice. |
| `quarantine-duration` | `15m` | How long a new island is in quarantine: nothing can be dropped on it and visitors stay out, so a fresh island cannot be used to pass its starter items to another player. |
| `reset-cooldown` | `12h` | How long a player waits between two resets or deletions of their island. |
| `max-resets-per-day` | `3` | How many resets a player may make within reset-window-duration. |
| `reset-window-duration` | `24h` |  |
| `coop-join-cooldown` | `24h` | How long a player who left or was kicked from an island waits before joining another. |
| `quarantine-lookup-ttl` | `30s` | How long this node trusts its own answer that an island is not quarantined. Movement is checked against the quarantine list and a player sends many movement packets a second, so this node remembers a clean answer instead of asking the database each time. It always sees a quarantine it set itself at once; this only bounds how long it can take to notice one set by another node in a cluster. |
| `reset-challenge-ttl` | `60s` | How long a reset confirmation code stays good. It is how long a player has to read a four digit code and type it back before the island they are about to erase stops listening. |
| `reset-bypass-permission` | `uxmskyblock.bypass.resetlimits` | A player with one of these nodes skips the reset limits, the join cooldown or the quarantine. |
| `coop-bypass-permission` | `uxmskyblock.bypass.coopcooldown` |  |
| `quarantine-bypass-permission` | `uxmskyblock.bypass.quarantine` |  |

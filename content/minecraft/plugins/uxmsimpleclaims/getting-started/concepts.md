---
title: Concepts
order: 203
description: Claims, chunks, roles, flags, rules, sub areas and the wilderness.
icon: lightbulb
---

| Word | Means |
|---|---|
| Claim | One connected piece of chunks with one owner, a name, members, flags and rules |
| Chunk | The unit of land: 16 by 16, from a world's lowest protected block to its highest |
| Wilderness | Land outside every claim, held by `flags.wilderness` and the world's mode |
| Role | What a member may do; `visitor` is everybody who is not a member, `owner` holds everything |
| Priority | A role's rank: nobody acts on a member whose role is as high as their own |
| Flag | What happens on its own inside a claim: explosions, spread, spawns, PvP |
| Rule | A whitelist or blacklist of blocks or mobs on top of the roles |
| Sub area | Chunks inside a claim with their own members and flags, which may be rented |
| Snapshot | A deleted claim, kept `retention.snapshots-days` for staff to restore |

A claim is always one connected piece: a chunk that touches one of your claims joins it, one apart starts a new claim.
The owner is never held by a role, a rule or a ban.

Related: [Claiming](../features/claiming.md)

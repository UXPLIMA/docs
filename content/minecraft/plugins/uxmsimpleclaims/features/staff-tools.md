---
title: Staff tools
order: 410
description: Bypass, inspection, limits, snapshots, the audit trail and the inbox.
icon: user-cog
---

## Bypass and managing

`/claimadmin bypass` ignores every claim until run again. `uxmsimpleclaims.admin.manage` manages every claim as if its
holder owned it: the claim window opens for them on any claim.

## Inspection

`/claimadmin inspect` says who owns the chunk you stand in. `/claimadmin stick` hands the inspection stick: a click on
a block says who owns it.

## Snapshots

A deleted claim (by its owner, by staff, by tax, time or inactivity) is kept for `retention.snapshots-days` (30).
`/claimadmin snapshots [player]` lists them and `/claimadmin restore <snapshot>` brings one back.

## The audit trail

Every change to a claim is recorded and kept `retention.audit-days` (90). `/claimadmin audit` and the claim window's
history button show the latest changes to a claim: who, when and what.

## The inbox

What happened while a player was away: invites, sales, purchases, rents, rent paid or ended, kicks, bans, tax warnings
and chunks lost to tax, expiry warnings and expiry, role changes, transfers, inactivity removals and payouts. Kept
`retention.inbox-days` (30). `/claim inbox` or the claims window opens it; a button clears it.

## Profiles

`/claimadmin profile <player>` opens a player's claims, as they would see them.

Related: [Admin commands](../commands/admin.md)

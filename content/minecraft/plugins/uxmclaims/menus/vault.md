---
title: The vault
order: 410
description: The claim's shared storage, and the lock that keeps it consistent.
icon: archive
---

`menu/claim_vault.yml`, opened from claim management or by `/claim vault`.

A shared inventory belonging to the claim. Items placed here are visible to every member who may open
it, and are saved to the database when the vault closes.

| Requirement | |
|---|---|
| Role permission | `MANAGE_VAULT` |
| Ability node | `uxmclaims.ability.vault.access` |

## What it holds

Items go only into the slots listed in `itemSlots`; the rest of the window is decoration and the
`back` button. Shift-clicking from your own inventory is blocked, so drag or place items by hand.

Materials listed under `vault.blacklistedMaterials` in [config.yml](../config/config-yml.md) are
refused with `errorVaultBlacklistedItem`. The list ships empty; the config comment suggests
`PLAYER_HEAD` and `BUNDLE`.

## One at a time

The vault is locked while someone has it open. Anyone else who tries gets `errorVaultAlreadyInUse`
until the first viewer closes it. The lock exists because two players editing one inventory at once is
how items get duplicated. When the server stops, every open vault is closed and saved first.

The vault always opens as a chest, also for [Bedrock players](bedrock.md).

How many items it holds is available as a [placeholder](../placeholders/reference.md), and the shipped
hologram shows it.

<Callout type="warning" title="The vault is as safe as the role that opens it">

`MANAGE_VAULT` is one permission covering both reading and taking. There is no view-only mode. Grant
it to a custom role rather than to `Member`, and remember a per-member deny
(`/claim member setperm Steve MANAGE_VAULT false`) overrides the role if you need one exception.

</Callout>

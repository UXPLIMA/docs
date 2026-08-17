---
title: The vault
order: 410
description: The claim's shared storage, and the lock that keeps it consistent.
icon: archive
---

`menu/claim_vault.yml`, opened from claim management or by `/claim vault`.

A shared inventory belonging to the claim. Items placed here are visible to every member who may open
it, and persist across restarts in the database.

| Requirement | |
|---|---|
| Role permission | `MANAGE_VAULT` |
| Ability node | `uxmclaims.ability.vault.access` |

## One at a time

The vault is locked while someone has it open. Three different refusals exist, and they mean different
things:

| Message | Means |
|---|---|
| `errorVaultBusy` | Someone is looking at it right now |
| `errorVaultInUse` | Someone with a **higher-priority role** took it from you |
| `errorVaultAlreadyInUse` | It is locked; try again shortly |

Priority is the tie-break, which is the same ordering that drives promotion — see
[Roles](../commands/roles.md). An owner can always take the vault back from a member.

The lock exists because two players editing one inventory concurrently is how items get duplicated.

## Contents in placeholders

| Placeholder | Renders |
|---|---|
| `%uxmclaims_count:claim_vault_items%` | How many items are in the vault |

The shipped hologram uses it — `Vault: %count:claim.vault.items% items` over the claim block.

<Callout type="warning" title="The vault is as safe as the role that opens it">

`MANAGE_VAULT` is one permission covering both reading and taking. There is no view-only mode. Grant
it to a `Trusted` role rather than to `Member`, and remember a per-member deny
(`/claim member setperm Steve MANAGE_VAULT false`) overrides the role if you need one exception.

</Callout>

---
title: Claim management
order: 403
description: The settings panel for one claim, and everything reachable from it.
icon: settings-2
---

The main screen for a claim you belong to. Opened by `/claim` while standing in it, or by clicking the
claim in your claims list.

Layout: `menu/claim_main.yml`, six rows, titled with the claim's name.

| Button | What it does |
|---|---|
| **Spawn point** | Click to teleport, shift-click to set the spawn where you stand |
| **Flags** | Opens the [flags screen](flags.md) |
| **Rename** | Prompts in chat for a new name |
| **Chunk map** | Opens the [chunk map](claim-map.md) |
| **Members** | Opens [members and roles](members-roles.md) |
| **Invitations** | Opens [pending invitations](invitations.md) |
| **Banned players** | Opens the [ban list](bans.md) |
| **Warps** | Opens the [warp list](warps.md) |
| **Vault** | Opens the [shared vault](vault.md) |
| **Claim block** | Choose the style marking the claim in the world |
| **Time** | Extend the claim's lifetime |
| **Delete** | Deletes the claim, after confirmation |

## What each button needs

Buttons respect [role permissions](../protection/permissions.md). A member without the permission
either does not see the button or is refused on click.

| Button | Role permission | Ability node |
|---|---|---|
| Set spawn | `MANAGE_RELOCATE` | `uxmclaims.ability.claim.relocate` |
| Rename | `MANAGE_RENAME` | `uxmclaims.ability.claim.rename` |
| Flags | — | `uxmclaims.ability.claim.flag.<FLAG>` |
| Chunk map | `MANAGE_CHUNKS` to change anything | `uxmclaims.ability.chunk.*` |
| Members | `MANAGE_INVITES` for invites, `…member.role` to move people | see [abilities](../permissions/abilities.md) |
| Banned players | `MANAGE_BANS` | `uxmclaims.ability.member.ban` / `.unban` |
| Warps | `MANAGE_WARPS` | `uxmclaims.ability.warp.*` |
| Vault | `MANAGE_VAULT` | `uxmclaims.ability.vault.access` |
| Claim block | `MANAGE_BLOCK` | `uxmclaims.ability.block.*` |
| Time | `MANAGE_TIME` | `uxmclaims.ability.claim.reschedule` |
| Delete | Owner only | `uxmclaims.ability.claim.delete` |

## Rename and time

Both prompt in chat rather than in an anvil. The plugin sends `infoChatInputInstruction` — *"Type your
input in chat. Type 'cancel' to cancel."* — and waits.

Time extension is priced by `uxmclaims.cost.time` **per second**, defaulting to `5.0`. A week is
604 800 seconds, so at the shipped price that is three million of your currency. Set the cost node
before you let players near the button.

## Claim block

`menu/claim_block.yml` and `claim_block_change.yml`. Lists the styles from the `blocks` section of
`config.yml`, filtered by the permission each carries. The block must sit inside the main chunk, and
it is what carries the hologram.

---
title: Overview
order: 201
description: How the command tree is shaped, which checks every command passes, and what runs from the console.
icon: list-tree
---

Every command lives under `/claim`. The root and its direct subcommands handle the claim itself, and
seven sub-roots group the rest.

| Root | Page |
|---|---|
| `/claim`, `/claim <subcommand>` | [Claim commands](claim.md) |
| `/claim chunk`, `/claim block` | [Chunks and the claim block](chunks.md) |
| `/claim member` | [Members](members.md) |
| `/claim invite` | [Invites](invites.md) |
| `/claim role` | [Roles](roles.md) |
| `/claim warp` | [Warps](warps.md) |
| `/claim admin`, `/claim reload` | [Admin](admin.md) |

## Commands

### Which claim a command acts on

Commands that act on "the current claim" resolve it in this order:

1. The claim you selected with `/claim select`, or that `/claim create` just selected for you.
2. The claim you are standing in.

If neither exists you get `errorNoClaimSelected`. A selection lasts until you log out, delete or leave
that claim, or deselect it in the `/claim list` menu.

<Callout type="warning" title="A selection outlives where you stand">

After `/claim select` or `/claim create`, commands act on the selected claim even while you stand in
another one. Check the selection in `/claim list` before running `/claim delete`, a kick, or a
transfer.

</Callout>

### Arguments

The last text argument of a command takes the rest of the line, so `/claim select Claim of Steve`
and `/claim rename North Base` work with spaces. A name in the middle of a command, such as the role in
`/claim role rename <role> <name>`, must be one word. Names of flags, role permissions, roles, warps
and claims match without regard to case.

### Confirmation

These commands ask first and wait for `/claim confirm` or `/claim cancel`. The prompt is
`confirmationRequired` in `messages.yml`, with clickable buttons.

| Command | Asks first |
|---|---|
| `/claim` on free land, creating a new claim | yes |
| `/claim delete` | yes |
| `/claim kick`, `/claim member kick` | yes |
| `/claim transfer`, `/claim member transfer` | yes |
| `/claim chunk remove` | yes |
| `/claim role delete` | yes |
| `/claim warp delete` | yes |
| `/claim warp teleport`, `/claim spawn <name>` to a public warp in a claim with `PVP` on | yes, with `warningWarpPvpEnabled` |
| `/claim ban`, every `/claim admin bulk` command | no |

A prompt expires after 30 seconds. Starting another confirmable command replaces the pending one.
`generalSettings.confirmationEnabled: false` in `config.yml` turns every prompt off, and the
commands run at once.

### Console

Only `/claim reload` and the `/claim admin bulk` commands run from the console. Every other command
needs a player, because it reads the player's position, selection or menus.

## Permissions

A command that changes a claim passes up to two checks, and both must pass:

| Check | What it is | Question |
|---|---|---|
| Ability | A server permission, `uxmclaims.ability.<category>.<action>` | May this player ever do this, anywhere? |
| Claim access | A [role permission](../protection/permissions.md) on the player's role, or ownership of the claim | May they do it in this claim? |

Some actions skip the role permission and require ownership instead: deleting and transferring the
claim, flags, role management, and assigning roles or permission overrides to members. Each command
page lists which check applies. The ability nodes, their wildcards and when they are read are in
[Ability permissions](../permissions/abilities.md).

`uxmclaims.admin` (default `op`, the only node `plugin.yml` declares) passes both checks, every limit,
cost and cooldown. A `uxmclaims.bypass.*` node passes the ability check only.

Limits, costs and cooldowns come from `uxmclaims.limit.*`, `uxmclaims.cost.*` and `uxmclaims.delay.*`
nodes, resolved through [entitlements.yml](../config/entitlements-yml.md). The full table is in
[Limits, costs and delays](../permissions/limits.md).

## Settings

| Key | File | Default | Effect |
|---|---|---|---|
| `generalSettings.smartCommand` | `config.yml` | `true` | What a bare `/claim` does on free land |
| `generalSettings.confirmationEnabled` | `config.yml` | `true` | Confirmation prompts on or off |
| `aliases` | `aliases.yml` | four aliases | Top-level shortcuts for `/claim` subcommands |

## Notes

- **Aliases.** `aliases.yml` ships `/accept` (`claim invite accept`), `/deny`
  (`claim invite reject`), `/unclaim` (`claim delete`) and `/untrust` (`claim kick`). Arguments pass
  through and tab completion follows the target. `/claim reload` applies a change to the file. See
  [aliases.yml](../config/aliases-yml.md).
- **Replies come from `messages.yml`.** A refusal such as `errorNoPermission` or
  `errorAbilityPermissionDenied` is a key in [messages.yml](../config/messages-yml.md), in the
  language set in `config.yml`. The replies of `/claim reload`, the bulk commands and an unreadable
  `/claim extend` duration are fixed English text.

Related: [Claim commands](claim.md), [Ability permissions](../permissions/abilities.md),
[Role permissions](../protection/permissions.md)

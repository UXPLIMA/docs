---
title: Claim commands
order: 202
description: The root command and the subcommands that create, run and leave a claim.
icon: land-plot
---

These commands sit directly under `/claim`. They cover the life of a claim, flags, spawn, the vault
and claim chat, plus short forms of the member commands. "Current claim" means the selected claim or
the one you stand in: see [Overview](overview.md#which-claim-a-command-acts-on).

## Commands

### The bare command

With `generalSettings.smartCommand: true`, `/claim` on its own decides what you meant:

| Where you stand | Result |
|---|---|
| In a claim you own or belong to | Opens the claim management menu |
| In someone else's claim | Opens your personal menu |
| On free land next to (north, south, east or west of) a claim you own | Adds this chunk to that claim at once, at the normal chunk price |
| On free land anywhere else | Asks for confirmation, then creates a claim and selects it |

With `smartCommand: false`, the first two rows still apply and free land opens your personal menu.
`/claim menu` always opens the personal menu.

### Lifecycle

| Command | What it does | Claim access | Ability node |
|---|---|---|---|
| `/claim create [name]` | Creates a claim on the chunk you stand in and selects it | none | `uxmclaims.ability.claim.create` |
| `/claim delete` | Deletes the current claim, after confirmation | owner | `uxmclaims.ability.claim.delete` |
| `/claim rename <name>` | Renames it | `MANAGE_RENAME` | `uxmclaims.ability.claim.rename` |
| `/claim extend <duration>` | Adds time before the claim expires | `MANAGE_TIME` | `uxmclaims.ability.claim.reschedule` |
| `/claim setspawn` | Sets the claim spawn to your position | `MANAGE_RELOCATE` | `uxmclaims.ability.claim.relocate` |
| `/claim transfer <player>` | Hands ownership to a member, after confirmation | owner | `uxmclaims.ability.claim.transfer` |
| `/claim leave` | Leaves a claim you belong to | member | none |

- `create` without a name uses `claimSettings.defaultName`. A claim name needs at least three
  characters. Creation is refused on claimed land, within `claimSettings.minDistance` chunks of
  another player's claim, inside a WorldGuard region or a world in `generalSettings.disabledWorlds`,
  over the claim limit, or without the money for the claim cost.
- `extend` takes a compound duration such as `30m`, `12h`, `7d` or `1d12h30m`, with units `d`, `h`,
  `m` and `s` in any case. It is refused when `claimSettings.expireMode` is `false`, and when the
  remaining time would pass the owner's `uxmclaims.limit.time`.
- `setspawn` and the warp commands only accept a position inside the claim's chunks.
- `transfer` checks that you are the owner (an admin who is not the owner is refused too), that the
  target is not you, and that the target is already a member. The new owner's claim limit applies.
  You keep membership with the `Member` role.
- The owner cannot `leave`; transfer the claim or delete it.

### Selection

| Command | What it does |
|---|---|
| `/claim select` | Opens a picker of the claims you own or belong to |
| `/claim select <name>` | Selects one of those claims by name |
| `/claim list` | Opens the list of your claims, where you can also deselect |

### Members and bans

| Command | What it does | Claim access | Ability node |
|---|---|---|---|
| `/claim trust <player>` | Sends an invite, same as `/claim invite send` | `MANAGE_INVITES` | `uxmclaims.ability.member.invite` |
| `/claim kick <player>` | Removes a member, after confirmation | `MANAGE_BANS` | `uxmclaims.ability.member.kick` |
| `/claim ban <player>` | Bans a player at once and removes them if they are a member | `MANAGE_BANS` | `uxmclaims.ability.member.ban` |
| `/claim unban <player>` | Lifts a ban | `MANAGE_BANS` | `uxmclaims.ability.member.unban` |

The owner cannot be kicked or banned. A banned player cannot enter, teleport into, or be invited to
the claim. The rest of member management is on [Members](members.md) and [Invites](invites.md).

### Flags

| Command | What it does | Claim access | Ability node |
|---|---|---|---|
| `/claim setflag <flag> <true\|false>` | Turns one flag on or off | owner | `uxmclaims.ability.claim.flag.<flag>` |
| `/claim toggleflag <flag>` | Flips one flag | owner | `uxmclaims.ability.claim.flag.<flag>` |

`<flag>` is one of the 32 [flag names](../protection/flags.md), such as `PVP` or `TNT_EXPLOSIONS`.
The ability node uses the lower-case name, for example `uxmclaims.ability.claim.flag.pvp`, and
`uxmclaims.ability.claim.flag.*` covers all of them.

### Spawn, warps and the vault

| Command | What it does | Claim access | Ability node |
|---|---|---|---|
| `/claim spawn` | Teleports you to the current claim's spawn | none | none |
| `/claim spawn <name>` | Teleports you to a warp or a claim by name | none | none |
| `/claim warps` | Opens the list of public warps | none | none |
| `/claim vault` | Opens the claim vault | `MANAGE_VAULT` | `uxmclaims.ability.vault.access` |

`/claim spawn <name>` looks for a public warp, then a warp in a claim you own, then a warp in a claim
you belong to, then a claim you own or belong to, and takes the first match. A public warp in a claim
with `PVP` on asks for confirmation first. Warp teleports cost what
[Warps](warps.md#permissions) lists. Every teleport waits out the warmup, and moving cancels it.

One player at a time can have a claim's vault open; anyone else gets `errorVaultAlreadyInUse`.

### Chat

| Command | What it does |
|---|---|
| `/claim chat` | Turns claim chat mode on or off for the current claim |
| `/claim chat <message>` | Sends one message to the claim without changing the mode |

Both need the `CLAIM_CHAT` role permission, which only the owner has in the shipped `roles.yml`. A
message reaches online players who have claim chat mode on and hold `CLAIM_CHAT` in that claim, plus
every online admin. When nobody receives it you get `errorClaimChatNoOnlineMembers`. The format is
`claimChatFormat` in `messages.yml`.

### Utility

| Command | What it does |
|---|---|
| `/claim menu` | Opens your personal menu |
| `/claim showborders` | Shows or hides the borders of the claim or chunk you stand in |
| `/claim invites` | Opens your invitation inbox |
| `/claim confirm` | Runs the pending confirmation |
| `/claim cancel` | Drops the pending confirmation |
| `/claim reload` | Reloads the configuration, see [Admin](admin.md#reload) |

## Permissions

| Node | Applies to | Default |
|---|---|---|
| `uxmclaims.limit.claim.<n>` | Claims a player may own, checked on `create` and for the `transfer` target | `1` |
| `uxmclaims.cost.claim.<level>.<price>` | Price of a new claim | `0.0` |
| `uxmclaims.cost.rename.<price>` | Price of a rename | `0.0` |
| `uxmclaims.cost.time.<price>` | Price of `extend`, per second and per chunk | `5.0` |
| `uxmclaims.limit.time.<n>` | Most seconds a claim may have left after `extend` | `2592000` |
| `uxmclaims.limit.ban.<n>` | Bans per claim | `25` |
| `uxmclaims.delay.rename.<n>` | Seconds between renames | `30` |
| `uxmclaims.delay.relocate.<n>` | Seconds between `setspawn` uses | `60` |
| `uxmclaims.delay.transfer.<n>` | Seconds between transfers | `300` |
| `uxmclaims.delay.flags.<n>` | Seconds between flag changes | `5` |
| `uxmclaims.delay.teleport.<n>` | Teleport warmup in seconds | `3` |

Limits on a claim use the owner's nodes; costs and cooldowns use the nodes of the player who runs the
command. See [Limits, costs and delays](../permissions/limits.md).

<Callout type="warning" title="Extending a claim is expensive by default">

`uxmclaims.cost.time` ships at `5.0` per second per chunk, so one extra day on a one-chunk claim costs
432000. Set a lower `defaultValue` in [entitlements.yml](../config/entitlements-yml.md) before
players use `/claim extend` or the time menu.

</Callout>

## Settings

| Key in `config.yml` | Default | Effect |
|---|---|---|
| `generalSettings.smartCommand` | `true` | What a bare `/claim` does on free land |
| `claimSettings.defaultName` | `Claim of %player.name%` | Name of a claim created without one |
| `claimSettings.minDistance` | `2` | Chunks kept free between your claim and another player's |
| `claimSettings.expireMode` | `true` | Claims expire; `false` also disables `extend` |
| `border.autoCloseSeconds` | `30` | When `showborders` hides the borders again, `0` never |

## Notes

- `/claim spawn` with no name costs nothing. A warp teleport charges after the warmup.
- `/claim extend` prints a fixed English line, `Invalid duration format!`, when nothing in the input
  parses as a duration. Every other reply comes from `messages.yml`.
- The menus run the same checks as these commands: see [Claim management](../menus/claim-management.md).

Related: [Overview](overview.md), [Chunks and the claim block](chunks.md), [Flags](../protection/flags.md)

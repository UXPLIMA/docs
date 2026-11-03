---
title: Flags and rules
order: 405
description: The thirteen flags, their defaults, locks and nodes, and the block and mob lists.
icon: toggle-left
---

## Flags

What happens on its own inside a claim.

| Flag | Default |
|---|---|
| `explosions` | false |
| `fire-spread` | false |
| `liquid-flow` | true |
| `growth` | true |
| `monster-spawn` | true |
| `animal-spawn` | true |
| `mob-grief` | false |
| `pvp` | false |
| `mob-damage` | true |
| `fall-damage` | true |
| `snow-and-ice` | true |
| `outside-redstone` | false |
| `outside-pistons` | false |

`flags.claim-defaults` sets them for every claim that has not changed one; `flags.locked` keeps a flag out of owners'
hands, though staff still can. A player changes a flag only with `uxmsimpleclaims.flag.<flag>`, which everybody holds.
In the [flags window](../menus/claim.md#flags) left click turns one over, right click puts the default back. A sub area
has flags of its own (`/claim areaflag`). `/claimadmin resetflags` puts a claim's flags back.

## Rules

Lists of blocks or mobs on top of the roles, one per target:

| Target | Holds |
|---|---|
| `block-break` | Blocks visitors may or may not break |
| `block-place` | Blocks they may or may not place |
| `block-use` | Blocks they may or may not use |
| `mob-spawn` | Mobs that may or may not spawn |
| `mob-interact` | Mobs that may or may not be handled |

A whitelist lets only what it names through; a blacklist stops what it names. The owner is never held; spawning holds
for everybody. In the [rules window](../menus/claim.md#rules) left click switches the mode, right click adds or removes
what you hold, shift right click clears the list. It needs the role permission `manage-rules`.

Related: [config.conf](../config/config-conf.md#flags)

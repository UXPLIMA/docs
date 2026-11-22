---
title: LuckPerms
order: 1344
description: Limits and abilities read through LuckPerms for offline players too, and uxmclaims contexts for LuckPerms permissions.
icon: key-round
---

The luckperms module reads uxmClaims permissions through LuckPerms instead of Bukkit, so limits, costs and
abilities can be read for offline players too, and an unset ability follows a rule you choose. It also gives LuckPerms contexts such as `uxmclaims:in-land`, so
you can grant a permission only while a player stands in a land, or only to town mayors.

Module `luckperms` · enabled by default · `modules/luckperms/config.yml` · requires [Lands](lands.md) · needs LuckPerms

## Commands

| Command | What it does | Permission |
|---|---|---|
| `/uxm luckperms contexts <player>` | Shows the `uxmclaims:` contexts LuckPerms sees for an online player. | `uxmclaims.admin.luckperms` |

## Permissions

| Node | Default | Grants |
|---|---|---|
| `uxmclaims.admin.luckperms` | op | `/uxm luckperms contexts`. |

## Settings

| Key | Default | What it does |
|---|---|---|
| `undefined-ability` | `allow` | What an ability node (`uxmclaims.ability.*`) that LuckPerms does not set means. `allow`: everyone may, until a group takes it away. `deny`: nobody may, until a group gives it (3.x behaviour). |
| `contexts.enabled` | `true` | Give LuckPerms the `uxmclaims:` contexts. |
| `contexts.disabled` | `[]` | Context keys to leave out, without the `uxmclaims:` prefix, for example `land-role`. |

## Contexts

| Context | Values | From |
|---|---|---|
| `uxmclaims:in-land` | `true`, `false` | The player stands in a land. |
| `uxmclaims:wilderness` | `true`, `false` | The player stands outside every land. |
| `uxmclaims:land-role` | the role name in lower case, such as `owner`, `member`, `visitor` or a custom role's name | The player's role in the land underfoot. Not set in the wilderness. LuckPerms' editor is offered the role names the lands use (up to 200). |
| `uxmclaims:land-owner` | `true`, `false` | The player owns the land underfoot. |
| `uxmclaims:trusted-here` | `true`, `false` | The player is a member of the land underfoot. |
| `uxmclaims:town`, `uxmclaims:town-rank` | town name, rank name, in lower case | The player's town and rank, with [Towns](towns.md) on. |
| `uxmclaims:nation`, `uxmclaims:nation-rank` | nation name, rank name, in lower case | The player's nation and rank, with [Nations](nations.md) on. |

Use a context like any other LuckPerms context:

```
/lp group default permission set essentials.fly true uxmclaims:land-owner=true
/lp group default permission set essentials.sethome false uxmclaims:wilderness=true
```

The contexts follow the player as they walk: LuckPerms is told to recalculate when the player enters or leaves
a land, or when the land underfoot changes.

## Notes

- `undefined-ability` applies to the ability checks of lands, areas, towns, nations, warps, land vault and
  claim block.
- 3.x treated an unset ability node as denied. The migration does not change this setting, so set
  `undefined-ability: deny` yourself after moving from 3.x if your groups rely on that.
- An offline player LuckPerms has not loaded is loaded on demand. Reads off the server thread wait up to two
  seconds for it; a read on the server thread never waits, counts every node as unset that once and starts the
  load, so the next read answers.
- Price nodes of [Economy](economy.md) and an offline player's `uxmclaims.bypass.jail` are read through
  LuckPerms too, contexts included.
- `/uxm luckperms contexts` takes online players only. It says when contexts are switched off.

Related: [Limits](limits.md), [Permissions](../permissions.md), [Towns](towns.md), [Nations](nations.md), [Integrations](../integrations.md)

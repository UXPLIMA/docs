---
title: LuckPerms
order: 1510
description: What LuckPerms adds, and why a permission plugin stays optional.
---

uxmEssentials never hard-depends on a permission plugin. Every check goes through an internal port on top
of Bukkit's `hasPermission`, so the defaults work on a bare server.

## What LuckPerms Adds

Most permissions are plain on/off nodes: those resolve through Bukkit exactly the
same whether or not LuckPerms is present. Boolean `has()` checks **always** delegate
to Bukkit.

The difference is the **value-bearing quota nodes**: the ones with a number baked
into them:

| Node | Meaning |
|------|---------|
| `uxmessentials.home.limit.<n>` | How many homes a player may set |
| `uxmessentials.vault.amount.<n>` / `.vault.size.<rows>` | Vault count / rows per vault |
| `uxmessentials.pwarp.limit.<n>` | Player-warp quota |
| `uxmessentials.tp.cooldown.<seconds>` / `.tp.warmup.<seconds>` | Teleport cooldown / warmup tier |
| `uxmessentials.kit.cooldown.<seconds>` | Kit cooldown tier |

Without LuckPerms these resolve from Bukkit **numbered permission nodes** (the
highest matching `<n>` wins). With LuckPerms present, uxmEssentials can *also* read
the same quota from a LuckPerms **meta** value, which many admins find tidier than
juggling dozens of numbered nodes.

## Granting a Quota

The numbered-node approach works everywhere, LuckPerms or not:

```
/lp user Steve permission set uxmessentials.home.limit.5 true
```

With LuckPerms, the equivalent quota can be supplied as a **meta** value instead of
a numbered node:

```
# meta value carrying the same home quota
/lp user Steve meta set <home-limit-key> 5
```

<Callout type="warning" title="Confirm the exact meta key in-game">

uxmEssentials reads quotas from LuckPerms meta, but the precise meta key names
are resolved at runtime and are **not fixed in this guide**. Grant the value,
then verify with `/uxmess doctor` or by checking the affected limit in-game. If
in doubt, the numbered permission node above is the guaranteed path.

</Callout>

When LuckPerms is absent, the meta source is simply empty and quotas fall back to
numbered nodes with no change in behavior.

## Prefixes, Suffixes and Groups

Two extra conveniences when LuckPerms is around:

- **Prefixes / suffixes in the HUD.** LuckPerms publishes `%luckperms_prefix%` and
  `%luckperms_suffix%` through PlaceholderAPI. Because uxmEssentials
  [resolves PAPI tokens](placeholderapi.md) inside scoreboards, the tablist and
  nametags, you can display a player's rank prefix anywhere in the HUD:

  ```
  %luckperms_prefix%%player%
  ```

  This flows through the PlaceholderAPI integration, not a direct LuckPerms hook.

- **Group lists in menus.** The menu engine can source a `luckperms-groups` list and
  gate items with a `has-group:` requirement, so a GUI can show or hide options per
  rank.

## When It's Absent

| Situation | Behavior |
|-----------|----------|
| No permission plugin | Bukkit defaults apply: self-service verbs work, staff verbs need `op`. |
| No LuckPerms specifically | Quotas resolve from numbered permission nodes; the meta source is `none()`. |
| `has-group:` used without LuckPerms | The condition fails closed (item hidden / denied). |

## Setup Checklist

1. Install **LuckPerms** (optional: the plugin runs fine without it).
2. Grant quotas either as numbered nodes (`uxmessentials.home.limit.<n>`) or as
   LuckPerms meta.
3. (Optional) Install PlaceholderAPI to surface `%luckperms_prefix%` in the HUD.
4. Verify limits with `/uxmess doctor`.

Related: [Core Concepts: Permissions](../getting-started/concepts.md), [Scoreboard, Tablist & Nametags](../modules/scoreboard.md), [PlaceholderAPI](placeholderapi.md)

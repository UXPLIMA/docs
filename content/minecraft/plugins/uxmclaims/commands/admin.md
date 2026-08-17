---
title: Admin
order: 208
description: Inspecting other players' claims, and the bulk operations.
icon: shield-check
---

Every command here requires `uxmclaims.admin`, declared in `plugin.yml` with `default: op`. That node
also bypasses every ability check and every role permission everywhere.

| Command | What it does |
|---|---|
| `/claim admin view <player>` | Open a menu of every claim that player owns or belongs to |
| `/claim reload` | Re-read every config file |

## Bulk operations

These act on **every claim currently loaded**, not on one. They exist for the moment a server changes
policy (a new flag default, a rebalanced role) and does not want to edit thousands of claims by
hand.

| Command | What it does |
|---|---|
| `/claim admin bulk setflag <flag> <true\|false>` | Set one flag on every claim |
| `/claim admin bulk setrolepermission <role> <perm> <true\|false>` | Set one permission on one system role, in every claim |
| `/claim admin bulk rename <base>` | Rename every claim to `base-1`, `base-2`, … |
| `/claim admin bulk setblock <style>` | Change every claim's visual block |
| `/claim admin bulk destroyblock` | Remove every claim's visual block |

`setrolepermission` completes against the standard roles (the three system roles) because a custom
role name means something different in every claim.

<Callout type="danger" title="Bulk commands do not confirm and cannot be undone">

There is no confirmation step and no rollback. `bulk rename` in particular discards every name every
player chose, permanently, and `bulk setflag PVP true` turns PvP on inside every base on the server at
once.

Take a database backup before running any of them. On a live server, run them during a restart window
rather than at peak, since each one writes every loaded claim.

</Callout>

## Bypass nodes

Beyond `uxmclaims.admin`, individual actions can be bypassed:

| Node | Effect |
|---|---|
| `uxmclaims.bypass.*` | Bypass every ability check |
| `uxmclaims.bypass.<category>.*` | Bypass a whole category, e.g. `uxmclaims.bypass.warp.*` |
| `uxmclaims.bypass.<action>` | Bypass one, e.g. `uxmclaims.bypass.claim.delete` |
| `uxmclaims.bypass.teleport` | Ignore the teleport warmup |

Bypass nodes mirror ability nodes exactly: swap `ability` for `bypass`. See
[Ability permissions](../permissions/abilities.md).

Give staff `uxmclaims.bypass.*` rather than `uxmclaims.admin` when you want them to work everywhere
without also silently passing every in-claim role permission check.

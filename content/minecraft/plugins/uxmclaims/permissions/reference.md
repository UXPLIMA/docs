---
title: Reference
order: 601
description: Every permission node uxmClaims reads, and what each one unlocks.
icon: list
---

`plugin.yml` declares one permission, the same in the Bukkit and the Folia build. uxmClaims reads
every other node from your permission plugin by name, so a node you never grant is simply absent.

## What reads the nodes

| `generalSettings.permissionSupport` | LuckPerms | Ability nodes | Limit, cost and delay nodes |
|---|---|---|---|
| `true` (default) | Installed | Checked through LuckPerms | Read from the player's LuckPerms nodes |
| `true` | Missing | Every ability is granted | Every player gets `defaultValue` |
| `false` | Either | Every ability is granted | Every player gets `defaultValue` |

Without LuckPerms the console logs `Permission support is enabled but LuckPerms is not installed!`.
`uxmclaims.admin`, `uxmclaims.bypass.enter`, `uxmclaims.bypass.teleport` and the claim block style
nodes are checked through Bukkit in every row of this table.

uxmClaims asks LuckPerms without a context, so a node you set only for one world or one server is
not seen by the ability, limit, cost and delay checks. Grant those nodes globally.

## uxmclaims.admin

| Node | Default | Declared in |
|---|---|---|
| `uxmclaims.admin` | op | `plugin.yml` |

An operator counts as admin even without the node. An admin:

| Gets | Where |
|---|---|
| `/claim admin …` and `/claim reload` | [Admin commands](../commands/admin.md) |
| Every ability, with no node checked | [Ability permissions](abilities.md) |
| Every [role permission](../protection/permissions.md) in every claim | Protection, menus, commands |
| Unlimited limits, free costs, no delays or cooldowns (with LuckPerms) | [Limits and costs](limits.md) |
| Every claim in the claim list and selection menus | `/claim list`, `/claim select` |
| Every claim's chat, whether in chat mode or not | `/claim chat` |
| Freedom from `disabledCommandsInClaim` | [config.yml](../config/config-yml.md) |

## The families

| Family | Shape | Page |
|---|---|---|
| Abilities | `uxmclaims.ability.<category>.<action>` | [Ability permissions](abilities.md) |
| Ability bypasses | `uxmclaims.bypass.<category>.<action>` | below |
| Movement bypasses | `uxmclaims.bypass.enter`, `uxmclaims.bypass.teleport` | below |
| Limits | `uxmclaims.limit.<kind>.<amount>` | [Limits and costs](limits.md) |
| Costs | `uxmclaims.cost.<kind>.…<price>` | [Limits and costs](limits.md) |
| Delays and cooldowns | `uxmclaims.delay.<kind>.<seconds>` | [Limits and costs](limits.md) |
| Claim block styles | Whatever `blocks.<key>.permission` says | below |

## Bypasses

Every ability node has a bypass twin: replace `ability` with `bypass`.

| Node | Effect |
|---|---|
| `uxmclaims.bypass.*` | Passes every ability check |
| `uxmclaims.bypass.<category>.*` | Passes a category, for example `uxmclaims.bypass.warp.*` |
| `uxmclaims.bypass.<category>.<action>` | Passes one, for example `uxmclaims.bypass.claim.delete` |
| `uxmclaims.bypass.enter` | Exempt from the access guard: never ejected from a claim for a ban or a missing `MOVE_INSIDE` |
| `uxmclaims.bypass.teleport` | Teleports into any claim: banned claims, missing `TELEPORT`, `ENDERPEARL_USE` or `CHORUS_TELEPORT` |

An ability bypass is checked before the ability itself, so it wins even where the ability node is
denied. It does not touch [role permissions](../protection/permissions.md): a player with
`uxmclaims.bypass.*` still needs the right role in a claim to change it. `uxmclaims.bypass.teleport`
does not shorten the teleport warmup; `uxmclaims.delay.teleport.<n>` does.

With LuckPerms, `uxmclaims.bypass.*` also matches `uxmclaims.bypass.enter` and
`uxmclaims.bypass.teleport` through LuckPerms' wildcard handling.

## Claim block styles

Each entry in the `blocks` section of `config.yml` may name its own permission. The shipped `block2`,
obsidian, uses:

| Node | Grants |
|---|---|
| `claim.blockchange.obsidian` | Use the obsidian claim block style |

The node is a config value, not a plugin constant: rename it to whatever fits your permission tree.
A style with an empty `permission` is open to everyone. The check runs when a player changes the
style, or places a block of a style other than the claim's current one.

## A worked example

```yaml
default:
  - uxmclaims.ability.claim.create
  - uxmclaims.ability.claim.delete
  - uxmclaims.ability.claim.rename
  - uxmclaims.ability.chunk.extend
  - uxmclaims.ability.chunk.shrink
  - uxmclaims.ability.member.invite
  - uxmclaims.ability.member.kick
  - uxmclaims.ability.vault.access
  - uxmclaims.limit.claim.1
  - uxmclaims.limit.chunk.20

vip:
  - uxmclaims.ability.warp.*
  - uxmclaims.ability.role.*
  - uxmclaims.ability.claim.flag.*
  - uxmclaims.limit.claim.2
  - uxmclaims.limit.chunk.40
  - uxmclaims.delay.teleport.1
  - claim.blockchange.obsidian

staff:
  - uxmclaims.bypass.*
  - uxmclaims.bypass.enter
  - uxmclaims.bypass.teleport
```

`staff` can walk and teleport into any claim and use every command on claims where they hold a
role, whatever their rank's ability nodes say. They cannot change a stranger's claim, and
`/claim admin bulk …` stays out of their reach, because neither comes from a bypass.

<Callout type="warning" title="uxmclaims.admin is not a moderation rank">

It passes every role permission in every claim and unlocks the `bulk` commands that rewrite every
claim on the server. Give it to owners. Give moderators the bypass nodes instead.

</Callout>

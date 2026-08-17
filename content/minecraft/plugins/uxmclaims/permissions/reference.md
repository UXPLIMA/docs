---
title: Reference
order: 601
description: Every permission uxmClaims reads, in one table.
icon: list
---

uxmClaims declares exactly one permission in `plugin.yml`. Everything else is read from your
permission plugin as a string, which means unlisted nodes are simply absent until you grant them.

| Node | Default | Grants |
|---|---|---|
| `uxmclaims.admin` | op | Every admin command, and bypasses every ability check and every role permission in every claim |

## The four families

| Family | Shape | Page |
|---|---|---|
| Abilities | `uxmclaims.ability.<category>.<action>` | [Ability permissions](abilities.md) |
| Bypasses | `uxmclaims.bypass.<category>.<action>` | below |
| Limits | `uxmclaims.limit.<kind>.<amount>` | [Limits and costs](limits.md) |
| Costs and delays | `uxmclaims.cost.<kind>…`, `uxmclaims.delay.teleport.<n>` | [Limits and costs](limits.md) |

## Bypasses

Every ability node has a bypass twin — replace `ability` with `bypass`:

| Node | Effect |
|---|---|
| `uxmclaims.bypass.*` | Bypass every ability check |
| `uxmclaims.bypass.<category>.*` | Bypass a category, e.g. `uxmclaims.bypass.warp.*` |
| `uxmclaims.bypass.<action>` | Bypass one, e.g. `uxmclaims.bypass.claim.delete` |
| `uxmclaims.bypass.teleport` | Skip the teleport warmup |

Bypasses are checked **before** abilities, so a bypass grants the action even where the ability node
is explicitly denied.

## Claim block styles

Each entry in the `blocks` section of `config.yml` may carry its own permission. The shipped
`block2`, obsidian, is gated behind:

| Node | Grants |
|---|---|
| `claim.blockchange.obsidian` | Use the obsidian claim block style |

That node is a config value, not a plugin constant — rename it to whatever fits your permission
tree. A style with an empty `permission` is available to everyone.

## A worked example

Three ranks on a typical server:

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
  - uxmclaims.bypass.teleport
```

Note what `staff` does **not** get: `uxmclaims.admin`. Bypasses let staff work anywhere without also
passing every in-claim role permission silently, which keeps `/claim admin bulk …` out of their reach.

<Callout type="warning" title="uxmclaims.admin is not a moderation rank">

It bypasses everything, including the confirmation-free `bulk` commands that rewrite every claim on
the server. Give it to owners. Give moderators `uxmclaims.bypass.*` instead.

</Callout>

---
title: Reference
order: 601
description: Every permission node of uxmSimpleClaims with its default and what it allows.
icon: list-checks
---

A player node is held by everybody, so its command stays visible and a player without it is refused with
`command.no-permission`. A staff node hides its command.

## Players

| Node | Default | Allows |
|---|---|---|
| `uxmsimpleclaims.use` | true | Claiming land and managing your claims: every `/claim` branch but `auto` and `lang` |
| `uxmsimpleclaims.auto` | true | `/claim auto` |
| `uxmsimpleclaims.language` | true | `/claim lang` |
| `uxmsimpleclaims.flag.<flag>` | true | Changing that flag in your claims |
| `uxmsimpleclaims.flag.*` | true | Every flag; its children are the thirteen flag nodes |

## Limits

| Node | Default | Gives |
|---|---|---|
| `uxmsimpleclaims.limit.vip` | false | The `vip` group of `limits` |
| `uxmsimpleclaims.limit.<group>` | undeclared, op | Any other group you write |
| `uxmsimpleclaims.limit.unlimited` | false | No limit at all |
| `uxmsimpleclaims.inactivity.exempt` | op | Your claims are never taken for inactivity |

## Staff

| Node | Default | Allows |
|---|---|---|
| `uxmsimpleclaims.admin.manage` | op | Managing every claim as if you owned it |
| `uxmsimpleclaims.admin.bypass` | op | `/claimadmin bypass` |
| `uxmsimpleclaims.admin.inspect` | op | `stick`, `inspect` |
| `uxmsimpleclaims.admin.limits` | op | `limits` |
| `uxmsimpleclaims.admin.delete` | op | `delete` |
| `uxmsimpleclaims.admin.transfer` | op | `transfer` |
| `uxmsimpleclaims.admin.flags` | op | `resetflags` |
| `uxmsimpleclaims.admin.snapshots` | op | `snapshots`, `restore` |
| `uxmsimpleclaims.admin.profile` | op | `profile` |
| `uxmsimpleclaims.admin.audit` | op | `audit` |
| `uxmsimpleclaims.admin.reload` | op | `reload` |
| `uxmsimpleclaims.admin.doctor` | op | `doctor` |
| `uxmsimpleclaims.admin.version` | op | `version` and the bare command |
| `uxmsimpleclaims.admin.language` | op | `/claim lang set-player`, `lang reset-player` |
| `uxmsimpleclaims.admin.*` | op | Every node above |

Related: [Commands](../commands/overview.md)

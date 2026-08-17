---
title: Welcome
order: 101
description: What uxmClaims protects, and what a claim actually is.
icon: door-open
---

uxmClaims is chunk-based land protection for Paper and Spigot servers. A player stands on a chunk, runs
`/claim`, and that 16×16 column — bedrock to build limit — becomes theirs. Everything after that is
about who else may do what inside it.

Requires **Java 21** and **Minecraft 1.19.4 or newer**. Folia is supported.

## What a claim is

| Piece | What it means |
|---|---|
| **Chunks** | The land. A claim owns one *main* chunk and any number of connected extras. |
| **Owner** | One player. Always has every permission, and is the only one who may transfer or delete. |
| **Members** | Players invited in. What they may do comes from their role, with per-member overrides. |
| **Roles** | Named permission sets — `Owner`, `Member`, `Default`, plus any you create. |
| **Flags** | Rules that apply to *everyone*, owner included. PvP, TNT, fire spread, mob spawning. |
| **Bans** | Players who lose every permission in the claim, whatever their role says. |

Roles and flags answer different questions. A role answers *"may this player break blocks here?"*.
A flag answers *"may TNT break blocks here at all?"*. Both must say yes.

## What comes in the box

- A GUI for every operation — 28 configurable menus, no command required
- Custom roles with priorities, and per-member allow/deny overrides on top
- 32 flags covering PvP, explosions, fire, redstone, mob spawning and world mechanics
- 48 role permissions covering building, containers, entities, movement and management
- Claim warps, public or private, with an optional teleport cost and warmup
- A per-claim shared vault
- Claim chat, so members can talk without a second plugin
- Visual borders — display entities or particles — and a hologram over the claim block
- Claims drawn on Dynmap, BlueMap, Pl3xMap or squaremap
- Discord webhooks on claim events
- Expiry: claims can be given a lifetime and removed when it runs out
- MySQL, PostgreSQL or SQLite

<Callout type="info" title="GUI first, commands optional">

Every operation in this documentation has a menu. The command tables exist because power users and
console want them, not because the plugin needs them. A server can ship uxmClaims and tell players
exactly one thing: type `/claim`.

</Callout>

## Where to go next

| You want | Page |
|---|---|
| Get it running | [Installation](installation.md) |
| Understand chunks, roles and flags | [Concepts](concepts.md) |
| Walk a player through their first claim | [Your first claim](first-claim.md) |
| The full command tree | [Commands](../commands/) |
| Decide what members may do | [Role permissions](../protection/permissions.md) |
| Decide what the world may do | [Flags](../protection/flags.md) |

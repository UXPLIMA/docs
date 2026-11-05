---
title: Visitors
order: 407
description: Visits, the visit fee, warps, trust, bans, locking and island settings.
icon: door-open
---

## Visits

`/is visit <player>` goes to another island's spawn, unless the island is locked, the visitor is banned, or the
island is quarantined. A visitor holds the Visitor role: they look but do not build or open anything, unless trusted.
Which blocks count as doors, switches, containers and workstations is in
[interactables.conf](../config/interactables-conf.md). The commands a visitor may not use on another island
(`sethome` and `setwarp` as shipped) are in [visitors.conf](../config/visitors-conf.md).

## The visit fee

An island may charge visitors, paid into its bank: `/is visitfee <amount>`. A visitor sees the fee and types
`/is visit <player> confirm` to pay and go. The most an island may charge is `visitors.fee.most`, 10000 as shipped.

## Warps

An island opens warps where visitors can go, as many as its Warp Slots upgrade allows, each with a category. The
[warp directory](../menus/travel.md#warp-directory) lists every public warp. A locked warp is closed to visitors. See
[warps.conf](../config/warps-conf.md).

## Trust, bans and locking

| Tool | What it does |
|---|---|
| `/is trust <player> [duration]` | Lets a visitor build and use things for a while, an hour unless you say, at most a day |
| `/is ban <player> [reason]` | Sends a player away and keeps them out |
| `/is lock` | Lets in only members and allies |

## Island settings

The [settings window](../menus/island.md#settings) and `/is flag` switch fighting, visitors and monster spawns, and
`/is flag list` shows every flag: fire spread, leaf decay, water and lava flow, crop trampling, animal and phantom
spawns, explosion damage, and what visitors may pick up, drop and fight.

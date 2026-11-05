---
title: Concepts
order: 103
description: Islands, the grid, presets and game modes, roles, the bank, modules and the network.
icon: lightbulb
---

## Islands and the grid

An island is a square of the island world that belongs to one owner and their team. Islands are laid out on a square
spiral from 0,0, `grid.spacing` blocks apart (5120 as shipped), so on Folia every island is in regions of its own.
The island's size is its radius, which the [island size upgrade](../features/upgrades.md) raises. A player belongs to
one island at a time.

## Presets and game modes

A player starts an island from a preset: a schematic, an icon, a starting kit and optionally a game mode. Plain presets
(Classic, Desert, Nether, Cave) are classic skyblock. The others start one of the eleven
[game modes](../game-modes/overview.md), each with rules of its own. Presets are in
[presets.conf](../config/presets-conf.md); as shipped only the classic ones are offered.

## Roles and permissions

Every island has four roles: Owner, Co-Owner, Moderator and Member, plus Visitor for everyone else. An island can add
roles of its own. Each role carries a set of the 35 island permissions, such as breaking blocks, opening chests or
taking money from the bank. A member can act only on members and roles below their own. Server permission nodes
decide which commands a player may type; island roles decide what they may do on an island. See
[Roles](../features/roles.md).

## The bank

The bank is money the team shares. Upgrades, the shop, the visit fee and upkeep spend it. Beside money it can keep
experience, items and other economy plugins' currencies. See [Bank](../features/bank.md).

## Level, worth and leaderboards

Every block on an island has a worth and points; points make the island's level. Three leaderboards rank islands by
level, worth and bank. See [Level and leaderboards](../features/levels.md).

## Modules

Every part of the plugin is a module that `modules.conf` turns on or off. A module that is off uses nothing: no
listener, no timer, no cache. Each module has its own file under `modules/`.

## One server or a network

On one server nothing needs setting up. Several servers can share islands through one MySQL, MariaDB or PostgreSQL
database and Redis: each server holds the islands it runs under a lease and hands visitors to the server that holds
an island. See [Network](../features/network.md).

---
title: Protection
order: 409
description: Block and creature limits, the generator, stacking, abuse rules and freezing.
icon: shield
---

## Who may do what

On an island, what a player may break, place and use is decided by their [role](roles.md). A visitor holds no
permission unless trusted. `uxmskyblock.admin.bypass` passes every island rule.

## Limits

An island holds at most so many of the blocks and creatures that cost the server the most: hoppers, pistons, redstone
blocks, brewing stands, spawners, villagers and other entities. The limits rise with their upgrades. A player with
`uxmskyblock.bypass.limits` places past them. See [limits.conf](../config/limits-conf.md).

## The generator

A cobblestone generator on an island gives up ore at rates set per tier of the Ore Generator upgrade, in
[generators.conf](../config/generators-conf.md).

## Stacking

Some blocks stack: a player places one onto another of its kind and it counts as more. See
[stacking.conf](../config/stacking-conf.md).

## Abuse rules

[anti_abuse.conf](../config/anti_abuse-conf.md) keeps players from farming starter items: a wait between resets, at
most 3 resets a day, a 24 hour wait before joining another island after leaving one, and rules for second accounts.
`uxmskyblock.bypass.resetlimits` and `uxmskyblock.bypass.coopcooldown` pass them.

## Losing nothing

[protection.conf](../config/protection-conf.md) keeps a player from losing what they cannot get back, such as
obsidian that lava and water made, which can be scooped back up with an empty bucket for a while.

## Freezing and quarantine

Staff freeze an island with `/is admin freeze <target> [reason]`: nobody builds and visitors are sent away.
`/is quarantine` tells a player why. `uxmskyblock.admin.inspect` enters a frozen island and
`uxmskyblock.bypass.quarantine` comes in anyway.

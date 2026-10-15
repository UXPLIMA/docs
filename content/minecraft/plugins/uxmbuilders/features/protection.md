---
title: Land protection
order: 206
description: Refusing to build where the player cannot, and stopping when the land changes hands.
icon: shield
---

A build is checked against land protection before money changes hands, while the preview is open, and
again every few seconds while it builds. A player can only build what they could have built by hand.

## Providers

| Id | Plugin | Priority |
|---|---|---|
| `worldguard` | WorldGuard | 10 |
| `griefprevention` | GriefPrevention | 20 |
| `towny` | Towny | 30 |
| `plotsquared` | PlotSquared | 40 |
| `bukkitEvent` | Any other plugin, through a fake block-place event | 900 |

Each provider is on only when its plugin is installed and its switch under `protection.providers` is
`true`. A protection plugin that enables after uxmBuilders is picked up when it loads.
`/builders admin protection` lists what is active.

## How a box is checked

| Mode | A build is allowed when |
|---|---|
| `STRICT` | Every active provider allows it |
| `PERMISSIVE` | One provider allows it |
| `NONE` | Always; protection is not asked |

Providers are asked about a grid of points covering the footprint: at least `edgeSampling`
subdivisions per edge, never more than 8 blocks apart, on the floor, the ceiling and the layers in
between. GriefPrevention is also asked about every claim whose rectangle touches the footprint, so a
claim smaller than the grid cannot slip between two points.

A provider that finds no claim or region anywhere in the box answers "not applicable" rather than
"allowed", so in `PERMISSIVE` mode wilderness on one plugin does not override a claim on another.

## The generic fallback

`bukkitEvent` covers Lands, Residence, RedProtect, GriefDefender, BentoBox, SuperiorSkyblock2 and
anything else: it fires a fake block-place event at the sample points and reads whether a plugin
cancelled it. It is **off** by default, because other plugins' listeners run too: a logging plugin
records the probe, an anti-cheat may react.

## When the land changes hands

With `protection.cancelIfLost: true`, every `recheckIntervalTicks` the site is checked again for its
owner. If the owner lost the right to build there, the job is cancelled and refunds the **unbuilt share**
of the price, with no cancel penalty. A plugin listening to the cancel event can change the refund but
cannot keep the build going.

The recheck needs the owner online, so the same switch also pauses a build while its owner is offline
and resumes it when they return.

## Settings

| Key | Default | Meaning |
|---|---|---|
| `protection.mode` | `STRICT` | `STRICT`, `PERMISSIVE` or `NONE` |
| `protection.recheckIntervalTicks` | `100` | How often a running build is checked again |
| `protection.edgeSampling` | `4` | Minimum subdivisions per edge |
| `protection.cancelIfLost` | `true` | Cancel on lost land; pause while the owner is offline |
| `protection.bypassPermission` | `uxmbuilders.bypass.protection` | Skips the checks |
| `protection.providers.<id>` | `true`, `bukkitEvent` `false` | Per-provider switch |

Your own plugin can add a provider through the [developer API](../developer.md).

Related: [Placement rules](placement-rules.md), [Restarts and refunds](recovery.md)

---
title: Protection
order: 405
description: Roads and unclaimed land, refusals, forbidden blocks, cross-boundary tricks, lag machines, crash items and the bypass.
icon: shield-check
---

`protection.conf`:

| Key | Default | Meaning |
|---|---|---|
| `build-on-roads` | false | Anybody may build on a road |
| `build-on-unclaimed` | false | Anybody may build on a free plot |
| `refusal-cooldown-ms` | 2000 | How long before the same line is said again |
| `refusals.road`, `unclaimed`, `not-trusted`, `banned`, `edit` | `[]` | Effect lists; empty is the plugin's own line in the reader's language |

## Forbidden blocks

Nobody may put these down, whoever owns the plot: command blocks of every kind and their minecart, structure blocks and
voids, jigsaws, spawners and trial spawners, barriers, light, bedrock, end portal frames and the dragon egg. Take one out
of `forbidden-blocks` and it is allowed.

## Redstone

```hocon
redstone {
  enabled            = true
  changes-per-second = 200
  pause-seconds      = 30
  paused             = []
}
```

Each plot's redstone changes (current on a wire, a piston moving) are counted; a plot past the rate is switched off for
`pause-seconds` and its owner is told once. Holders of `uxmplots.admin.lag` are told too, with the plot named.
`/uxmplots redstone` switches it back on early.

## Cross-boundary

All on: `pistons`, `sticky-pistons`, `dispensers`, `droppers`, `liquids`, `fire`, `explosions`, `doors`, `tree-growth`,
`falling-blocks`, `projectiles`, `entity-teleport`, `portals`. Each stops a way in from outside a plot that a protection
watching only hands would miss. A redstone world may want pistons across the line.

## Crash items

`config.conf`, every rule off by default:

| Rule | Takes |
|---|---|
| `crash-book` | A book past `book-characters` (12000) or `book-pages` (50) |
| `broken-firework` | A firework past `firework-explosions` (12) |
| `illegal-stack` | A stack above the item's maximum |
| `unbreakable` | A tool that never wears out |
| `suspicious-potion` | A potion past `potion-effects` (8) or `potion-level` (10) |
| `overpowered-attribute` | An attribute past `attribute` (100) |
| `custom-skull` | A head carrying a texture the server would fetch |

An item that breaks a rule is taken, not refused, and the console says what and from whom. `on-taken` is an effect list.
Only plot worlds are checked, and holders of `uxmplots.bypass` are not.

## The bypass

`uxmplots.bypass` builds, breaks and opens anywhere, whoever owns the plot, places forbidden blocks and is not held by
edits or crash rules.

Related: [Flags](flags.md), [protection.conf](../config/protection-conf.md)

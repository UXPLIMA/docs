---
title: Poses
order: 9170
description: Sit, lay, belly-flop, spin and crawl, with claim and WorldGuard gating.
icon: person-standing
---

Poses lets players relax in the world: sit on the ground or on a block, sit on another player, lay down,
belly-flop, spin and crawl. It is the built-in answer to needing a separate sitting plugin, and every pose obeys
whatever land protection you already run.

Module `poses` · enabled by default · `modules/poses/config.conf`

## Commands

{/* generated:commands */}
| Command | What it does | Permission |
|---|---|---|
| `/crawl` | Crawl through a one-block gap; run again to stand up. | `uxmessentials.crawl.use` |
| `/poses` | Allow or refuse other players sitting on you. | `uxmessentials.poses.toggle` |
| `/sit` | Sit down where you stand or on the block you're looking at. | `uxmessentials.sit.use` |
{/* /generated */}

## Permissions

{/* generated:permissions */}
| Node | Default | Grants |
|---|---|---|
| `uxmessentials.bellyflop.use` | everyone | /bellyflop: flop onto your front where you stand. |
| `uxmessentials.crawl.use` | everyone | /crawl: crawl through a one-block gap; run again to stand up. |
| `uxmessentials.lay.use` | everyone | /lay: lie down on your back where you stand. |
| `uxmessentials.module.poses` | op | Hot-reload / inspect the poses module (built-in GSit-parity sitting and posing). |
| `uxmessentials.playersit.use` | everyone | Right-click another player to sit on them (the stacking pose), when player-sit is enabled. |
| `uxmessentials.poses.gui` | everyone | /poses (or /poses gui): open your personal poses settings & status panel. |
| `uxmessentials.poses.toggle` | everyone | /poses toggle: allow or refuse other players sitting on you. |
| `uxmessentials.sit.use` | everyone | /sit and right-click-to-sit: sit down where you stand or on a sittable block. |
| `uxmessentials.spin.use` | everyone | /spin: sit and spin in place. |
{/* /generated */}

## Settings

{/* generated:settings */}
| Key | Default | What it does |
|---|---|---|
| `features.sit` | `true` |  |
| `features.player-sit` | `false` |  |
| `features.lay` | `true` |  |
| `features.bellyflop` | `true` |  |
| `features.spin` | `true` |  |
| `features.crawl` | `true` |  |
| `sit-on-blocks` | `true` | Whether right-clicking a sittable block seats the player there. With this off, /sit only sits the player in place. |
| `sittable-materials` | `[ "*_STAIRS", "*_SLAB", "*_CARPET" ]` | The block materials a player may sit on when sit-on-blocks is true. A leading or trailing * is a wildcard, so "*_STAIRS" matches every stair type. Add or remove entries to suit your build palette. |
| `max-distance` | `2.0` | The furthest a player may be, in blocks, from the seat block they right-click. Keeps a player from sitting on a block across the room. Must be greater than zero. |
| `return-to-start` | `true` | Whether ending a pose returns the player to exactly where they stood when it began, rather than leaving them where the seat sat. Useful so sitting on a block edge does not nudge the player on stand-up. |
| `snore` | `true` | Whether the lying poses emit a soft snore (a subtitle and sound, no resource pack) so a sleeping pose reads as sleeping to nearby players. Set false for a silent lie-down. |
| `respect-claims` | `true` | Region gating. When a claim plugin is present, respect-claims requires the player to have build/interact permission where they pose; when WorldGuard is present, respect-worldguard honours the pose region flags. With no such plugin installed, posing is allowed everywhere regardless of these switches. |
| `respect-worldguard` | `true` |  |
{/* /generated */}

## Placeholders

{/* generated:placeholders */}
| Placeholder | Renders |
|---|---|
| `%uxmessentials_poses_pose%` | The pose the player holds: sit, lay, bellyflop, spin or none. |
| `%uxmessentials_poses_posing%` | Whether the player holds a free pose: lay, bellyflop or spin (yes/no). |
| `%uxmessentials_poses_sitting%` | Whether the player is sitting (yes/no). |
| `%uxmessentials_poses_toggle%` | Whether the player lets others sit on them: allow or refuse. |
{/* /generated */}

## Notes

- **Claims are honoured when a claim plugin is installed.** With `respect-claims` on, a player poses only where
  they are trusted to act.
- **WorldGuard gets four flags:** `sit`, `playersit`, `pose` and `crawl`, each defaulting to allow. Set one to
  deny in a region to block that pose there, or turn the whole check off with `respect-worldguard`. With no
  region plugin installed everything is simply allowed.
- **Seats leave nothing behind.** The helper entity is invisible, never saved, and removed the moment a player
  stands, logs out or the chunk unloads; anything a crash leaves is swept on the next start.
- **Sitting on another player needs both sides to allow it,** which is what keeps it a joke between friends
  rather than a way to pin somebody.

Related: [Player State](playerstate.md), [Regions](regions.md), [Survival](survival.md)

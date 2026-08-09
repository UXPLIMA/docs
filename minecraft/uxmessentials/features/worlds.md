---
title: Worlds
order: 1390
description: 'The worlds module is a full multi-world manager: create, import, load,
  unload and delete worlds, tune their properties and gamerules, set per-world spawns,
  and move players between them — all in-game, no bukkit.yml edits and no restart.
  It''s built for a server that runs more than the vanilla overworld/nether/end: a
  creative plot world, a resource world you reset on a cycle, a minigame lobby, an
  event map.'
---

<Callout type="note" title="`/worlds` (plural) vs `/world` (singular)">

Management lives under **`/worlds`** (plural), with nodes under
`uxmessentials.world.*`. The singular **`/world`** is a small player-state
*info* command that just tells you which world you're standing in — a different
module entirely.

</Callout>

---

## Creating & Importing Worlds

`create <name> [env] [type] [gen]` builds a brand-new world. Beyond the vanilla
environments and world types, two built-in **generators** cover the common special
cases:

| Generator | Result |
|-----------|--------|
| `void` | An empty world — nothing but the spawn platform you build |
| `flat` | A superflat world |

Already have a world folder on disk (a downloaded map, or one from another plugin)?
`import <folder> <env> [gen…]` registers it in place without regenerating a single
chunk. New-world defaults (environment, world type, structure generation) live in
`worlds.conf` under `defaults {}`, and the two generators under `generators { void, flat }`.

<Callout type="tip" title="Coming from Multiverse-Core? Import the whole registry at once">

`/uxmess import multiverse` reads `plugins/Multiverse-Core/worlds.yml` and registers
every world it lists, with its alias, auto-load, environment, seed, generator, spawn and
settings, instead of you re-importing them one folder at a time. Nothing is regenerated.
See [Migrating from another plugin](../getting-started/migration.md#coming-from-multiverse-core)
for what does and does not come across.

</Callout>

---

## Loading, Unloading & Deleting

Worlds have a clear lifecycle, and each step is its own command:

| Command | Effect |
|---------|--------|
| `load <name>` | Load a registered world into memory |
| `unload <name>` | Unload it (players are moved out first) |
| `unregister <name>` | Forget the world without touching its files |
| `delete <name>` → `confirm <name>` | Permanently remove the world and its folder |

Deletion is deliberately **two-step**: `delete` stages the removal and
`confirm` commits it, within `delete-confirm-timeout`. The main world is guarded by
`protect-default-world` so it can't be deleted by accident.

<Callout type="info" title="Adopting worlds on startup">

`auto-adopt-loaded` picks up worlds other plugins loaded, and
`auto-load-registered` re-loads the worlds you've registered when the server
starts — so your world roster comes back exactly as you left it.

</Callout>

---

## Properties & Gamerules

Two commands cover per-world tuning:

- `set <name> <property> <value…>` — world properties (difficulty, PvP, spawn
  behaviour, keep-spawn-loaded, and so on).
- `gamerule <name> <rule> <value>` — any vanilla gamerule (`doDaylightCycle`,
  `keepInventory`, `mobGriefing`, …), applied to that world alone.

Set `keepInventory` on the arena world without touching survival, or turn off mob
spawning in the lobby — each world carries its own rules.

---

## Per-World Spawn

`setspawn <name>` pins a world's spawn to where you're standing; `spawn [name]`
teleports you (or, with a name, sends you to a specific world's spawn). This is the
anchor `tp` and portals land players on.

---

## Teleport, Access & Entry Fee

`tp <name> [player]` sends you — or another player — into a world. Access isn't
automatically open: the `access {}` block in `worlds.conf` can gate a world behind a
**permission** and charge an **entry fee** through the economy, so a premium world
or a paid event map is a config change, not a plugin.

---

## Portals

Worlds can be wired together with **portals** — frames that drop a player into a
target world (and spawn) when they step through. Combined with per-world access
gates and entry fees, portals turn your world roster into a navigable hub without
anyone typing a command.

---

## The World Editor GUI

`gui [name]` opens the world manager as a menu: the bare form lists every world with
its state, and picking one opens a per-world editor for properties, gamerules,
spawn, and teleport — the same actions as the commands, point-and-click. It runs on
the shared [menu engine](../menus/engine.md), so it's config-skinnable like every
other panel.

---

## Pregeneration

Pre-generating chunks ahead of time stops the lag spikes that hit when players
explore fresh terrain. `pregen <name> <radius>` walks the world outward to `radius`,
and `pregen cancel <name>` stops it. The job is paced by `worlds.conf` —
`pregen { max-radius, max-concurrent-chunks, tick-period-ticks }` — so it fills in
the background without dragging down TPS.

---

## Backup & Restore

| Command | Effect |
|---------|--------|
| `backup <name>` | Snapshot a world |
| `backups <name>` | List a world's snapshots |
| `restore <name> <backup>` → `restoreconfirm <name>` | Roll a world back (staged) |

Snapshots land in `backup { directory }` and are pruned to `backup { retention-count }`.
Like deletion, restore is two-step so you can't overwrite a live world by fat-finger.

---

## Auto-Unload

Idle worlds don't need to sit in memory. `auto-unload` (off by default) unloads a
world after `idle-minutes` with no players, sweeping every `sweep-interval-seconds`,
skipping anything in `excluded-worlds`. Keep the resource world loaded only when
someone's actually mining it.

---

## Placeholders

The current world's stats are exposed through **PlaceholderAPI**
(`%uxmessentials_*%` families) for use in scoreboards, tablists and holograms — so a
world name, player count, or spawn can appear on your HUD without extra plumbing.

---

## Next Steps

- [🗺️ Worlds Commands](../commands/worlds.md) — the full `/worlds` command reference
- [🎛️ Custom Menu Engine](../menus/engine.md) — how the world editor GUI is built
- [🧩 Per-Module Config](../config/per-module.md) — `worlds.conf` defaults, pregen, backups, auto-unload
- [🔑 Permission Reference](../permissions/reference.md) — the `uxmessentials.world.*` nodes

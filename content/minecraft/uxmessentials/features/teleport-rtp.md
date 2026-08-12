---
title: Teleport & RTP
order: 1230
description: 'The teleport module is the busiest part of uxmEssentials. It bundles
  everything that moves a player around: player-to-player teleport requests,
  return-to-last-spot, random wilderness teleports, spawns, and the direct staff
  teleports: all governed by one shared warmup and cooldown system so the rules feel
  consistent no matter which command a player uses.'
---

Everything here runs through the plugin's Folia-ready scheduler and does its I/O off the main thread, so even the safe-search behind `/rtp` never freezes the server.

---

## Teleport Requests (TPA)

The request flow is the polite way to teleport to another player: you ask, they accept. The pieces:

| Concept | Commands |
|---------|----------|
| **Ask** | `/tpa <player>` (go to them), `/tpahere <player>` (bring them to you) |
| **Answer** | `/tpaccept`, `/tpdeny` |
| **Manage yours** | `/tpcancel`, `/tpalist` |
| **Set a policy** | `/tptoggle`, `/tpon`, `/tpoff`, `/tpauto` |
| **Block someone** | `/tpblock <player>`, `/tpunblock <player>` |
| **Ask everyone** | `/tpaall` |

Requests expire after `request-ttl-seconds`. `/tptoggle` refuses all incoming requests; `/tpauto` auto-accepts them; and `/tpblock` shuts out one specific player, which is the tool a player reaches for against a stalker.

---

## Back & Death-Back

`/back` (alias `return`) returns you to your last captured location, and `/deathback` (alias `dback`) returns you to where you last died.

<Callout type="info" title="Returning to a death location is a grant">

By default `/back` and `/deathback` only take a player back to their last **teleport** location. Returning to a **death** location requires the `uxmessentials.back.ondeath` marker node. This lets you decide, per rank, whether dying is something players can undo.

</Callout>

The `back` config block controls the death behaviour: `on-death` (whether deaths are captured at all), `death-delay-seconds` (a grace window), and `ignored-causes` (death causes that don't seed a back point: void, for instance).

---

## Random Teleport (RTP)

`/rtp` (alias `wild`) drops a player at a random safe spot in the wilderness. The important detail is **how** it stays fast: instead of searching for a safe location the moment a player runs the command (which can be slow and blocks nothing usefully), uxmEssentials keeps a **pre-warmed queue** of validated safe locations. `/rtp` hands out a location from the queue instantly, and the queue tops itself back up in the background.

<Callout type="success" title="Fully asynchronous: no TPS drops, no chunk leaks">

The background safe-search loads and inspects candidate chunks **off the main
thread** (via asynchronous chunk loading), so scanning the wilderness never
freezes the server the way a naive RTP does. Just as importantly, any chunk the
search loads to check is **released again afterward**: RTP won't quietly bloat
your memory or region files with far-off, one-off chunks.

</Callout>

<Callout type="tip" title="Survives restarts: no cold-start lag spike">

Validated locations are also **saved to the database**, so after a restart the
queue re-warms itself from disk (re-checking each spot is still safe) instead of
doing a cold search storm on the first `/rtp`. The persisted pool is bounded per
world and prunes stale entries automatically.

</Callout>

RTP has a few more entry points:

| Command | What it does |
|---------|--------------|
| `/rtp` | Opens the world-picker GUI by default (set `command-opens-gui = false` to random-teleport in your current world instead). |
| `/rtp <world>` | Random teleport in a specific world. |
| `/rtp <player>` | Staff: send another player on a random teleport. |
| `/rtp biome <biome>` | Random teleport into a chosen biome. |
| `/rtp gui` | Open the world picker (always, regardless of the toggle). |

Cooldown and reach can be tuned per rank with the numbered permissions
`uxmessentials.rtp.cooldown.<seconds>` and `uxmessentials.rtp.radius.<n>`.

<Callout type="tip" title="Land in a specific biome">

**`/rtp biome <biome>`** drops a player in the biome they ask for. Common biomes
come straight from the pool; for rare ones (mushroom fields, cherry groves…),
uxmEssentials keeps a lightweight, passive map of where each biome has been seen
as the world is explored, and steers the search toward those spots, so finding
a rare biome converges instead of hammering the server with random guesses.

</Callout>

<Callout type="tip" title="Safe landings &amp; fair charging">

On arrival players get a short **grace window**: Resistance and Slow-Falling
plus no fall damage, so nobody dies to the ground before the chunks render.
And any RTP **cost or cooldown is only applied once the teleport succeeds**: a
player is never charged, and their cooldown never starts, for a search that
failed. RTP can also fire automatically **on respawn** and **on a player's first
join** (both configurable), served straight from the pre-warmed pool.

</Callout>

The search zone and queue behaviour live in `modules/teleport/rtp.conf`:

| Key | What it does |
|-----|--------------|
| `command-opens-gui` | Whether a bare `/rtp` opens the world-picker GUI (default) or random-teleports in place. |
| `min-radius` / `max-radius` | The ring, in blocks from origin, that RTP searches within. |
| `respect-claims` | Never land inside a land claim (GriefPrevention, PlotSquared, uxmClaims, …). |
| `respect-worldguard` | Never land inside a WorldGuard region. |
| `queue-target-size` | How many safe locations to keep pre-warmed. |
| `queue-low-water` | Refill the queue when it drops below this. |
| `max-attempts` | How many candidate points a single search may try before giving up. |
| `max-chunk-loads` | Cap on chunks a single search may load: a hard ceiling on its cost. |
| `max-wall-clock-ms` | Time budget for a single search; whichever cap is hit first ends it. |
| `persist-pool` | Save validated locations to the database so the queue survives restarts. |
| `pool-max-per-world` | Cap on how many validated locations are stored per world. |
| `pool-stale-after-hours` | Prune stored locations older than this. |
| `grace-seconds` | Length of the safe-landing grace (Resistance + Slow-Falling + no fall damage). |
| `rtp-on-respawn` | Worlds where a player respawns to a random spot. |
| `rtp-on-first-join` | Random-teleport a player the first time they join. |
| `biome-targeting` | Enable `/rtp biome <biome>` and the rare-biome map. |
| `biome-hotspot-weight` | How strongly the search steers toward known spots for a rare biome (0–1). |
| `excluded-biomes` | Biomes RTP will never land in (oceans, for example). |
| `avoid-blocks` | Block types considered unsafe to land on. |

Staff can retune the ring live without editing the file:

```
/settpr <minRange> <maxRange>
```

`/settpr` needs `uxmessentials.teleport.settpr`.

---

## Spawns

The module manages a **main spawn** plus any number of **named spawns**, and can **mirror** another world's spawn:

| Command | Description | Permission |
|---------|-------------|------------|
| `/spawn [name]` | Go to the server spawn, or a named one | `uxmessentials.spawn.use` / `.spawn.named` |
| `/setspawn [name]` | Set the (named) spawn | `uxmessentials.spawn.set` |
| `/setmainspawn` | Set the primary server spawn | `uxmessentials.spawn.set` |
| `/removespawn` | Delete a spawn | `uxmessentials.spawn.set` |
| `/mirrorspawn <world>` | Mirror another world's spawn | `uxmessentials.spawn.set` |

Named spawns let you run destinations like `/spawn pvp` or `/spawn events`; `/mirrorspawn` is how you make a new world hand players off to an existing spawn instead of a fresh one.

---

## Staff Teleports

These pull players directly, without the request handshake, and default to `op`:

- `/tp`, `/tphere`, `/goto`, `/bring`: direct player teleports (`uxmessentials.tp.use`).
- `/tp <x> <y> <z> [world]`, `/tppos …`: coordinate teleports (`uxmessentials.tp.position`).
- `/tpo`, `/tpohere`: override no-tp flags (`uxmessentials.tp.others`).
- `/tpall`: pull everyone to you (`uxmessentials.tp.all`).
- `/tpoffline`, `/tpofflinehere`: to/from a player's logout location (`uxmessentials.tp.offline`).
- `/top`, `/bottom`, `/jump`, `/up`, `/down`, `/ascend`, `/descend`, `/thru`: vertical/directional jumps (`uxmessentials.tp.vertical`).

---

## Warmups, Cooldowns & the Move Rule

Every teleport can carry a **warmup** (a countdown before it fires) and a **cooldown** (a wait before the next one). Both are granted through numbered permission tiers, so ranks can be tuned independently, and the **highest matching value wins**:

| Node | Purpose |
|------|---------|
| `uxmessentials.tp.warmup.<seconds>` | Warmup countdown before a teleport fires |
| `uxmessentials.tp.cooldown.<seconds>` | Cooldown before the next teleport |
| `uxmessentials.tp.warmup.bypass` | Skip warmups entirely |
| `uxmessentials.tp.cooldown.bypass` | Skip cooldowns entirely |

<Callout type="warning" title="Move cancels warmup">

A teleport with a warmup is **cancelled if you move** before the countdown finishes. Whether rotating, taking damage, or interacting also cancels it is controlled by the `warmup` config block (`cancel-on-move`, `cancel-on-rotate`, `cancel-on-damage`, `cancel-on-interact`, and a `move-threshold`). This anti-combat-log invariant is owned by the teleport module and applies to every teleport that has a warmup. Holders of `uxmessentials.tp.warmup.bypass` skip it.

</Callout>

The defaults live in `modules/teleport/config.conf`: `default-warmup` (ships `3`), `default-cooldown` (ships `5`), `cooldown-start-phase` (whether the cooldown clock starts when the command is run or when the teleport lands), `teleport-to-center` (drop players at block centre), plus `effects`, `arrival-messages`, `arrival-title`, and `arrival-effects` for arrival polish.

---

## No Teleporting Out of a Fight

If you run **CombatLogX** or **PvPManager**, a player they have combat-tagged cannot self-teleport away:

```hocon
combat {
  block-teleport = true
}
```

`/home`, `/warp`, `/spawn`, `/back`, `/rtp` and opening a `/tpa` request are all refused while the tag is up, with `You cannot teleport while in combat.` Staff teleports are not affected, and neither is anything that moves a player without them asking for it.

We keep **no combat timer of our own**, on purpose: a combat timer is a whole feature with its own edge cases, and servers that care about it already run a plugin for it. What is worth owning is the consequence, because a player who can `/home` out of a losing fight makes the other plugin's timer pointless, and that hole is ours rather than theirs.

So with neither plugin installed this setting does nothing at all, and nothing is ever blocked. With both installed a player is tagged when either one says so, and a combat plugin that misbehaves lets the teleport through rather than trapping anybody.

---

## Key Settings (module config)

Two files:

- **`modules/teleport/config.conf`**: `default-warmup`, `default-cooldown`, `cooldown-start-phase`, `request-ttl-seconds`, `teleport-to-center`, and the `effects`, `arrival-messages`, `cooldowns`, `warmup { … }`, `combat { … }`, and `back { … }` blocks.
- **`modules/teleport/rtp.conf`**: the RTP ring, queue, and safe-search tuning described above.

---

## Tips & Gotchas

- **RTP feels instant because it's pre-warmed.** If players report `/rtp` "failing", check the search budget (`max-attempts` / `max-wall-clock-ms`) and `excluded-biomes`: an over-restrictive search can starve the queue. Every search is bounded and spread across ticks, so raising these tunes success rate without ever stalling the server.
- **`/back` after death does nothing without `back.ondeath`.** If players expect to return to their corpse, grant `uxmessentials.back.ondeath` and set `back.on-death = true`.
- **Warmups are a feature, not a bug.** Setting `default-warmup = 0` (or granting `tp.warmup.bypass` to everyone) removes the move-to-cancel protection that stops players escaping combat.
- **`cooldown-start-phase` changes the feel.** Starting the cooldown on *arrival* rather than on *command* effectively adds the warmup to the wait.
- **RTP avoids protected land.** With `respect-claims` / `respect-worldguard` on (the default), a candidate spot inside anyone's land claim or a WorldGuard region is rejected, so players only land in true wilderness. Both work with no extra setup: install the claim plugin or WorldGuard and RTP just honours it.
- **The "no lag" guarantee is built in.** RTP's wilderness scan is designed so it can never load a chunk on the server thread (and the build itself refuses to compile if that ever changes) so `/rtp` stays fast no matter how the plugin evolves.

---

## Next Steps

- [🌀 Teleport & RTP Commands](../commands/teleport.md) - The full command reference
- [📍 Warps](warps.md) - Fixed staff destinations
- [🏠 Homes](../modules/homes.md) - Personal teleport points
- [🔐 Permission Reference](../permissions/reference.md) - Warmup/cooldown tiers and bypass nodes
- [⚙️ Per-Module Config](../config/per-module.md) - Every `config.conf` and `rtp.conf` key

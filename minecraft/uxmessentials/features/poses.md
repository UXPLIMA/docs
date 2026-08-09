---
title: Sitting & Poses
order: 1450
---

## What Is Sitting & Poses?

The **poses** module lets players relax in the world — sit down on the ground or on
furniture-like blocks, with more poses on the way. It is the built-in answer to
"do I need a separate sit plugin?" — you don't.

Like every part of uxmEssentials, it is a module you can turn off entirely
(`modules/poses/config.conf` → `enabled = false`) if your server doesn't want it.

<Callout type="tip" title="Everything's here">

The full pose set is available: **sit**, **sit on other players**, **lay**,
**belly-flop**, **spin**, and **crawl** — one module, no extra plugins.

</Callout>

---

## Sitting

There are two ways to sit:

| How | What happens |
|-----|--------------|
| **`/sit`** | Sit down right where you're standing. If you're looking at a nearby seat block, you sit on that instead. |
| **Right-click a seat block** | Right-click the top of a stair, slab, or carpet (or any block on your custom list) to sit there. |

You stand back up by **sneaking**, taking damage, teleporting, or logging out.

<Callout type="tip" title="You return to where you sat">

With `return-to-start` enabled (the default), standing up puts you back on the
exact spot you were before you sat — no drifting off the seat.

</Callout>

---

## Sitting on Other Players

Players can pile up: **right-click another player to sit on their shoulders**, and
they can right-click someone too — the stack goes as high as you like.

This is **off by default** because it's a novelty, not every server wants it. Turn
it on with `features.player-sit = true` in the config.

Anyone can opt out of being sat on with **`/poses toggle`** — one command flips
between allowing and refusing riders. Their choice is remembered across logouts.
Prefer a menu? **`/poses`** opens a small settings panel showing your current pose
and the same allow/refuse switch.

| Command | What it does |
|---------|--------------|
| Right-click a player | Sit on them (if the feature is on and they allow it) |
| `/poses toggle` | Allow / refuse other players sitting on you |

<Callout type="tip" title="Nobody gets stuck">

If the player you're sitting on logs out, you're gently dropped and set back on
your feet — no floating, no stuck passengers.

</Callout>

---

## Lay, Belly-flop & Spin

Sitting isn't the only way to relax:

| Command | What it does |
|---------|--------------|
| `/lay` | Lie down on the ground |
| `/bellyflop` | Flop face-down on the ground |
| `/spin` | Sit and slowly spin in place |

You leave any pose the same way you leave a seat — sneak, take damage, teleport,
or log out.

<Callout type="tip" title="Snoring, no resource pack needed">

Players who `/lay` down softly **snore** — a gentle sound plays every few
seconds, with no resource pack required. Turn it off with `snore = false` in
the config.

</Callout>

Each pose can be turned off individually in the config (`features.lay`,
`features.bellyflop`, `features.spin`).

---

## Crawling

**`/crawl`** drops you into a crawl so you can move through 1-block-high gaps —
under slabs, into 1×1 tunnels, wherever a standing player wouldn't fit. Run
`/crawl` again to stand back up. It also ends when you teleport, die, or log out.

Unlike the other poses, you can **keep walking** while you crawl.

<Callout type="note" title="How it works — and its one limit">

Crawling shows *you* an invisible block above your head that keeps you low; it
is client-side only, so it never places a real block, never suffocates you, and
is cleared the instant you stand, teleport, or leave. At a full sprint the
effect can trail you by a single tick — harmless, and it catches up immediately.

</Callout>

Turn it off with `features.crawl = false`.

---

## Which blocks can I sit on?

Out of the box, players can right-click **stairs, slabs, and carpets**. You decide
the full list in the config with `sittable-materials`, and you can turn
right-click seating off entirely while keeping `/sit`.

The plugin works out the right sitting height and facing from the block — a player
on a stair faces the way the stair faces, and sits at the correct height on slabs
and carpets.

---

## Claims & Regions

Poses respect your land-protection setup, so players can't sit or crawl where they
aren't welcome:

- **Land claims** — if you run a claim plugin (GriefPrevention, PlotSquared,
  uxmClaims and others), a player can only pose where they're trusted to act.
  Toggle with `respect-claims`.
- **WorldGuard flags** — when WorldGuard is installed, uxmEssentials registers four
  custom region flags — **`sit`**, **`playersit`**, **`pose`**, **`crawl`** — each
  defaulting to *allow*. Set a flag to `deny` in a region to block that pose there.
  Toggle the whole check with `respect-worldguard`.

If you don't run any region plugin, everything is simply allowed — no setup needed.

<Callout type="note" title="WorldGuard flag registration">

The flags are registered while the server loads, so WorldGuard must be present
at startup for them to appear. A player denied by a claim or flag just gets a
short "you can't do that here" message and stays standing.

</Callout>

---

## Configuration

Everything lives in `plugins/uxmEssentials/modules/poses/config.conf` (HOCON):

```hocon
poses {
  enabled = true

  features {
    sit = true
    player-sit = false   # right-click a player to sit on them (off by default)
    lay = true
    bellyflop = true
    spin = true
    crawl = true
  }

  # Play a soft snore sound while a player is laying down (no resource pack)
  snore = true

  # Right-click a block to sit on it
  sit-on-blocks = true
  sittable-materials = [
    "*_STAIRS"
    "*_SLAB"
    "*_CARPET"
  ]

  # How far away a seat block may be when you right-click it
  max-distance = 2.0

  # Stand back up on the exact spot you started from
  return-to-start = true

  # Respect land claims and WorldGuard region flags
  respect-claims = true
  respect-worldguard = true
}
```

After editing, apply it with `/uxmess reload poses`.

<Callout type="warning" title="HOCON, not YAML">

uxmEssentials config is HOCON (`.conf`). Keys are kebab-case, comments start
with `#`, and lists use square brackets. See
[Configuration](../config/overview.md) for the full picture.

</Callout>

---

## Permissions

| Node | What it grants | Default |
|------|----------------|---------|
| `uxmessentials.sit.use` | Use `/sit` and right-click seats | ✅ everyone |
| `uxmessentials.playersit.use` | Sit on another player | ✅ everyone |
| `uxmessentials.lay.use` | Use `/lay` | ✅ everyone |
| `uxmessentials.bellyflop.use` | Use `/bellyflop` | ✅ everyone |
| `uxmessentials.spin.use` | Use `/spin` | ✅ everyone |
| `uxmessentials.crawl.use` | Use `/crawl` | ✅ everyone |
| `uxmessentials.poses.toggle` | Use `/poses toggle` to allow/refuse riders | ✅ everyone |
| `uxmessentials.poses.gui` | Open the `/poses` settings panel | ✅ everyone |
| `uxmessentials.poses.cooldown.<seconds>` | Wait `<seconds>` between starting poses (anti-spam; grant the number of seconds) | Not set |
| `uxmessentials.poses.cooldown.bypass` | Skip the pose cooldown | Operators |
| `uxmessentials.module.poses` | Reload/inspect the poses module | Operators |

---

## Placeholders

With PlaceholderAPI installed:

| Placeholder | Returns |
|-------------|---------|
| `%uxmessentials_poses_sitting%` | `yes` while the player is sitting, `no` otherwise |
| `%uxmessentials_poses_posing%` | `yes` while the player is laying, belly-flopping, or spinning |
| `%uxmessentials_poses_pose%` | the current pose: `sit`, `lay`, `bellyflop`, `spin`, or `none` |
| `%uxmessentials_poses_toggle%` | `allow` or `refuse` — whether the player accepts riders |

---

## Good to know

- **No ghost seats.** Seats are invisible, non-saved helper entities that are
  cleaned up the moment you stand, log out, or the chunk unloads — and any stray
  left by a crash is swept away on the next server start. You will never find a
  leftover invisible stand in your world.
- **Folia-ready.** All seat handling runs through the region-aware scheduler, so
  it behaves on both Paper and Folia.

---

## Next Steps

- ⚙️ [Configuration](../config/overview.md) — how uxmEssentials config works
- 🧩 [The Module System](../modules/overview.md) — turn modules on and off
- 🔑 [Permission Reference](../permissions/reference.md) — every node

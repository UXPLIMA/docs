---
title: Homes
order: 1200
description: 'Homes are private teleport points that each player saves for themselves.
  In uxmEssentials they are deliberately GUI-first: instead of memorising /sethome,
  /delhome, and a pile of subcommands, a player types /home and manages everything
  from a slot grid. The single root command /home (aliases h, homes) is the whole
  surface — creating, deleting, renaming, re-iconing, and teleporting all happen inside
  the menu.'
---

Homes are **DB-backed**. A player's homes live in the database, not in a chunk or in player-data files, so they survive world rollbacks, `/reload`, and server restarts.

---

## The Home Grid

Opening `/home` shows a grid of **slots**, numbered starting at **1**. Each slot is either an existing home (click to teleport) or an empty slot (click to create a home at your current location). The grid is where a player does everything:

- **Teleport** — click a filled slot.
- **Create** — click an empty slot to save your current position there.
- **Rename / re-icon / delete** — open a slot's detail view.

Because the number of slots a player sees is driven by their **quota** (below), the grid naturally communicates "you can have this many homes" without a separate limits command.

<Callout type="tip" title="Custom icons">

Players who hold `uxmessentials.home.icon` can set a custom icon (a material) for each home, so the grid becomes a personal, visual list — a bed for the base, a diamond for the mine, and so on.

</Callout>

---

## Quotas: How Many Homes?

The number of homes a player may keep is a **quota**, resolved through numbered permission nodes:

```
uxmessentials.home.limit.<n>
```

The `<n>` is the number of homes granted. If a player matches several of these nodes, the outcome depends on the module's `limit-mode`:

| `limit-mode` | Behaviour |
|--------------|-----------|
| `highest` (default) | The largest `<n>` the player holds wins. Granting `.home.limit.3` and `.home.limit.10` gives **10**. |
| `stack` | All matched `<n>` values are **added together**. The same two grants give **13**. |

You can also make quotas **per-world** by appending a world name:

```
uxmessentials.home.limit.5.world_nether
```

A per-world node overrides the global limit for that world only, which is how you let players keep, say, three homes in the overworld but only one in the Nether.

<Callout type="info" title="Default limit">

Players who match no numbered node fall back to `default-limit` (ships as `3`). Set `unlimited-max` in the module config to cap how high "unlimited"-style grants are allowed to resolve.

</Callout>

---

## Sharing: Invites & Visits

Homes are private by default, but a player can grant access to specific others:

| Action | Command | Permission |
|--------|---------|------------|
| Grant access | `/home invite <player> [slot]` | `uxmessentials.home.invite` |
| Revoke access | `/home uninvite <player> [slot]` | `uxmessentials.home.invite` |
| Teleport to an invited/public home | `/home visit <player> [slot]` | `uxmessentials.home.visit` |

Omitting `[slot]` targets the player's default (first) home. This makes shared bases and co-op builds easy without exposing every home a player owns.

---

## Admin Management

Staff can manage anyone's homes through one admin verb, which mirrors the player-facing actions:

```
/home admin <player> list|info|set|del|tp|clear [slot]
```

This requires `uxmessentials.home.admin`. `list`/`info` inspect, `set`/`del`/`tp` operate on a slot, and `clear` wipes all of a player's homes. Useful for cleaning up after a ban or relocating a player off a deleted world.

---

## Key Settings (module config)

Homes are configured in `modules/homes/config.conf`. The notable knobs:

| Key | What it does |
|-----|--------------|
| `default-limit` | Home quota for players matching no numbered node (default `3`). |
| `limit-mode` | `highest` or `stack` — how numbered quota nodes combine. |
| `unlimited-max` | Ceiling applied when resolving very high quota grants. |
| `disabled-worlds` | Worlds where homes cannot be set. |
| `block-unsafe-sethome` | Refuse creating a home at an unsafe spot. |
| `confirm-delete` / `confirm-relocate` / `confirm-unsafe-teleport` | Require a confirmation click for destructive or risky actions. |
| `date-format` | How creation timestamps render in the GUI. |
| `economy { … }` | Optional costs for homes (see below). |
| `claims { … }` | Land-claim awareness (see below). |

### Optional costs

The `economy` block (off by default) lets each home operation charge the player:

```hocon
economy {
  enabled = false
  create-cost = 0
  relocate-cost = 0
  teleport-cost = 0
  currency = "coins"
}
```

Charges are drawn through the DB-backed [economy](economy.md). Players with `uxmessentials.home.bypass.cost` are never charged.

### Land-claim awareness

The `claims` block integrates with a land-claim provider (uxmClaims, Lands, GriefPrevention, and others). When enabled you can require homes to sit inside a claim, block homes set inside **foreign** claims, and check teleport access on arrival — so a player can't set a home inside someone else's protected base and keep visiting it.

---

## Permissions & Quotas at a Glance

| Node | Purpose | Default |
|------|---------|---------|
| `uxmessentials.home.use` | Open and manage own homes | `true` |
| `uxmessentials.home.visit` | Visit invited/public homes | — |
| `uxmessentials.home.invite` | Invite/uninvite others | — |
| `uxmessentials.home.admin` | Manage other players' homes | `op` |
| `uxmessentials.home.icon` | Set custom home icons | — |
| `uxmessentials.home.limit.<n>` (opt. `.<world>`) | Home quota tier | — |
| `uxmessentials.home.bypass.unsafe` | Teleport to unsafe homes | — |
| `uxmessentials.home.bypass.cost` | Skip home economy costs | — |

---

## Tips & Gotchas

- **Slots are 1-based.** `/home visit Steve 2` means Steve's second slot, not a zero-indexed one.
- **Quotas clamp everywhere.** Homes imported from EssentialsX go through the same service as `/home`, so a migrated player never ends up over their quota.
- **`highest` vs `stack` matters for LadderPerms setups.** If you grant home limits at multiple rank levels expecting them to add up, you must set `limit-mode = stack` — the default `highest` will only honour the biggest.
- **Per-world quotas need the world name exactly.** The suffix is the Bukkit world name (`world`, `world_nether`, `world_the_end`, or your custom world folder).
- **Unsafe-teleport confirmation** protects players from warping into a spot that has since become lava or a drop; holders of `uxmessentials.home.bypass.unsafe` skip both the check and the prompt.

---

## Next Steps

- [🏠 Homes & Warps Commands](../commands/homes-warps.md) - The full command reference
- [🌐 Player Warps](player-warps.md) - Player-owned public destinations
- [💰 Economy](economy.md) - The currency behind optional home costs
- [🔐 Permission Reference](../permissions/reference.md) - Quota tiers and bypass nodes
- [⚙️ Per-Module Config](../config/per-module.md) - Every `modules/homes/config.conf` key

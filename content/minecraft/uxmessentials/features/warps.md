---
title: Warps
order: 1210
---

Warps are **server-wide destinations** — a spawn hub, a PvP arena, a market, an event stage. Unlike [homes](homes.md) (private, per-player) or [player warps](player-warps.md) (owned by individual players), a warp is created by staff and, by default, usable by everyone. The single root command `/warp` (aliases `wp`, `warps`) covers both using and managing them; older standalone commands like `/setwarp`, `/delwarp`, and `/warpinfo` are folded in as subcommands.

Warps are **DB-backed**, so they survive world rollbacks and restarts, and each carries a small pile of metadata: an owner, a creation time, an optional cost, lock/password state, a rating, and a **visitor counter** that tracks how many times it has been used.

---

## Creating & Managing Warps

Everything a builder needs lives under `/warp`:

| Action | Command | Permission |
|--------|---------|------------|
| Create at your position | `/warp create <name>` (alias `set`) | `uxmessentials.warp.set` |
| Re-anchor to your position | `/warp move <name>` | `uxmessentials.warp.move` |
| Delete | `/warp del <name>` | `uxmessentials.warp.delete` |
| Inspect | `/warp info <name>` | `uxmessentials.warp.info` |

`/warp info` is where the metadata surfaces: owner, creation time, cost, and the visitor counter.

---

## The Warp Editor GUI

For the richer settings, `/warp editor [name]` opens a management GUI (permission `uxmessentials.warp.edit`). Rather than remembering a dozen flags, staff configure a warp visually:

- **Cost** — how much using the warp charges.
- **Gates** — permission/state requirements to use it.
- **Effects** — potion/particle effects applied on arrival.
- **Welcome** — the message shown when a player arrives.
- **Icon** — the item shown for the warp in list GUIs.

This is the recommended way to set up anything beyond a bare teleport point.

---

## Costs, Locks & Passwords

Warps can be gated three complementary ways:

| Mechanism | Set with | Bypass node |
|-----------|----------|-------------|
| **Cost** — charge on use | `/warp editor` (cost field) | `uxmessentials.economy.bypasscmdcost` |
| **Lock** — closed to everyone | `/warp lock <name>` | `uxmessentials.warp.bypass.lock` |
| **Password** — must be entered | `/warp password <name> [pw]` | `uxmessentials.warp.bypass.password` |

Costs draw through the DB-backed [economy](economy.md). Locking is the quickest way to take a warp offline (for an event that hasn't started) without deleting it. Passwords let you share a semi-private destination with people you hand the password to. Running `/warp password <name>` with no password clears it.

<Callout type="info" title="Safety bypass">

A warp whose anchor has become unsafe (lava, a drop, a wall) is refused for normal players. Holders of `uxmessentials.warp.bypass.safety` teleport anyway — handy for staff fixing a broken warp.

</Callout>

---

## Per-Warp Access Gates

By default, `uxmessentials.warp.use` lets a player use **any** warp. To restrict individual warps, uxmEssentials adds a per-warp gate:

```
uxmessentials.warp.use.<warp>
```

The pattern is powerful for tiered access — a `vip` warp reachable only by holders of `uxmessentials.warp.use.vip`, for example. Whether a per-warp gate is *required* depends on how the warp is configured; the gate is always *available* to grant.

---

## Cooldowns

Warp cooldowns are granted through a numbered tier, so different ranks can wait different amounts of time:

```
uxmessentials.warp.cooldown.<seconds>
```

The highest matching value wins, matching how teleport and kit tiers behave across the plugin.

---

## Ratings

Players can rate warps 1–5, which is a lightweight way to let the community surface the best destinations:

| Command | Description | Permission |
|---------|-------------|------------|
| `/warp rate <name> <1-5>` | Rate a warp | `uxmessentials.warp.use` |
| `/warp rating <name>` | Show a warp's current rating | `uxmessentials.warp.use` |

Ratings are stored with the warp and shown in its info.

---

## Warp Signs

Warps don't have to be reached by command. A `[warp]` sign teleports a player who clicks it:

| Action | Permission |
|--------|------------|
| Create a `[warp]` sign | `uxmessentials.warp.sign.create` |
| Use a `[warp]` sign | `uxmessentials.warp.sign.use` |

This is ideal for a physical warp hub where players walk up to a wall of signs.

---

## Key Settings (module config)

Warps are configured in `modules/warps/config.conf`. Warp *definitions* themselves are operator-created (there are no shipped warps), so the module config is small:

| Key | What it does |
|-----|--------------|
| `list-display` | `gui` or `chat` — how `/warp list` presents warps. |

The interesting per-warp settings (cost, effects, welcome, icon, gates) live on each warp and are edited through `/warp editor`, not in this file.

---

## Permissions at a Glance

| Node | Purpose | Default |
|------|---------|---------|
| `uxmessentials.warp.use` | Use warps; rate them | `true` |
| `uxmessentials.warp.use.<warp>` | Per-warp use gate | — |
| `uxmessentials.warp.others` | Send another player to a warp | — |
| `uxmessentials.warp.list` | List usable warps | — |
| `uxmessentials.warp.set` / `.move` / `.delete` | Create / re-anchor / delete | `op` |
| `uxmessentials.warp.info` | View warp metadata | — |
| `uxmessentials.warp.lock` / `.password` | Lock / password a warp | `op` |
| `uxmessentials.warp.edit` | Open the warp editor GUI | `op` |
| `uxmessentials.warp.cooldown.<seconds>` | Per-warp cooldown tier | — |
| `uxmessentials.warp.bypass.lock` / `.password` / `.safety` | Bypass gates | — |
| `uxmessentials.warp.sign.create` / `.sign.use` | Warp signs | — |

---

## Tips & Gotchas

- **Lock instead of delete** for temporary closures — deleting throws away the cost, effects, welcome, rating, and visitor count.
- **`/warp password` with no argument clears** the password; there is no separate "remove password" command.
- **Per-warp gates are opt-in.** Simply having `uxmessentials.warp.use` reaches every warp unless the warp is configured to require its own `use.<warp>` gate.
- **Costs and the bypass node.** Warp costs are treated as command costs, so `uxmessentials.economy.bypasscmdcost` (not a warp-specific node) is what waives them.
- **The visitor counter is descriptive, not a limit.** It's a usage stat shown in `/warp info`, not something that gates access.

---

## Next Steps

- [🏠 Homes & Warps Commands](../commands/homes-warps.md) - The full command reference
- [🌐 Player Warps](player-warps.md) - The player-owned counterpart
- [💰 Economy](economy.md) - The currency behind warp costs
- [🔐 Permission Reference](../permissions/reference.md) - Per-warp gates, cooldown tiers, and bypasses
- [⚙️ Per-Module Config](../config/per-module.md) - The `modules/warps/config.conf` keys

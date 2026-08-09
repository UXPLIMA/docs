---
title: Kits
order: 1260
description: Kits are pre-defined bundles of items (and, optionally, commands) that
  players claim with a single command. uxmEssentials kits are fully operator-authored,
  gated by permission, rate-limited by cooldown, previewable before claiming, and
  editable from an in-game GUI — no restart, no code.
---

The whole feature lives in the **`kits`** module. Turn the module off and it wires
nothing: no `/kit`, no listeners, no state.

---

## How Kits Work

A kit is a named definition you create once and players claim many times. Claiming
runs three checks, in order:

```
1. GATE       Does the player have uxmessentials.kit.<id>?
   └── No  → the kit is hidden / claim denied
   └── Yes → continue
2. COOLDOWN   Has the player's cooldown for this kit elapsed?
   └── No  → claim refused, remaining time shown
   └── Yes → continue
3. DELIVERY   Is there room in the inventory?
   └── Deny-on-full → claim refused, nothing consumed
   └── Otherwise    → items granted, cooldown stamped
```

Kits are **GUI-first**. `/kit` with no arguments opens the browse menu, where
players see the kits they can claim, their cooldown state, and — for buy-to-unlock
kits — the price. Everything a player does day-to-day happens through that menu; the
raw subcommands exist for muscle memory and scripts.

<Callout type="info" title="Kit claim &amp; cooldown stamps are PDC-backed">

Unlike vaults or the economy, a player's per-kit claim and cooldown stamps are
stored in **PDC** (per-holder transient state), not the database. This is the one
place uxmEssentials deliberately keeps state on the player rather than in the DB —
a rolled-back world could reset a kit cooldown, which is acceptable.

</Callout>

---

## Commands

`/kit` has the aliases `k` and `kits`. The bare command opens the browse GUI.

| Command | What it does | Permission |
|---------|--------------|------------|
| `/kit` · `/kit list` | Open / list the kits you can claim | `uxmessentials.kit.use` |
| `/kit <name>` | Claim a kit | `uxmessentials.kit.use` |
| `/kit <name> <player>` | Give a kit to another player | `uxmessentials.kit.others` |
| `/kit show <name>` | Preview a kit's contents **without** claiming it | `uxmessentials.kit.preview` |
| `/kit create\|del\|editor …` | Define / delete / edit a kit | `uxmessentials.kit.edit` |
| `/kit reset <player> [kit]` | Clear a player's claim / cooldown stamps | `uxmessentials.kit.reset` |

`/kit show <name>` renders the exact items a kit will hand out, so players can decide
before spending a cooldown or currency. It never consumes anything.

---

## The Kit Editor

`/kit create <name>` and `/kit editor <name>` open the editor GUI, which is the
intended way to build and tune a kit. From the editor you set:

- **Contents** — drag the items (and their full meta: names, lore, enchantments, model data) straight into the grid.
- **Cooldown** — how long before the same player can claim again.
- **Cost / buy-to-unlock** — an economy price to claim, or a one-time unlock purchase.
- **First-join / starter behaviour** — mark a kit to be handed out automatically the first time a player joins.
- **Deny-on-full** — whether a claim is refused (nothing consumed) when the player's inventory is full, versus dropping the overflow.

Kit definitions are **operator-created and persisted** — nothing ships out of the
box, so a fresh install has an empty kit list until you build one.

<Callout type="tip" title="Editing is live">

Saving in the editor updates the kit immediately for everyone. There's no reload
step for kit *contents* — `/uxmess reload kits` is only needed if you change the
module's own `config.conf`.

</Callout>

---

## Cooldowns

Every kit has a base cooldown set in its definition. On top of that, cooldowns
resolve through the shared **Cooldowns port**, so you can grant a different cooldown
per rank with a numbered permission tier:

| Node | Effect |
|------|--------|
| `uxmessentials.kit.cooldown.<seconds>` | Grants a cooldown tier of `<seconds>` for kits |
| `uxmessentials.kit.cooldown.bypass` | Ignores the cooldown entirely (claim any time) |

For example, granting `uxmessentials.kit.cooldown.3600` gives a holder a one-hour
cooldown, while `uxmessentials.kit.cooldown.bypass` lets staff test kits freely.

---

## Permissions & Gating

| Node | Default | Grants |
|------|---------|--------|
| `uxmessentials.kit.use` | `true` | Browse and claim kits |
| `uxmessentials.kit.preview` | `true` | Use `/kit show` |
| `uxmessentials.kit.others` | `op` | Give a kit to another player |
| `uxmessentials.kit.edit` | `op` | Create / delete / edit kits |
| `uxmessentials.kit.reset` | `op` | Clear another player's claim & cooldown stamps |
| `uxmessentials.kit.<id>` | — | **Per-kit gate**: access to the kit whose id is `<id>` |
| `uxmessentials.module.kits` | `op` | Reload / inspect the kits module |

The **per-kit gate** `uxmessentials.kit.<id>` is the key to rank-locked kits: create
a `vip` kit, grant `uxmessentials.kit.vip` to your VIP group, and only they see and
claim it. A "buy-to-unlock" kit combines a purchase (the editor's cost setting) with
this gate.

---

## Key Settings

The module's own tunables live in `modules/kits/config.conf` (HOCON):

```hocon
# modules/kits/config.conf
enabled = true
list-display = "gui"      # gui | chat — how /kit and /kit list render
showkit-display = "gui"   # how /kit show previews a kit
```

Per-kit properties (contents, cooldown, cost, deny-on-full, starter flag) are **not**
in this file — they belong to each kit definition and are edited through the GUI.

---

## Tips & Gotchas

<Callout type="warning" title="Deny-on-full protects the claim">

With deny-on-full enabled, a player whose inventory is full has their claim
**refused** and nothing is consumed — the cooldown is not stamped and any cost is
not charged. Make sure players know to clear space before claiming a paid kit.

</Callout>

- **Starter kits** are handed out on first join only; use `/kit reset <player> <kit>` if you need to re-issue one for testing.
- **`/kit reset`** clears both the claim record and the cooldown, so it doubles as a "let this player claim again now" tool.
- Kits imported from EssentialsX (`/uxmess import essentialsx`) land in the same file-backed catalog, so they show up in `/kit` and the editor exactly like hand-built ones.
- Developers can hook the Bukkit-facing `KitPreClaimEvent` (cancellable) and `KitClaimEvent` to veto or react to claims.

---

## Next Steps

- [📟 Kits & Vaults Commands](../commands/kits-vaults.md) - Full command reference
- [🔐 Permission Reference](../permissions/reference.md) - Every kit node and cooldown tier
- [⚙️ Per-Module Config](../config/per-module.md) - The `kits` module block
- [💰 Economy](economy.md) - Wire up buy-to-unlock and paid claims

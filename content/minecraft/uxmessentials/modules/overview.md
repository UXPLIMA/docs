---
title: Modules & Reloading
order: 940
---

## Everything Is a Module

uxmEssentials isn't one monolithic blob of features, it's **34 independent
modules** (bounded contexts) wired together. Homes is a module. The economy is a
module. Moderation, holograms, NPCs, the scoreboard, the world manager: each is
its own self-contained module.

This matters because of one simple rule:

<Callout type="tip" title="A disabled module costs you nothing">

When a module is off it registers **no commands**, adds **no listeners**, runs
**no database migrations**, ticks **no background tasks**, and holds **zero**
runtime state. Turn off what you don't use — it is genuinely free.

</Callout>

So if you only want homes, warps and an economy, disable the rest and run a lean
server. Nothing you switch off is loaded.

---

## Turning a Module On or Off

Every module has its own config file, and the very first key in it is `enabled`.
To switch a module off:

1. Open `plugins/uxmEssentials/modules/<module>/config.conf`.
2. Set `enabled = false`.
3. Reload it: `/uxmess reload <module>`.

```
# plugins/uxmEssentials/modules/holograms/config.conf
holograms {
    enabled = false
    ...
}
```

That's it — no `modules.conf`, no central switchboard. The `enabled` key inside
each module's own file *is* the switch. Flip it back to `true` and reload to bring
the module back. **Disabling a module never deletes its data**, so you can safely
turn it off and on again.

---

## The Management Commands

| Command | What it does |
|---------|--------------|
| `/uxmess status` | Lists every module and whether it's enabled |
| `/uxmess doctor` | Runtime health checks — database, economy provider, soft-depends, threading |
| `/uxmess reload [module]` | Reload one module by id, or everything if you omit the id |

`/uxmess reload <module>` re-reads just that module's config, stops and restarts it
off the main thread, and republishes its commands — no full server restart needed.

The admin root `/uxmess` also answers to `/uxmessentials` and `/uxe`.

---

## The 34 Modules

| Module id | Name | What it does |
|-----------|------|--------------|
| `teleport` | Teleport | `/tpa`, `/back`, `/rtp`, `/spawn` and staff teleports, with warmups & cooldowns |
| `worlds` | Worlds | Multi-world create/import, gamerules, generators, pregeneration, backups |
| `homes` | Homes | Slot-based, GUI-first personal homes, invites and visiting |
| `economy` | Economy | DB-backed multi-currency wallet, `/pay`, `/baltop`, banks and loans |
| `warps` | Warps | Server-wide warps with cost, lock, password and ratings |
| `kits` | Kits | Claimable kits with cooldowns, previews and an editor GUI |
| `playerstate` | Player State | `/god`, `/fly`, `/heal`, `/feed`, `/gamemode`, `/speed` and info verbs |
| `messaging` | Messaging | Private messages, mail, ignore lists, helpop and socialspy |
| `presence` | Presence | AFK, `/list`, `/nick`, `/whois`, `/gc`, `/staff` |
| `moderation` | Moderation | Mute, jail, ban, tempban, warn, freeze — with history and a GUI |
| `itemworld` | Item & World | ~65 item, workstation, cleanup and admin-fun verbs (7 sub-groups) |
| `vaults` | Vaults | DB-persisted player item storage with quotas and optional cost |
| `communication` | Communication | Join/quit messages, rotating announcer, info pages, chat control |
| `holograms` | Holograms | Native-display holograms with action chains and leaderboards |
| `playerwarps` | Player Warps | Player-owned warps keyed by owner and name |
| `scoreboard` | Scoreboard | Per-player sidebar; also drives the tablist header/footer |
| `tablist` | Tablist | Packet/config-driven tablist (no commands) |
| `nametags` | Nametags | Packet/config-driven above-head nametags (no commands) |
| `vote` | Vote | Votifier-bridged vote rewards, vote party and leaderboards |
| `discordlink` | Discord Link | Self-service account linking (redeemed in the Discord bridge) |
| `staff` | Staff | Staff-mode-only loadout swap, gadget hotbar and staff chat |
| `vanish` | Vanish | PremiumVanish-class invisibility with layered see/use levels |
| `npc` | NPC | Server-wide packet NPCs (fake players/entities) with action chains |
| `custommenus` | Custom Menus | The operator surface over the data-driven menu engine (`/menu`) |
| `survival` | Survival | Opt-in gameplay mechanics: auto-smelt, auto-tool, auto-sell, auto-replant and friends |
| `ranks` | Ranks | Rankup, prestige and autorank on a DB-backed ladder, no permission plugin required |
| `poses` | Poses | GSit-parity `/sit`, `/lay`, `/bellyflop`, `/spin` and `/crawl` |
| `trade` | Trade | Secure player-to-player trading with items, money and two-sided confirmation |
| `security` | Security | Opt-in second factor, op-command protection and same-IP alt detection |
| `commandcontrol` | Command Control | Decide which commands each player may run, and hide the rest from tab-complete |
| `invrollback` | Inventory Rollback | DB-backed inventory snapshots at death and logout, restorable by staff |
| `villagers` | Villagers | Villager trade management: never-locking trades, restock timers, a trade editor |
| `regions` | Regions | A GUI over WorldGuard regions: browse, edit flags, manage the roster |
| `servertweaks` | Server Tweaks | A grab-bag of small infrastructure toggles, each independently switched |

Each module id is the exact token you use everywhere: the config path
(`modules/<id>/config.conf`), the reload command (`/uxmess reload <id>`), and the
tier permission node below.

---

## What a Fresh Install Starts With

Most modules are on out of the box. Nine ship **off**, because each of them either
changes something a server may already handle another way or needs something the
plugin cannot assume you have:

| Module | Why it starts off | Turn it on when |
|---|---|---|
| `scoreboard` | Rewrites the sidebar every player sees | You want uxmEssentials to own the sidebar |
| `tablist` | Rewrites the tab-list header and footer | You want uxmEssentials to own the tab list |
| `nametags` | Replaces the above-head name with a display entity | You want custom nametags and no other plugin draws them |
| `survival` | Changes how vanilla blocks break and drop | You want tree-feller, veinminer, auto-smelt and friends |
| `villagers` | Changes how villager trading works | You want never-locking trades, restock timers or the trade editor |
| `discordlink` | Needs a Discord bot token to do anything | You have configured the Discord bridge |
| `vote` | Needs Votifier and a vote-site listing | Your server is listed and Votifier is installed |
| `regions` | Is a front end for WorldGuard | WorldGuard is installed |
| `invrollback` | Writes an inventory snapshot on every death and logout | Your staff actually restore inventories |

Turning one on is the same two steps as turning anything off: set `enabled = true`
in `modules/<id>/config.conf`, then `/uxmess reload <id>`. `/uxmess status` prints
the live state of every module, so it is the quickest way to see where you stand.

Everything not in that table ships **enabled**.

---

## Two Special Cases

A couple of things behave a little differently from the 34 toggleable modules:

- **`migration`** — the EssentialsX / Vault / LiteBans importer. It ships
  **disabled** because it's a one-shot admin tool: enable it for a cutover, run
  your import, then switch it back off. See
  [Migrating from EssentialsX](../getting-started/migration.md).
- **`management`** — the in-game management GUI framework that backs `/uxmess gui`
  and every module's admin panel. It's **always on** and isn't a toggleable module.

---

## The Module Permission Node

Each module has a tier node used to gate its reload and inspect access:

```
uxmessentials.module.<id>        # e.g. uxmessentials.module.economy
```

This node defaults to `op`. It (together with `uxmessentials.admin.reload`)
controls who can reload or inspect that specific module.

---

## Next Steps

- [🧠 Core Concepts](../getting-started/concepts.md) — modules, config, permissions and messages
- [⚙️ Per-Module Config](../config/per-module.md) — what's inside each module's file
- [📟 Commands Overview](../commands/overview.md) — every command, grouped by module
- [🔑 Permission Reference](../permissions/reference.md) — module tiers and quota nodes

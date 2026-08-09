---
title: Overview
order: 950
---

## How Commands Work

Every uxmEssentials command is registered through **Brigadier**, Paper's native command engine. In practice that means three things you get for free:

- **Tab completion** — press **TAB** while typing any command to see valid arguments: player names, warp names, currencies, kit ids, world names, and so on.
- **Permission-filtered suggestions** — you only ever see arguments (and, in `/help`, whole commands) that you actually have permission to run.
- **Inline usage** — type a command wrong and the client underlines the offending argument and shows the expected syntax before you press enter.

All player-facing text is **MiniMessage**-formatted and resolved from a per-locale message catalog, so colours, hover text, and click actions render natively in chat.

<Callout type="tip" title="Discover what you can run">

`/help` lists (and searches) every command available to *you*, one page at a time. Because the list is permission-filtered per line, it is the fastest way to learn what your rank unlocks. `/help <query>` searches by keyword.

</Callout>

---

## The `/uxmess` Admin Root

Server-wide administration lives under a single root command: `/uxmess` (aliases `/uxmessentials`, `/uxe`). It manages the 23 feature modules, runs diagnostics, and imports legacy data.

| Command | Description | Permission |
|---------|-------------|------------|
| `/uxmess` | Admin root; bare opens the module management hub or prints help | `uxmessentials.admin` |
| `/uxmess status` | List all modules and their enable state | `uxmessentials.admin` |
| `/uxmess doctor` | Runtime health checks (DB, economy provider, soft-depends, threading), run off-tick | `uxmessentials.admin` |
| `/uxmess gui` | Open the module management hub | `uxmessentials.gui` |
| `/uxmess reload [module]` | Reload all modules, or one by id | `uxmessentials.admin.reload` |
| `/uxmess import <source> [--dry-run\|dry-run]` | Import legacy data (e.g. from EssentialsX) | `uxmessentials.admin.import` |
| `/uxmess import --list` | List the built-in import sources | `uxmessentials.admin.import` |
| `/backup` | Snapshot the plugin data directory on demand (off-tick) | `uxmessentials.admin.backup` |
| `/help [page-or-query]` | List/search the commands you can use (per-line permission-filtered) | `uxmessentials.help` |
| `/lang [code\|reset]` | Show / set / clear your personal language override | `uxmessentials.lang.use` |

<Callout type="warning" title="Always dry-run an import first">

Run `/uxmess import <source> --dry-run` before the real thing. The dry run reports exactly what *would* change without writing a single row, so you can review conflicts before committing.

</Callout>

<Callout type="info" title="Reloading a single module">

`/uxmess reload <module>` re-reads just that module's config subtree, restarts it off-thread, and re-publishes its commands — no server restart, no data loss. Reloading one module is additionally gated by that module's tier node `uxmessentials.module.<module>`.

</Callout>

### Examples

```
/uxmess status
/uxmess doctor
/uxmess reload economy
/uxmess import essentialsx --dry-run
/lang tr
/lang reset
```

---

## Curated Default Aliases

On top of each command's own aliases, uxmEssentials folds in a set of short, familiar aliases so muscle memory from other suites keeps working:

| Command | Aliases |
|---------|---------|
| `/msg` | `m`, `tell`, `whisper`, `w` |
| `/reply` | `r` |
| `/vanish` | `v` |
| `/balance` | `bal`, `money` |
| `/eco` | `economy` |
| `/gamemode` | `gm` |
| `/home` | `h`, `homes` |
| `/warp` | `wp`, `warps` |
| `/kit` | `k`, `kits` |
| `/feed` | `eat` |
| `/back` | `return` |
| `/afk` | `away` |
| `/god` | `godmode` |
| `/near` | `nearby` |
| `/itemlore` | `lore` |
| `/itemname` | `iname` |
| `/itemflag` | `iflag` |
| `/itemdb` | `idb` |

---

## Renaming, Aliasing & Disabling Commands

Every command can be renamed, given different aliases, or turned off entirely — no code, no restart. The whole command surface is controlled by one file, `commands/commands.conf`, keyed by each command's **stable id**:

```hocon
commands {
  tpaccept { enabled = true, name = "tpaccept", aliases = ["tpyes"] }
}
```

- Set `enabled = false` to drop a command.
- Edit `name` to change the primary literal players type.
- Edit `aliases` to change the alternates.

<Callout type="note" title="Permission nodes never move">

Permission nodes are keyed to the command's stable id, **not** its typed name. Renaming `/tpaccept` to `/yes` does not change `uxmessentials.tpa.use` — your permission grants keep working untouched.

</Callout>

See [Command Configuration](../config/commands-conf.md) for the full file reference.

---

## Permission Defaults

uxmEssentials ships with sensible defaults so a fresh install is playable immediately:

- **Self-service verbs default to `true`** — read-only and own-account commands like `/home`, `/balance`, `/msg`, `/afk`, `/list`, `/warp`, `/kit`, `/vault`, `/help`, and `/lang` are available to everyone out of the box.
- **Staff verbs default to `op`** — mutations, moderation, and cross-player actions (`/tp`, `/eco give`, `/ban`, `/vanish`, item/world verbs, hologram & NPC admin) require operator or an explicit grant.
- **Quotas are open-ended, numbered nodes** — home/warp/vault limits and teleport warmups/cooldowns are granted per tier (e.g. `uxmessentials.home.limit.5`).

For the full node reference — including quota tiers, bypass nodes, and per-instance gates — see the [Permission Reference](../permissions/reference.md).

---

## Jump To

- [Homes, Warps & Player Warps](homes-warps.md) — `/home`, `/warp`, `/pwarp` and friends
- [Teleportation](teleport.md) — `/tpa`, `/back`, `/rtp`, `/spawn`, and staff teleports
- [Economy](economy.md) — `/balance`, `/pay`, `/baltop`, `/eco`, banks, loans, and shops

---

## Next Steps

- [🔐 Permission Reference](../permissions/reference.md) - Every node, default, and quota tier
- [⚙️ Command Configuration](../config/commands-conf.md) - Rename, alias, and disable commands

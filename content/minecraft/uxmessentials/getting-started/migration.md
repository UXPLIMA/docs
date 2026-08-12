---
title: Migrating from EssentialsX
order: 910
description: Already running EssentialsX (or a few other plugins)? You don't have
  to lose your players' data. uxmEssentials ships a built-in importer that reads your
  existing files and pulls the data across in one shot.
---

---

## How It Works

The importer is a single admin command:

```
/uxmess import <source> [--dry-run]
```

- It runs **off the main thread**, so your server never freezes during an import.
- It is **idempotent**: every record is written as an *upsert* keyed by identity.
  Balances are **set, never added**, so re-running an import can never double a
  player's money.
- It's **safe to run again** if something looked off, or if new data appeared.

To see which sources are available on your setup:

```
/uxmess import --list
```

You need the `uxmessentials.admin.import` permission (operators have it by default).

<Callout type="warning" title="Back up first, then dry-run">

**Always take a backup of your world and plugin data before importing**, and
**always run with `--dry-run` first**. A dry run produces the *exact same*
per-record report as a real import (it just doesn't write anything) so you
can review precisely what would change before committing.

</Callout>

---

## Enabling the Importer

The importer lives in the `migration` module, which is the **only module that ships
disabled** (it's a one-shot admin tool, not something you want running day to day).

1. Open `plugins/uxmEssentials/modules/migration/config.conf`.
2. Set `enabled = true`.
3. Run `/uxmess reload migration`.

When you're done migrating, set it back to `enabled = false` and reload again.

---

## Import Sources

These are the sources uxmEssentials can read:

| Source | Brings across | Notes |
|--------|---------------|-------|
| `essentialsx` | Homes, balance (default currency), mail, warps, kits, jail/mute | Reads the EssentialsX file tree |
| `vault` | Balance | Live read of the running Vault economy |
| `playerpoints` | Balance | Live read of the running PlayerPoints plugin |
| `litebans` | Bans, IP-bans, mutes, warns | Reads the LiteBans database over JDBC |
| `decentholograms` | Holograms | Reads `plugins/DecentHolograms/holograms/*.yml` |
| `fancyholograms` | Holograms | Reads `plugins/FancyHolograms/holograms.yml` |
| `axplayerwarps` | Player warps | Reads the AxPlayerWarps database over JDBC |
| `athelionplayerwarps` | Player warps | Reads Athelion's serialised `data.yml` |
| `olzieplayerwarps` | Player warps | Reads the Olzie PlayerWarps database over JDBC |
| `multiverse` | The world registry | Reads `plugins/Multiverse-Core/worlds.yml` (version 4 and 5 layouts) |

### What EssentialsX brings over

From each player's `userdata/<uuid>.yml`, plus `warps/`, `kits.yml` and `jail.yml`:

- ✅ **Homes**
- ✅ **Balance** (default currency only)
- ✅ **Mail**
- ✅ **Warps**
- ✅ **Kits** (written into the kit catalog)
- ✅ **Jail & mute state**

**Not migrated:** nicknames / display names, chat formatting, powertools,
per-permission kit-delay overrides, GeoIP data, spawn-on-join, and non-native or
multi-economy balances. EssentialsX has no player-vault data, so no vaults are
produced.

### Coming from another player-warps plugin

Three player-warp plugins can be imported, and each brings across more than the
destinations themselves:

| Source | What comes over |
|---|---|
| `axplayerwarps` | Warps with their ratings, whitelist, blacklist (imported as warp **bans**), favourites and visit tallies |
| `athelionplayerwarps` | Warps with access derived from the password/status gate, ratings, and blocked players (imported as warp bans) |
| `olzieplayerwarps` | The richest source: warps with ratings, whitelist, **managers** (imported as warp members), bans with their reason, and favourites |

Each is configured in its own block in `modules/migration/config.conf`. The two JDBC
sources take a `jdbc-url`, `username` and `password`; leave `jdbc-url` empty and the
importer auto-detects a local H2 file under the source plugin's folder and opens it
**read-only**.

### Coming from Multiverse-Core

```
/uxmess import multiverse dry-run
/uxmess import multiverse
```

This reads `plugins/Multiverse-Core/worlds.yml` and registers every world it lists in our
own [worlds module](../modules/worlds.md): name, alias, auto-load, environment, seed,
generator, per-world spawn, and the difficulty, PvP, forced gamemode, player-limit and
entry-fee settings. Both on-disk layouts are read, so it does not matter whether you are on
Multiverse 4 or 5.

Worlds are registered **in place**. Nothing is regenerated and no chunk is touched: the
import claims the folder that is already on disk, exactly as `/world import` does.

A few things deliberately do not come across:

- **Gamerules.** Multiverse does not store them. They live in the world's own `level.dat`,
  we never rewrite that, so they survive the import untouched.
- **Values you left at Multiverse's default.** Only settings you actually chose are written,
  so Multiverse's defaults do not silently become explicit overrides of ours.
- **Per-world weather, hunger, auto-heal, respawn and flight flags, world scale, biome and
  the hidden/blacklist flags.** These have no counterpart in our worlds module.
- **An entry fee charged in items.** Ours is charged in money, so only a money fee imports.

<Callout type="warning" title="Don't leave both managing your worlds">

Multiverse and uxmEssentials both manage worlds, and where their command names overlap
whichever plugin registers one first wins. Import your worlds, then remove Multiverse (or
disable our `worlds` module) rather than running both. `/uxmess doctor` warns about the
overlap while both are installed.

</Callout>

<Callout type="info" title="Where does it look?">

The EssentialsX path defaults to `plugins/Essentials` and can be changed with
`source-path` in `modules/migration/config.conf`. For LiteBans and the two JDBC
player-warp sources, an empty `jdbc-url` auto-detects the local database.

</Callout>

---

## Conflict Policy

When an incoming record collides with something already in uxmEssentials, the
`on-conflict` setting in `modules/migration/config.conf` decides what happens:

<Tabs>
<Tab label="skip (default)">


Keep what's already there. The safest choice; nothing existing is overwritten.

</Tab>
<Tab label="overwrite">


The imported value replaces the existing one.

</Tab>
<Tab label="merge">


Combine both: warps/homes are unioned by name, mail is de-duplicated, and for
moderation records the longest-remaining expiry wins.

</Tab>
</Tabs>

Balances have their own conservative guard, `balance-policy` (default
`skip-if-present`), so an import won't quietly stomp on money a player already has.

Imported homes and vaults go through the same rules as `/sethome` and `/vault`, so
your per-group **quotas still apply** to migrated data.

---

## Recommended Checklist

1. **Back up** your world folder and `plugins/` directory.
2. **Install** uxmEssentials and start the server once so it generates its files.
3. **Enable** the `migration` module (`enabled = true` → `/uxmess reload migration`).
4. Check available sources with `/uxmess import --list`.
5. **Dry-run** first: `/uxmess import essentialsx --dry-run` and review the report.
6. Set your `on-conflict` / `balance-policy` if you want anything other than the
   safe defaults.
7. **Import for real:** `/uxmess import essentialsx`.
8. Repeat for any other sources (`vault`, `litebans`, ...).
9. **Spot-check** in-game: `/home`, `/balance`, `/warp list`, `/kit`.
10. **Disable** the `migration` module again and remove the old plugin.

---

## Next Steps

- [🔧 Installation](installation.md): if you haven't installed the plugin yet
- [🧠 Core Concepts](concepts.md): how modules, config and permissions fit together
- [📟 Commands Overview](../modules/index.md): the full command set
- [🔑 Permission Reference](../permissions/reference.md): quotas and node families

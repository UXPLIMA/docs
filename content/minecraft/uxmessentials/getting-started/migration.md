---
title: Migrating from EssentialsX
order: 910
description: 'The one-shot importer: sources, what comes across, conflict policy and the order to run it in.'
---

`/uxmess import <source> [--dry-run]` reads another plugin's data and writes it into uxmEssentials. It runs
off the main thread, and every record is an upsert keyed by identity, so balances are set rather than added
and a second run cannot double anyone's money. `/uxmess import --list` shows the sources built into your
copy. The command needs `uxmessentials.admin.import`, which operators hold.

<Callout type="warning" title="Back up, then dry-run">

Back up the world and `plugins/` before importing, and run with `--dry-run` first. A dry run prints the same
per-record report as a real import and writes nothing.

</Callout>

## Enabling the importer

The importer lives in the `migration` module, which ships disabled because it is a one-shot tool.

1. Open `plugins/uxmEssentials/modules/migration/config.conf`.
2. Set `enabled = true`.
3. Run `/uxmess reload migration`.

Set it back to `false` when the migration is done.

## Sources

| Source | Brings across | Read from |
|---|---|---|
| `essentialsx` | Homes, balance, mail, warps, kits, jail and mute state | The EssentialsX file tree |
| `vault` | Balance | The running Vault economy |
| `playerpoints` | Balance | The running PlayerPoints plugin |
| `litebans` | Bans, IP bans, mutes, warns | The LiteBans database over JDBC |
| `decentholograms` | Holograms | `plugins/DecentHolograms/holograms/*.yml` |
| `fancyholograms` | Holograms | `plugins/FancyHolograms/holograms.yml` |
| `axplayerwarps` | Player warps | The AxPlayerWarps database over JDBC |
| `athelionplayerwarps` | Player warps | Athelion's serialised `data.yml` |
| `olzieplayerwarps` | Player warps | The Olzie PlayerWarps database over JDBC |
| `multiverse` | The world registry | `plugins/Multiverse-Core/worlds.yml`, version 4 and 5 layouts |

Each source has its own block in `modules/migration/config.conf`. The EssentialsX path defaults to
`plugins/Essentials` and is changed with `source-path`. For LiteBans and the JDBC player-warp sources, an
empty `jdbc-url` auto-detects the local database and opens it read-only.

## EssentialsX

Homes, the default-currency balance, mail, warps, kits and jail and mute state come across, read from each
`userdata/<uuid>.yml` plus `warps/`, `kits.yml` and `jail.yml`.

Nicknames, chat formatting, powertools, per-permission kit delays, GeoIP data, spawn-on-join and
multi-economy balances do not. EssentialsX has no player vaults, so no vaults are produced.

## Player warps

| Source | What comes over |
|---|---|
| `axplayerwarps` | Warps with ratings, whitelist, blacklist (as warp bans), favourites and visit tallies |
| `athelionplayerwarps` | Warps with access from the password and status gate, ratings, blocked players (as warp bans) |
| `olzieplayerwarps` | Warps with ratings, whitelist, managers (as warp members), bans with their reason, favourites |

## Multiverse

`/uxmess import multiverse` registers every world in `worlds.yml` in the
[worlds module](../modules/worlds.md): name, alias, auto-load, environment, seed, generator, per-world
spawn, difficulty, PvP, forced game mode, player limit and entry fee. Worlds are claimed in place, so
nothing is regenerated and no chunk is touched.

Gamerules stay in the world's own `level.dat`, which the import never rewrites. Settings you left at
Multiverse's default are not written, so its defaults do not become explicit overrides of ours. Per-world
weather, hunger, auto-heal, respawn and flight flags, world scale, biome and the hidden and blacklist flags
have no counterpart here. An entry fee charged in items does not import; ours is charged in money.

Where the two plugins share a command name, whichever registers first wins. Import your worlds, then remove
Multiverse or disable our `worlds` module rather than running both. `/uxmess doctor` warns while both are
installed.

## Conflicts

`on-conflict` in `modules/migration/config.conf` decides what happens when an incoming record collides with
one already here.

| Value | Result |
|---|---|
| `skip` | Keep what is already there. The default |
| `overwrite` | The imported value replaces the existing one |
| `merge` | Warps and homes are unioned by name, mail is de-duplicated, and the longest-remaining expiry wins for moderation records |

Balances have their own guard, `balance-policy`, which defaults to `skip-if-present`. Imported homes and
vaults pass through the same rules as `/sethome` and `/vault`, so quotas still apply.

## Order to run it in

1. Back up the world folder and `plugins/`.
2. Install uxmEssentials and start the server once.
3. Enable the `migration` module and reload it.
4. `/uxmess import --list`.
5. `/uxmess import essentialsx --dry-run`, then read the report.
6. Set `on-conflict` and `balance-policy` if the defaults are not what you want.
7. `/uxmess import essentialsx`.
8. Repeat for the other sources.
9. Check `/home`, `/balance`, `/warp list` and `/kit` in-game.
10. Disable the `migration` module and remove the old plugin.

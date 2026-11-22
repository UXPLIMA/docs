---
title: Importers
order: 1350
description: Bringing towns, nations, lands and claims over from Towny, Lands, GriefPrevention, HuskClaims and HuskTowns.
icon: import
---

The importers module reads another claim plugin's data and writes it into uxmClaims as lands, areas, towns and
nations. A dry run writes only a report, a real run records a batch you can undo, and the source data is never
changed. Turn the module on for the import and off again afterwards.

Module `importers` · disabled by default · `modules/importers/config.yml` · requires [Lands](lands.md)

## Commands

| Command | What it does | Permission |
|---|---|---|
| `/uxm import <source> dry-run` | Reads the source and writes a report of what an import would do. Nothing else is written. | `uxmclaims.admin.import` |
| `/uxm import <source> run [--force]` | Imports. Refused when the source was already imported, unless `--force`, which imports only what is missing. | `uxmclaims.admin.import` |
| `/uxm import status` | Lists import batches, newest first, with source, status, start time and counts. | `uxmclaims.admin.import` |
| `/uxm import undo <batch>` | Removes what one batch created: nations, then towns, then lands, then areas. | `uxmclaims.admin.import` |

`<source>` is `towny`, `lands`, `griefprevention`, `huskclaims` or `husktowns`. Only one import runs at a time.

## Permissions

| Node | Default | Grants |
|---|---|---|
| `uxmclaims.admin.import` | op | Every `/uxm import` command |

## Settings

Paths are relative to the server's `plugins` folder unless absolute.

| Key | Default | What it does |
|---|---|---|
| `backup` | `true` | Backs up the uxmClaims database to `modules/importers/backups/` before a run writes anything. SQLite: a copy of the file; MySQL, MariaDB, PostgreSQL: a gzip dump of the `uxmc_*` tables. The run stops if the backup fails. |
| `progress-every` | `25` | Lands, towns and nations written between two progress messages. |
| `respect-distance-rules` | `false` | `false` ignores the towns and lands minimum distance rules, so close source claims come over whole. `true` applies them and lists refused ones in the report. |
| `towny.storage` | `AUTO` | `AUTO`: SQL when `jdbc-url` is set, else the flatfile folder. `FLATFILE` or `SQL` forces one. |
| `towny.folder` | `Towny/data` | Towny's flatfile folder. |
| `towny.jdbc-url`, `username`, `password` | `''` | Towny's SQL database, such as `jdbc:mysql://localhost:3306/towny`. |
| `towny.table-prefix` | `towny_` | Towny's `table_prefix`. |
| `towny.town-block-size` | `16` | Towny's `town_block_size`. |
| `lands.read` | `AUTO` | `AUTO`: the Lands API when the Lands plugin is enabled, else the database. `API` or `DATABASE` forces one. |
| `lands.jdbc-url`, `username`, `password` | `''` | Lands' database, read only without the API. |
| `griefprevention.storage` | `AUTO` | `AUTO`, `FLATFILE` or `SQL`, as for Towny. |
| `griefprevention.folder` | `GriefPreventionData/ClaimData` | The flat files. |
| `griefprevention.jdbc-url`, `username`, `password`, `table` | `''`, `griefprevention_claimdata` | GriefPrevention's SQL database. |
| `griefprevention.claim-blocks-per-point` | `256` | Claim blocks (accrued plus bonus) per [claim point](claim-points.md). `0` skips claim blocks. |
| `huskclaims.jdbc-url` | `''` | HuskClaims' MySQL or MariaDB database. Empty: the SQLite file. |
| `huskclaims.file` | `HuskClaims/HuskClaimsData.db` | HuskClaims' SQLite file. |
| `huskclaims.server` | `''` | Only claim worlds of this HuskClaims server name, when servers share a database. |
| `huskclaims.*-table` | `huskclaims_claim_worlds`, `huskclaims_users`, `huskclaims_user_groups` | Table names. |
| `husktowns.jdbc-url` | `''` | HuskTowns' MySQL or MariaDB database. Empty: the SQLite file. |
| `husktowns.file` | `HuskTowns/HuskTownsData.db` | HuskTowns' SQLite file. |
| `husktowns.server` | `''` | Only claim worlds of this server name. |
| `husktowns.*-table` | `husktowns_town_data`, `husktowns_claim_worlds` | Table names. |
| `areas.bottom-y`, `areas.top-y` | `-64`, `319` | Height of areas made from Towny plots and GriefPrevention claims. |
| `areas.keep-exact-claims` | `true` | Keeps a rounded-out claim's exact box as an area named `claim` when [Areas](areas.md) is on. |
| `names.min`, `names.max` | `3`, `24` | Name length of new lands, towns and nations. Characters other than letters, digits, `_` and `-` become `_`. |

## Mapping files

`modules/importers/mappings/<source>.yml` says how the source's ranks, trust levels, roles, flags and permissions
become uxmClaims ones. Each file is written once and never overwritten, and its header explains every section.

| Source | What comes over |
|---|---|
| Towny | Towns (home land plus outposts), tag, board, open and public; nations with towns, capital, ranks; owned or special plots as one-townblock areas with members and flags; jails as jail cells. |
| Lands | Lands per world and connected piece, areas, members with their mapped role, nations. |
| GriefPrevention | Claims as lands rounded out to whole chunks, subdivisions as areas, trust lists as roles, `public` trust to the visitor role, claim blocks as claim points. |
| HuskClaims | Claims as lands, child claims as areas, trust levels as roles, `public` tag trust, claim bans when [Bans](bans.md) is on. |
| HuskTowns | Towns with ranks by role weight, bank balance to the town [bank](bank.md), bio as board; plot and farm chunks as areas; admin claims as admin lands; relations, levels and plot types when those modules are on. |

## Running an import

1. Turn the module on and the target modules ([Towns](towns.md), [Nations](nations.md), [Areas](areas.md) and
   so on) you want filled.
2. Set the source's paths or database in `config.yml` and check its mapping file.
3. Run `/uxm import <source> dry-run` and read the report in `modules/importers/reports/`.
4. Run `/uxm import <source> run`. Note the batch id it prints.
5. If the result is wrong, `/uxm import undo <batch>`, fix the mapping, and run again.

## Notes

- Lands without its plugin running is read from its database by table names that are guesses. Open your Lands
  database and fix the `schema` section of `mappings/lands.yml` before a dry run; the report lists tables it
  could not find.
- Not imported: Towny per-player plot overrides, outlaws, the nation public switch and bank balances (they stay
  in the economy plugin); GriefPrevention permission-node trust and group bonuses; Lands sub-area flags and
  roles; HuskClaims claim blocks (listed in the report), other trust tags and wilderness flags; HuskTowns
  greetings, farewells, bonuses, logs, colours and ongoing wars. HuskClaims on MongoDB is not read.
- Districts and plot groups are kept only as an attribute of their areas.
- Undo takes back bank balances only up to what each bank still holds. Money spent since stays spent.
- The Towny, GriefPrevention, HuskClaims, HuskTowns and Lands readers follow those plugins' sources but have not
  yet been run against a real server's data. Always dry-run first.

Related: [Lands](lands.md), [Towns](towns.md), [Areas](areas.md), [Upgrading from 3.x](../upgrading/from-3x.md)

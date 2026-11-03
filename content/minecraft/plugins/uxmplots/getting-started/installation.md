---
title: Installation
order: 102
description: Requirements, the first start, the plot world, the files, updating and checking the install.
icon: download
---

This manual describes uxmPlots after 0.1.0: German, Spanish, French, Portuguese and Russian arrive with the next
release, and 0.1.0 itself reads English and Turkish.

## Requirements

| Requirement | Detail |
|---|---|
| Java | 25 or newer |
| Server | Paper 26.2 or newer, or Folia |

uxmLib is shaded into the jar, so the plugin needs nothing else on the server. Prices, tips and fees are in diamonds
until you turn another currency on.

## Optional plugins

| Plugin | Adds |
|---|---|
| WorldEdit, FastAsyncWorldEdit | Edits held inside the plot a player may build on |
| Vault, VaultUnlocked, Treasury | Money currencies |
| PlaceholderAPI | The [placeholders](../placeholders/reference.md) |
| mcMMO | Skill levels in conditions |
| Redis (`lettuce-core`) | The `redis` network transport |
| uxmLang | One language choice for every UXM plugin |

## Installing

1. Drop `uxmPlots-<version>.jar` into `plugins/` and remove any older jar.
2. Start the server. The plugin writes its folder and, with `create-world = true` in `worlds/creative.conf`, creates
   the world `creative` with its own generator.
3. Adjust the world file and `config.conf`, then run `/uxmplots reload`.

On Folia a world cannot be created while the server runs: add it to the `worlds` section of `bukkit.yml` with
`generator: uxmPlots`, as the log says.

## The files

```
plugins/uxmPlots/
├── config.conf          language, storage, plots, limits, work, snapshots, visits, guestbook, tips, sessions,
│                        expiry, reports, leaderboard, follow, archive, personality, merge, crash items,
│                        featured, slots, menus
├── protection.conf      what an untrusted player may do, redstone, cross-boundary tricks
├── rating.conf          what makes a rating, trending and confidence
├── contests.conf        the contest boss bar and the vote reward
├── network.conf         how servers on one database hear from each other
├── worlds/<world>.conf  one plot world; the file name is the world name
├── contests/<id>.conf   one contest; the file name is the id
├── catalogues/          shop.conf, personality.conf, tags.conf
├── currencies.conf      what prices, fees, rents and tips are paid in
├── levels.conf          what raises a plot's level and what a level unlocks
├── discord.conf         webhook lines for plot and contest events
├── hologram.conf        the sign over a plot
├── text-input.conf      how typing is asked for
├── editor/              world.conf and contest.conf: the in-game editors' layouts
├── menus/               fifteen window files
├── messages/            messages_<language>.conf: en, tr, de, es, fr, pt, ru
├── boards.conf          boards planted in the world, written when you plant one
├── snapshots/, templates/, archive/
├── data/plots.db        the SQLite file
└── backups/
plugins/uxmTheme/
└── theme.conf
```

The examples (the world, the catalogues and the contest) are written once: one you delete stays deleted. A block an
earlier release kept in `config.conf` (`protection`, `rating`, `contests`, `network`) moves to its own file on the first
start.

## Updating

Stop the server, replace the jar and start it. A release that adds a key writes it into `config.conf` and the four
feature files with its comment and never changes a value you wrote; your file as it was is kept in `backups/`.

## Checking the install

`/uxmplots doctor` (alias `health`, permission `uxmplots.admin.doctor`) prints one line per check: storage,
configuration, worlds, world shapes, plots, estate worlds, network, currencies, shop, shop offers, tags, templates,
levels, work, merges, slots, market, tips, visits, sessions, follows, guestbook, reports, gallery rows, boards
standing, honours, contests, contest bars, contest votes, contest results, entries, archive, viewing, signs, crash
items, redstone, generating ahead, game modes, world edits and windows. Each says what it found, or what is wrong and
what that costs.

## Troubleshooting

- **A world will not start.** On Folia, add it to `bukkit.yml` with `generator: uxmPlots`.
- **A change to a world file is refused.** The world exists and its grid or layers would no longer match.
- **A WorldEdit edit leaves blocks unchanged.** Blocks outside the plot the player may build on are not written;
  `uxmplots.bypass` lifts this for staff.
- **`/plot auto` finds nothing.** Raise `plots.auto-search-rings`, or the world is full.

## Metrics

The plugin sends anonymous usage counts to bStats (plugin id 34255). Turn it off for every plugin in
`plugins/bStats/config.yml`.

Related: [Concepts](concepts.md), [Configuration](../config/index.md)

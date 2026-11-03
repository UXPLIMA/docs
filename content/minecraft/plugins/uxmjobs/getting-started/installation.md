---
title: Installation
order: 102
description: Requirements, the first start, the files that appear, updating and checking the install.
icon: download
---

This manual describes uxmJobs after 0.1.0: German, Spanish, French, Portuguese and Russian arrive with the next
release, and 0.1.0 itself reads English and Turkish.

## Requirements

| Requirement | Detail |
|---|---|
| Java | 25 or newer |
| Server | Paper 26.2 or newer, or Folia; work is priced on the thread that owns the block or the creature |

uxmLib is shaded into the jar, so the plugin needs nothing else on the server. Without an economy plugin, wages in
money are not paid (the console says so once), costs are taken in experience, and everything else works.

## Optional plugins

| Plugin | Adds |
|---|---|
| Vault, VaultUnlocked, PlayerPoints, EcoBits, Treasury | Money wages and costs |
| PlaceholderAPI | The [placeholders](../placeholders/reference.md), and placeholders inside price expressions |
| Nineteen claim plugins | Work in another player's claim pays less or nothing |
| RoseStacker | A stacked kill pays for every creature in the stack |
| mcMMO | Skill levels in requirements |
| uxmLang | One language choice for every UXM plugin |

The full list is on [Integrations](../integrations/overview.md).

## Installing

1. Drop `uxmJobs-<version>.jar` into `plugins/` and remove any older jar.
2. Start the server. The plugin writes its folder on the first boot, including eight example jobs.
3. Check `economy.conf` and the jobs under `jobs/`, then run `/uxmjobs reload`.
4. Run `/uxmjobs preview <job>` to see what a job pays at levels 1, 10, 50 and 100 before anyone plays.

## The files

```
plugins/uxmJobs/
├── config.conf            language, storage, slots, employment, ledger, decay, income cap, alerts, tax,
│                          server account, claims, zones, leaderboard, signs, report, explore, editor, menus
├── protections.conf       farm grades, creative, fertiliser, generators, vehicles, disabled worlds
├── economy.conf           what costs are taken in, the placeholder backend, bridges
├── display.conf           action bar, boss bar, chat prefix, dialogs, level up, payment actions
├── boosters.conf          permission boosters and how two combine
├── schedule.conf          boosters that run on a calendar
├── titles.conf            the title each level carries
├── shop.conf              what points buy
├── commands.conf          the names, aliases and switches of the three commands
├── jobs/                  one file per job: builder, enchanter, farmer, fisherman, hunter, miner, smith, woodcutter
├── contracts/             evening-shift.conf, weekend-crew.conf
├── zones/                 one file per work zone, written by the wand
├── menus/                 thirteen window files
├── messages/              messages_<language>.conf: en, tr, de, es, fr, pt, ru
├── languages.conf         the language each player chose, when uxmLang is not installed
├── data/jobs.db           the SQLite file
└── backups/               the last five copies of a file a release added keys to
plugins/uxmTheme/
└── theme.conf             the colours and glyphs every UXM plugin reads
```

The template the in-game editor starts a new job from, `editor/new-job.conf`, lives in the jar. A block an earlier
release kept in `config.conf` (`protections`, `economy`, `display`) moves to its own file on the first start.

## Updating

Stop the server, replace the jar and start it. A release that adds a key writes it into your `config.conf`,
`protections.conf`, `economy.conf` or `display.conf` with its comment and never changes a value you wrote; your file
as it was is kept in `backups/`.

## Checking the install

`/uxmjobs doctor` (permission `uxmjobs.admin.doctor`):

| Check | ok | warn or fail |
|---|---|---|
| `storage` | Where the rows are kept | fail: nobody can join a job or be paid |
| `jobs` | How many jobs were read | fail: none, so there is nothing to join |
| `currencies` | The money backends that answered | warn: none, so a job that pays money pays nothing |
| `zones` | How many zones are drawn, or that zones are off | warn: zones are on and none is drawn |
| `windows` | How many windows were read | fail: none, so nothing can open |
| `placeholders` | Published | warn: PlaceholderAPI is not installed |

## Troubleshooting

- **A reload is refused.** An expression or a key is wrong; the console names the file and the key.
- **A player is paid nothing for a block.** They placed it, it is a generator block, or the ledger already paid for that
  spot. `/jobs why` says which.
- **Pay drops during a long session.** That is decay or the income cap; `/jobs why` says which.
- **The economy pays nothing.** `/uxmjobs doctor` shows which backend answered.

## Metrics

The plugin sends anonymous usage counts to bStats (plugin id 34245). Turn it off for every plugin in
`plugins/bStats/config.yml`.

Related: [Concepts](concepts.md), [Configuration](../config/index.md)

---
title: "Guide"
order: 10
description: "How uxmJobs works, how to set it up and what each file holds."
---

uxmJobs pays players for work: breaking, placing, killing, fishing, smelting and more, priced per job in money, experience and points. Every job levels on its own curve. The defaults ship with protections against farms and loops, a soft cap on income, and a report of how much money the plugin created.

## Install

1. Put the jar in `plugins/`.
2. Start the server once. The plugin writes its files into `plugins/uxmJobs/`, including eight example jobs.
3. Check `economy.conf` and the jobs under `jobs/`, then run `/uxmjobs reload`.
4. Run `/uxmjobs preview <job>` to see what a job pays at levels 1, 10, 50 and 100 before anyone plays.

Vault or another economy, PlaceholderAPI and the claim plugins are optional.

## Files

| File | What it holds |
|---|---|
| `config.conf` | storage, slots, employment, ledger, decay, income cap, alerts, tax, server account, claims, zones, leaderboard, signs, report |
| `protections.conf`, `economy.conf`, `display.conf` | protections, the economy and the display, each under its own block name; a block an earlier release kept in `config.conf` moves here on the first start |
| `jobs/<id>.conf` | one job per file; the file name is the id |
| `contracts/<id>.conf` | contracts a player signs for better wages in set hours |
| `boosters.conf` | permission boosters and how two of them combine |
| `schedule.conf` | boosters that run on a calendar |
| `titles.conf` | the title each level carries |
| `shop.conf` | the job shop, where points are spent |
| `editor/new-job.conf`, `menus/*.conf`, `commands.conf`, `messages/`, `theme.conf` | editor template, windows, commands, words, colours |

Each shipped file is commented in full; this page is the map.

## A job

`jobs/miner.conf` shows every key. In short:

| Key | Meaning |
|---|---|
| `name`, `description` | a line of the messages files (`"@content.jobs.miner.name"`), drawn in each reader's language; or `default` plus one line per language tag; or one text everybody reads. The shipped files use the first, with their words under `content` in each messages file |
| `icon` | the material the windows draw |
| `curve` | `shape` (`linear` with `base` and `step`, or `geometric` with `base` and `factor`) and `maximum-level` (up to 10000) |
| `level-cap` | `default`, and a `node` prefix: `uxmjobs.level-cap.miner.150` lets a group climb to 150 |
| `seats` | how many players may hold the job at once, offline holders included; `0` for no limit |
| `currency` | what join and leave costs are taken in: a backend, or `backend:currency` |
| `join-cost`, `leave-cost` | expressions; `leave-cost.nodes` gives cheaper costs to nodes, and the cheapest held wins |
| `bounds.<currency>` | the `floor` and `ceiling` one piece of work may pay |
| `requirements` | condition lines a player must pass to join |
| `actions` | per action type, per target, the expressions for `money`, `experience` and `points` |

A level costs what the curve says for the level before it: `linear` costs `base + step * (level - 1)`, `geometric` costs `base * factor^(level - 1)`. Every price is an arithmetic expression; `%level%` is the job level, and any other `%token%` is a placeholder. A token that answers nothing reads as `0`. A broken expression fails the reload and names the file.

## What a payment goes through

For each piece of work the wage expression is evaluated, multiplied by boosters and other modifiers, and clamped to the job's `bounds`. These rules decide how much of it is paid:

1. **Ledger** (`ledger`): a block a player placed pays nothing when broken, and a spot that already paid inside `retention-days` pays nothing again.
2. **Protections** (`protections`): spawner mobs pay `spawner-fraction`, player-made mobs `artificial-fraction`, hopper-fed workstations `hopper-fed-fraction`; hand feeding pays `hand-fed-bonus`. Creative mode, fertilised crops, generator blocks and vehicles pay nothing by default, and a kill needs `damage-share` of the creature's health. `disabled-worlds` pay nothing.
3. **Decay** (`decay`): after `threshold` repeats of one material in one region, each repeat takes `step` off, down to `floor`, recovering one step per `recovery-minutes`.
4. **Income cap** (`income-cap`): per `window-minutes` and per currency, pay is full up to `cap` and shrinks smoothly above it; a larger `softness` keeps more. `0` is no cap.
5. **Tax** (`tax`, off by default): `rate`, lowered by nodes under `node`; `bypass` pays none.
6. **Server account** (`server-account`, off by default): payments are drawn from a named account, and the player is told when it is empty.

`/jobs why` explains the last payment step by step.

## Boosters, contracts, titles and the shop

- `boosters.conf` maps permission nodes to multipliers and chooses `highest` (only the largest counts) or `summed` (the extra parts add up). `/uxmjobs boost-start <multiplier> <minutes>` starts a server-wide boost that survives a restart. `schedule.conf` holds boosters that run on given `days` between `start` and `end` (a window may cross midnight), for given `jobs` and `currencies`.
- A contract is signed for a `job` at a `cost`; inside its working hours the wage is raised, outside them it is lowered. Players use `/contracts`.
- `titles.conf` gives each level a title, read downwards: a player wears the highest entry at or below their level. The `default` block covers every job, a job's own block is asked first.
- `shop.conf` lists what points buy. Points come only from work and are spent only in the shop.

## Other settings

| Block | Meaning |
|---|---|
| `slots` | how many jobs a player may hold: `default`, raised by `uxmjobs.slots.<n>` |
| `permissions.refresh-seconds` | how often slots and zone access are read from a player's nodes |
| `employment` | `rejoin-cooldown-seconds`, `keep-on-return` (how much archived experience a return keeps, compounded per departure) and `confirm-before-leaving` |
| `progress-alerts` | actions run at chosen percentages of a level; ships empty |
| `economy` | which backends pay money, experience and points |
| `claims` | land claim plugins work is checked against |
| `zones` | work zones drawn with the wand, where money is worth more and blocks regenerate |
| `leaderboard` | the boards and their periods |
| `signs` | job signs players click to join |
| `display` | the action bar line and the progress bar |
| `report` | the money report and the income alerts |

## Commands

Player commands need no permission.

| Command | What it does |
|---|---|
| `/jobs` | open the job board |
| `/jobs mine` | the jobs you work and what they pay |
| `/jobs browse` | every job the server has |
| `/jobs join <job>`, `leave <job>`, `quit <job>` | join, leave, and see what leaving would cost |
| `/jobs stats <job>`, `menu <job>` | one job's level and progress, as lines or as a window |
| `/jobs top [job] [period]`, `board` | the leaderboard |
| `/jobs why` | why the last piece of work paid what it paid |
| `/jobs boosters`, `boosts` | what is boosting you |
| `/jobs points`, `shop`, `buy <entry>` | points and the job shop |
| `/jobs archive` | what is kept of jobs you left |
| `/jobs toggle`, `bar` | the payment line and the progress bar |
| `/contracts`, `mine`, `sign <contract>`, `cancel <contract>`, `shift` | contracts |

| Command | Permission | What it does |
|---|---|---|
| `/uxmjobs preview <job>` | `uxmjobs.admin.preview` | what a job pays at levels 1, 10, 50 and 100 |
| `/uxmjobs report`, `report-menu`, `alerts` | `uxmjobs.admin.report` | money created in the window, and players whose income stands out |
| `/uxmjobs inspect` | `uxmjobs.admin.inspect` | what the block or creature you look at is worth |
| `/uxmjobs experience <player> <job> <amount>` | `uxmjobs.admin.experience` | add experience |
| `/uxmjobs setlevel <player> <job> <level>` | `uxmjobs.admin.setlevel` | set a level |
| `/uxmjobs admin-points <player> <amount>` | `uxmjobs.admin.points` | add points |
| `/uxmjobs reset <player>` | `uxmjobs.admin.reset` | take a player out of every job and off every board |
| `/uxmjobs boost-start <multiplier> <minutes>`, `boost-stop` | `uxmjobs.admin.boost` | a server-wide boost |
| `/uxmjobs zone-wand`, `zone-define <zone>`, `zone-name`, `zone-delete <zone>`, `zone-list`, `zone-requirement <zone> <requirement>`, `zone-requirements-clear <zone>`, `zone-regenerate` | `uxmjobs.admin.zone` | work zones |
| `/uxmjobs sign-update` | `uxmjobs.admin.sign` | write every job sign again |
| `/uxmjobs prune` | `uxmjobs.admin.prune` | drop rows past their retention now |
| `/uxmjobs editor` | `uxmjobs.admin.editor` | the in-game editor |
| `/uxmjobs reload`, `version`, `doctor` | `uxmjobs.admin.reload`, `.version`, `.doctor` | the usual operator commands |

The names and aliases come from `commands.conf`. The [reference](reference.md) lists the default aliases and every permission.

## Placeholders

Per job, written `%uxmjobs_<fact>_<job>%`:

| Fact | Value |
|---|---|
| `level`, `xp`, `xp_next`, `progress` | the level, experience, experience to the next level, and percent through the level |
| `title` | the title of the player's level |
| `joined`, `can_join` | whether the player holds the job, and whether they could join it |
| `workers`, `seats_left` | how many hold it, and seats left |

Others: `%uxmjobs_points%`, `%uxmjobs_prefix%` (the jobs a player holds, for a chat prefix), `%uxmjobs_zone%` (the zone the player stands in), and the `slots`, `booster`, `cap`, `earned`, `contract` and `promotion` families.

The boards use the shared grammar: `%uxmjobs_top_<board>_<period>_<n>_name|value|head%`, `%uxmjobs_rank_<board>_<period>%`, `%uxmjobs_value_<board>_<period>%` and `%uxmjobs_resets_<period>%`, where a board is a job id or `total`. All time ranks by level; a month, week or day ranks by experience earned in it. A job id may hold an underscore: a key is read from its end, so `%uxmjobs_top_deep_miner_weekly_1_name%` is the `deep_miner` board.

## Troubleshooting

- **A reload is refused.** An expression or a key is wrong; the console names the file.
- **A player is paid nothing for a block.** They placed it, it is a generator block, or the ledger already paid for that spot.
- **Pay drops during a long session.** That is decay, or the income cap; `/jobs why` says which.
- **The economy pays nothing.** `/uxmjobs doctor` shows which backend is missing.

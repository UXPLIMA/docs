---
title: Contests
order: 312
description: A contest file, phases from the clock, own or fresh plots, teams, judging, rewards, the boss bar, the archive and the hall of fame.
icon: trophy
---

One contest per file under `contests/`; the file name is the id. Every time is worked out from the clock when somebody
asks, so a restart resumes exactly where it was.

## Phases

| Phase | Means |
|---|---|
| `announced` | Said, and nobody may enter yet; the theme may be hidden |
| `building` | Entries open, people build |
| `locked` | Building is over, judging has not started: no edit, restore or template |
| `judging` | People vote; no entry may be touched |
| `closed` | Over, the result stands |

`/uxmplots phase <contest>` ends the current phase now.

## The file

| Key | Meaning |
|---|---|
| `name`, `theme`, `description` | What players read |
| `hide-theme-until-building` | Keep the theme back until building opens |
| `hide-builder-until-result` | Judge without names |
| `entry-mode` | `own-plot` (a showcase of a plot the entrant holds) or `fresh-plot` (an equal new plot each) |
| `fresh-plots` | `world`, `from` and `to` plot corners, `afterwards`: `return-to-the-pool` or `leave-with-the-entrant` |
| `entry-fee` | `currency`, `amount`; the fees make the pot |
| `capacity` | How many entries; 0 for no limit |
| `schedule` | `announced-at` (`"2026-10-01 18:00"`), `building-hours`, `locked-hours`, `judging-hours`, `repeat` (`once`, `weekly`, `monthly`: 28 days) |
| `judging` | `public-share` and `jury-share` (adding to 1), `must-have-stood-for-seconds`, `jury-permission`, `jury`, `categories` |
| `rewards` | Rungs: `from`, `to`, `money`, `pot-share`, `commands`, `effects` |
| `effects` | One list per phase, run for everybody, with `<contest>`, `<theme>`, `<phase>`, `<entries>` |

The example: "The Example Build Off", a lighthouse, own plot, free, 72 hours building, 12 locked, 48 judging, all public
votes, 1 minute standing before voting.

## Entering and teams

`/contest enter <contest> [team]` enters the plot you stand on (or hands a fresh plot), paying the fee.
`/contest invite` asks a teammate and `/contest join` accepts; everybody behind an entry is paid a place's money, and
the pot share is divided.

## Judging

A voter must stand in an entry for the contest's own time. `/contest tour` and `/contest vote` walk the entries not yet
judged; the [scorecard](../menus/contests.md#scorecard) scores each category. A jury votes by `jury-permission` or the
`jury` list; a jury share with nobody named is refused at load.

## Rewards

Rungs are read in order and the first that holds a place wins. A pot share is worked out from the whole pot and rounded
down; shares that add past the pot are scaled to fit, and the console says so. Commands run with `{player}`.

## The boss bar and vote reward

`contests.conf`:

```hocon
boss-bar {
  enabled = true
  show-to = "in-a-plot-world"
  phases  = ["announced", "building", "judging"]
  colour  = "blue"
  style   = "notched-10"
}
vote-reward {
  per-day     = 10
  time-zone   = ""
  effects     = ["[message] @effect.vote.thanks", "[give-money] diamonds 1", "[sound] ENTITY_EXPERIENCE_ORB_PICKUP 1.0 1.4"]
  when-capped = ["[message] @effect.vote.capped"]
}
```

`show-to` is `everybody`, `entrants` or `in-a-plot-world`. A voter is paid for up to `per-day` votes a day.

## Archive and hall of fame

Winning builds are kept under `archive/` the moment judging closes, and nothing deletes one. `/contest hall` opens the
[hall of fame](../menus/contests.md#hall-of-fame). A click puts the kept build on a viewing plot from
`archive.viewing.plots` for `minutes` (10), then the ground goes back. Leave the list empty and a row takes a player to
where the build stood. A plot somebody owns is never written over.

## Editing in game

`/contest edit <contest>` (`uxmplots.contest.edit`); see [In-game editors](editors.md).

Related: [Contest commands](../commands/contest.md), [Contest files](../config/contest-files.md)

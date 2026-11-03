---
title: "Guide"
order: 10
description: "How uxmEasterEggs works, how to set it up and what each file holds."
---

uxmEasterEggs turns blocks around the map into eggs players hunt for. Each egg pays its type's reward when a player
finds it, milestones pay at set counts, finding every egg pays a completion prize, and leaderboards rank finders
all time, by month, by week and by day. A radar points players at the eggs they have not found.

## Install

Put `uxmEasterEggs-<version>.jar` in `plugins/` and start the server. The first start writes `config.conf`,
`commands.conf`, `theme.conf`, the menus, two egg types under `types/` (`common` and `golden`) and the message
files into `plugins/uxmEasterEggs/`. The shipped rewards pay experience, so they work with nothing else installed.

## Placing eggs

Look at a block and run `/uxmeastereggs create [type]`; the block becomes an egg of that type. Two eggs never share
a block. `/uxmeastereggs editor` opens a menu over every egg, where a click teleports and a shift click removes;
`list`, `show`, `set`, `tp` and `delete` do the same from chat. Deleting an egg also removes every find of it.

## Finding eggs

A player finds an egg by clicking it.

- The first find pays the type's `reward`, runs its `effects` and counts toward the player's total.
- A type with no `cooldown` is found once. A type with a cooldown, such as `"1d"`, can be found again once the wait
  has passed, and pays its reward again, but a repeat never counts twice toward milestones, completion or the boards.
- A milestone pays when the player's count reaches its `at`, once. Finding the last egg on the map pays the
  `completion` prize.
- An egg whose type file was deleted stays on the map, pays nothing and says its type is gone.

## Egg types

Each `types/<name>.conf` is one type; the file name is the type's name.

| Key | Meaning |
|---|---|
| `display-name` | The name in the collection menu. `@key` reads it from `messages/`. |
| `texture` | The head: a skin URL or a base64 texture. Empty shows a plain head. |
| `cooldown` | How long before a player can find the same egg again. Empty: once. |
| `reward.money` | Payments such as `"exp 50"`: `exp`, `levels`, `vault`, `vault-unlocked`, `playerpoints`, `ecobits:<currency>`, `treasury`. |
| `reward.commands` | Console commands. |
| `particle` | The idle particle of an egg the player has not found yet. |
| `effects` | What a find does: `[sound]`, `[message]`, `[subtitle]` and the other action lines. |

## Configuration

| Key | Default | Meaning |
|---|---|---|
| `find.announce-progress` | `true` | Tells the player their running total after each new egg. |
| `radar.default-on` | `false` | Whether a player who never used `/eggs radar` has it on. |
| `radar.radius` | `48` | How far the radar looks, in blocks. |
| `radar.interval-ms` | `3000` | How often the radar runs for everybody. |
| `radar.action-bar` | `true` | Shows the hint on the action bar; `false` sends it to chat. |
| `particles.interval-ms` | `500` | How often idle particles are drawn. Raise it first on a busy server. |
| `particles.radius` | `24` | How far a player sees idle particles. |
| `particles.max-eggs-per-player` | `8` | The most eggs drawn for one player at once. |
| `milestones.<name>` | 5 and 25 | `at`, `money`, `commands` and `broadcast` of each milestone. Two milestones cannot share a count. |
| `completion` | | `money`, `commands` and `broadcast` of the prize for finding every egg. |
| `leaderboard.size` | `10` | How many places each board keeps. |
| `leaderboard.refresh-seconds` | `15` | How often boards are read again. Needs a restart. |
| `leaderboard.periods` | all four | `alltime`, `monthly`, `weekly`, `daily`. |
| `leaderboard.time-zone` | server's | The zone a day starts in, such as `"Europe/Istanbul"`. Servers on one database should agree. |
| `leaderboard.week-starts` | `monday` | The first day of a weekly board. |
| `leaderboard.keep-windows` | `12` | How many ended windows of each period are kept. |
| `menu.collection.hide-missing` | `false` | Hides the head of a type the player has not found. |
| `effects` | | What progress, milestones, completion and the radar switch do. |

A weekly board starts at midnight of `week-starts` in `time-zone`, a monthly one at midnight on the 1st, and a
daily one at midnight.

## Commands

| Command | Permission | What it does |
|---|---|---|
| `/uxmeastereggs` | `uxmeastereggs.admin` | Administer the easter eggs. |
| `/uxmeastereggs reset <player>` | `uxmeastereggs.admin.reset` | Take a player's finds and every board away. |
| `/uxmeastereggs create [type] [block]` | `uxmeastereggs.admin.create` | Put an egg on a block. |
| `/uxmeastereggs set <egg> <piece> <value>` | `uxmeastereggs.admin.create` | Set one piece of one egg. |
| `/uxmeastereggs show <egg>` | `uxmeastereggs.admin.list` | Print every piece of one egg. |
| `/uxmeastereggs delete [egg]` | `uxmeastereggs.admin.delete` | Take an egg off the map. |
| `/uxmeastereggs list` | `uxmeastereggs.admin.list` | Print every egg, with its type and its position. |
| `/uxmeastereggs editor` | `uxmeastereggs.admin.editor` | Open the editor, where a click teleports and a shift click removes. Players only. |
| `/uxmeastereggs tp <egg>` | `uxmeastereggs.admin.tp` | Stand at an egg. Players only. |
| `/uxmeastereggs reload` | `uxmeastereggs.admin.reload` | Read the configuration and the content again. |
| `/uxmeastereggs version` | `uxmeastereggs.admin` | Read the version this server runs. |
| `/uxmeastereggs doctor` | `uxmeastereggs.admin.doctor` | Say how this plugin is, in one line per thing that can be wrong. |
| `/eggs` | `uxmeastereggs.use` | See which eggs you have found. |
| `/eggs radar` |  | Switch the radar on or off. Players only. |
| `/eggs top [board] [period]` |  | Open the leaderboard. |
| `/eggs lang` | `uxmeastereggs.language` | Say what language this player reads, and what else this server has. Players only. |
| `/eggs lang set <language>` | `uxmeastereggs.language` | Read this plugin in one language, whatever the client is set to. Players only. |
| `/eggs lang reset` | `uxmeastereggs.language` | Follow the client again, as a player who never chose anything does. Players only. |
| `/eggs lang set-player <player> <language>` | `uxmeastereggs.admin.language` | Set the language another player reads. |
| `/eggs lang reset-player <player>` | `uxmeastereggs.admin.language` | Clear the language another player chose. |

`/eggs` and its subcommands need `uxmeastereggs.use`, which every player has. All nodes and their defaults are in
[reference.md](reference.md).

## Placeholders

| Placeholder | Value |
|---|---|
| `%uxmeastereggs_found%` | How many eggs this player has found. |
| `%uxmeastereggs_total%` | How many eggs are on the map. |
| `%uxmeastereggs_percent%` | The percentage, rounded down. |
| `%uxmeastereggs_complete%` | `true` or `false`. |
| `%uxmeastereggs_top_<board>_<period>_<n>_name\|value\|head%` | Place `n` of a board. |
| `%uxmeastereggs_rank_<board>_<period>%` | The reader's own place on it. |
| `%uxmeastereggs_value_<board>_<period>%` | The reader's own value on it. |
| `%uxmeastereggs_resets_<period>%` | The time until the period starts again. |
| `%uxmeastereggs_head%` | What a `<head:...>` tag draws the reader from. |

The board is `found`, the period one of `alltime`, `monthly`, `weekly` or `daily`: `%uxmeastereggs_top_found_weekly_1_name%`
is this week's best finder.

## Examples

**A daily egg.** Copy `types/common.conf` to `types/daily.conf` and set `cooldown = "1d"`. Players may find each daily
egg once a day; only the first find counts toward the total.

**A finale for the whole hunt.** Set `completion.commands = ["give %player% dragon_egg 1"]` and keep `broadcast = true`.

**Local midnight.** Set `leaderboard.time-zone = "Europe/Istanbul"` so the daily board turns over at midnight there.

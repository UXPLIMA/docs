---
title: "Guide"
order: 10
description: "How uxmDailyRewards works, how to set it up and what each file holds."
---

uxmDailyRewards gives players one reward a day from a calendar. Each claim moves the player one day along; missing
a day ends the streak and starts the calendar again at day one. Every day can give a rank its own reward.

## Install

Put `uxmDailyRewards-<version>.jar` in `plugins/` and start the server. The first start writes `config.conf`,
`commands.conf`, `theme.conf`, `menus/calendar.conf`, a 28 day calendar under `days/` and the message files into
`plugins/uxmDailyRewards/`. The shipped rewards use vanilla commands and experience, so they work with nothing else
installed. Players open the calendar with `/dailyrewards`. `/uxmdailyrewards reload` reads the files again.

## How a claim works

`claim.window` (24 hours) is how long one claim holds a player off. After the window the next day can be claimed.
`claim.grace` (24 hours) is how much longer the streak survives: a player later than window plus grace starts again
at day one. With the shipped values a player claims once a day and loses the run by missing a whole day. Set
`grace = "0s"` to make the window itself the deadline.

The calendar has `calendar.length` days. After the last day it starts again at day one while the streak keeps
counting, so a 28 day calendar gives day 1 again on the 29th claim of an unbroken run. A day with no file gives
nothing and says so in the console; the player's progress does not move. A claim that cannot be saved gives nothing.

In the calendar window a day is claimed, claimable, waiting (the next day, before the window has passed) or locked.

## Day files

`days/<number>.conf` holds one day:

| Key | Meaning |
|---|---|
| `icon` | `material`, `model-data` and `glow` of the day in the window. |
| `reward` | What everybody gets: one `[verb] payload` line per action. |
| `ranks.<rank>.reward` | What a rank gets instead. It replaces the default, so repeat the lines it should keep. |

The usual verbs are `[console]`, `[player]`, `[give-money] <economy> <amount>`, `[message]`, `[sound]`, `[title]`
and `[particle]`. `[give-money]` takes `exp`, `levels`, `vault`, `vault-unlocked`, `playerpoints`,
`ecobits:<currency>` or `treasury`. `%player%`, `%day%` and `%streak%` are filled in before a line runs. The name and
the lore of a day are in `messages/`, so they can be translated.

## Ranks

| Key | Default | Meaning |
|---|---|---|
| `ranks.source` | `"auto"` | `luckperms` for LuckPerms groups, `permission` for nodes, `auto` for LuckPerms when it is installed. |
| `ranks.permission-prefix` | `"uxmdailyrewards.rank."` | On the permission path, `uxmdailyrewards.rank.vip` grants the `vip` rank. |
| `ranks.ladder` | `["default", "vip", "mvp"]` | The ranks that count, worst first. The last one a player holds wins; other groups are ignored. |

## Other settings

| Key | Default | Meaning |
|---|---|---|
| `reminder.enabled` | `true` | Reminds a player with a reward waiting, shortly after they join. |
| `reminder.delay` | `"5s"` | How long after the join. |
| `reminder.effects` | a subtitle, an action bar and a sound | What the reminder does. |
| `celebration.actions` | a sound and particles | Run after every claim, on top of the day's reward. |
| `economy.treasury-wait-ms` | `250` | How long a Treasury payment may take. |
| `storage` | SQLite | Where progress is kept: `sqlite`, `mysql`, `mariadb` or `postgresql`. |

## Commands

| Command | Permission | What it does |
|---|---|---|
| `/uxmdailyrewards` |  | Administer the plugin. |
| `/uxmdailyrewards reload` | `uxmdailyrewards.admin.reload` | Read the configuration and the content again. |
| `/uxmdailyrewards version` | `uxmdailyrewards.admin.version` | Read the version this server runs. |
| `/uxmdailyrewards reset <player>` | `uxmdailyrewards.admin.reset` | Put a player back to nothing claimed. |
| `/uxmdailyrewards grant <player> <day>` | `uxmdailyrewards.admin.grant` | Give a player one day of the calendar without moving their progress. |
| `/uxmdailyrewards doctor` | `uxmdailyrewards.admin.doctor` | Say how this plugin is, in one line per thing that can be wrong. |
| `/dailyrewards` | `uxmdailyrewards.use` | Open the daily reward calendar. |
| `/dailyrewards claim` |  | Take today's reward without opening anything. Players only. |
| `/dailyrewards lang` | `uxmdailyrewards.language` | Say what language this player reads, and what else this server has. Players only. |
| `/dailyrewards lang set <language>` | `uxmdailyrewards.language` | Read this plugin in one language, whatever the client is set to. Players only. |
| `/dailyrewards lang reset` | `uxmdailyrewards.language` | Follow the client again, as a player who never chose anything does. Players only. |
| `/dailyrewards lang set-player <player> <language>` | `uxmdailyrewards.admin.language` | Set the language another player reads. |
| `/dailyrewards lang reset-player <player>` | `uxmdailyrewards.admin.language` | Clear the language another player chose. |

`/uxmdailyrewards grant` gives a day's reward without moving the player's progress; `reset` puts the player back to
nothing claimed. All nodes and their defaults are in [reference.md](reference.md).

## Placeholders

| Placeholder | Value |
|---|---|
| `%uxmdailyrewards_streak%` | Days claimed in a row, 0 when the run has lapsed. |
| `%uxmdailyrewards_day%` | The day that is waiting, or the last one taken. |
| `%uxmdailyrewards_available%` | `true` or `false`. |
| `%uxmdailyrewards_time_left%` | Such as `6h 12m`, empty when a claim is due. |
| `%uxmdailyrewards_seconds_left%` | The same span in whole seconds. |

## Examples

**A weekly calendar.** Set `calendar.length = 7` and keep `days/1.conf` to `days/7.conf`.

**Forgiving streaks.** Set `claim.grace = "48h"`: a player may miss a day and keep the run.

**A better reward for donors.** Add `ranks { vip { reward = ["[give-money] vault 500"] } }` to a day, and keep `vip`
in `ranks.ladder`.

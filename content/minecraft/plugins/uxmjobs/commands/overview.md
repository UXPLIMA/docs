---
title: Overview
order: 201
description: The three commands at a glance, renaming them, and who sees what.
icon: list
---

| Command | Aliases | Subcommands |
|---|---|---|
| `/jobs` | `/job`, `/j` | `mine`, `browse` (`list`), `menu`, `board`, `boosts`, `join`, `leave`, `quit` (`confirm`), `stats` (`info`), `points`, `top` (`leaderboard`), `why`, `boosters` (`boost`), `toggle`, `bar`, `archive`, `shop`, `buy` |
| `/contracts` | `/contract` | `menu`, `mine`, `sign`, `cancel`, `shift` |
| `/uxmjobs` | `/jobsadmin` | `editor`, `reload`, `doctor`, `version`, `experience` (`xp`), `setlevel`, `points`, `reset`, `boost start`, `boost stop`, `preview`, `report`, `report menu`, `alerts`, `prune`, `inspect`, `sign update`, `zone wand`, `zone define`, `zone name`, `zone delete`, `zone list`, `zone requirement`, `zone requirements clear`, `zone regenerate`, `lang`, `lang set`, `lang reset`, `lang set-player`, `lang reset-player` |

Subcommand aliases are in brackets. Names and aliases are the defaults from [commands.conf](../config/commands-conf.md);
a change needs a restart.

## Who sees what

`/jobs` and `/contracts` need no node: every player has them. Every `/uxmjobs` subcommand hides behind its
`uxmjobs.admin` node, except `lang`, `lang set` and `lang reset`, which carry none: a player's own language is set
under `/uxmjobs`, so every player sees that command with those three words in it.

## From the console

`top`, every operator subcommand except `editor`, `report menu`, `inspect`, `zone wand`, `zone define` and `zone name`,
and the two `lang ...-player` words run from the console. Everything else needs a player.

Related: [Player commands](player.md), [Admin commands](admin.md)

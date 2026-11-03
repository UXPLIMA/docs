---
title: "Reference"
order: 20
description: "Every command, permission, placeholder and file of uxmJobs."
---

## Commands

Names and aliases are the defaults from `commands.conf`, which renames or disables each of them.

| Command | Aliases | Subcommands |
|---|---|---|
| `/jobs` | `/job`, `/j` | `mine`, `browse`, `menu`, `board`, `boosts`, `join`, `leave`, `quit`, `stats`, `points`, `top`, `why`, `boosters`, `toggle`, `bar`, `archive`, `shop`, `buy` |
| `/contracts` | `/contract` | `menu`, `mine`, `sign`, `cancel`, `shift` |
| `/uxmjobs` | `/jobsadmin` | `editor`, `reload`, `doctor`, `version`, `experience`, `setlevel`, `points`, `reset`, `boost start`, `boost stop`, `preview`, `report`, `report menu`, `alerts`, `prune`, `inspect`, `sign update`, `zone wand`, `zone define`, `zone name`, `zone delete`, `zone list`, `zone requirement`, `zone requirements clear`, `zone regenerate`, `lang`, `lang set`, `lang reset`, `lang set-player`, `lang reset-player` |

## Permissions

| Permission | What it allows | Default |
|---|---|---|
| `uxmjobs.tax.bypass` | Be paid without the tax the server takes | op |
| `uxmjobs.admin.*` | Everything an operator does with this plugin | op |
| `uxmjobs.admin.doctor` | Ask how this plugin is | op |
| `uxmjobs.admin.version` | Read the version this server runs | op |
| `uxmjobs.admin.reload` | Read the configuration and the content again | op |
| `uxmjobs.admin.language` | Set or clear the language another player reads | op |
| `uxmjobs.admin.editor` | Build and change jobs from inside the game | op |
| `uxmjobs.admin.experience` | Give, take and set a player's experience in a job | op |
| `uxmjobs.admin.setlevel` | Set a player's level in a job | op |
| `uxmjobs.admin.points` | Give, take and set a player's points | op |
| `uxmjobs.admin.reset` | Take a player out of every job and off every board | op |
| `uxmjobs.admin.boost` | Start and clear a payment boost | op |
| `uxmjobs.admin.inspect` | Read what a target is worth, and to whom | op |
| `uxmjobs.admin.preview` | See what a row would pay before it does | op |
| `uxmjobs.admin.report` | Read what this server has paid out | op |
| `uxmjobs.admin.prune` | Clear rows this server no longer needs | op |
| `uxmjobs.admin.zone` | Draw and change the zones work is priced in | op |
| `uxmjobs.admin.sign` | Put up and take down a job sign | op |

## Placeholders

The plugin registers a PlaceholderAPI expansion when PlaceholderAPI is installed.

## Files

The plugin writes these into `plugins/uxmJobs/` on first start and reads them again on reload:

- `boosters.conf`
- `commands.conf`
- `config.conf`
- `contracts/evening-shift.conf`
- `contracts/weekend-crew.conf`
- `display.conf`
- `economy.conf`
- `editor/new-job.conf`
- `jobs/builder.conf`
- `jobs/enchanter.conf`
- `jobs/farmer.conf`
- `jobs/fisherman.conf`
- `jobs/hunter.conf`
- `jobs/miner.conf`
- `jobs/smith.conf`
- `jobs/woodcutter.conf`
- `menus/boosters.conf`
- `menus/contract.conf`
- `menus/contracts.conf`
- `menus/editor-job.conf`
- `menus/editor-list.conf`
- `menus/editor.conf`
- `menus/job.conf`
- `menus/jobs.conf`
- `menus/levels.conf`
- `menus/quit.conf`
- `menus/report.conf`
- `menus/shop.conf`
- `menus/top.conf`
- `messages/messages_en.conf`
- `messages/messages_tr.conf`
- `protections.conf`
- `schedule.conf`
- `shop.conf`
- `theme.conf`
- `titles.conf`

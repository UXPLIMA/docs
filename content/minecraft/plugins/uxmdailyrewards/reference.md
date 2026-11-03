---
title: "Reference"
order: 20
description: "Every command, permission, placeholder and file of uxmDailyRewards."
---

## Commands

Names and aliases are the defaults from `commands.conf`, which renames or disables each of them.

| Command | Aliases | Subcommands |
|---|---|---|
| `/dailyrewards` | `/daily`, `/rewards` | `claim`, `lang`, `lang set`, `lang reset`, `lang set-player`, `lang reset-player` |
| `/uxmdailyrewards` | `/uxmdr`, `/dailyrewardsadmin` | `reload`, `version`, `reset`, `grant`, `doctor` |

## Permissions

| Permission | What it allows | Default |
|---|---|---|
| `uxmdailyrewards.language` | Read this plugin in a language of your own. | true |
| `uxmdailyrewards.admin.language` | Set or clear the language another player reads | op |
| `uxmdailyrewards.use` | Open the daily rewards window and claim what is yours | true |
| `uxmdailyrewards.rank.*` | Every reward rank an operator writes, including the ones added later | op |
| `uxmdailyrewards.admin.*` | Everything an operator does with this plugin | op |
| `uxmdailyrewards.admin.grant` | Give a player a claim they have not earned | op |
| `uxmdailyrewards.admin.reset` | Put a player's streak back to the beginning | op |
| `uxmdailyrewards.admin.doctor` | Ask how this plugin is | op |
| `uxmdailyrewards.admin.version` | Read the version this server runs | op |
| `uxmdailyrewards.admin.reload` | Read the configuration and the days again | op |

## Placeholders

The plugin registers a PlaceholderAPI expansion when PlaceholderAPI is installed.

## Files

The plugin writes these into `plugins/uxmDailyRewards/` on first start and reads them again on reload:

- `commands.conf`
- `config.conf`
- `days/1.conf`
- `days/10.conf`
- `days/11.conf`
- `days/12.conf`
- `days/13.conf`
- `days/14.conf`
- `days/15.conf`
- `days/16.conf`
- `days/17.conf`
- `days/18.conf`
- `days/19.conf`
- `days/2.conf`
- `days/20.conf`
- `days/21.conf`
- `days/22.conf`
- `days/23.conf`
- `days/24.conf`
- `days/25.conf`
- `days/26.conf`
- `days/27.conf`
- `days/28.conf`
- `days/3.conf`
- `days/4.conf`
- `days/5.conf`
- `days/6.conf`
- `days/7.conf`
- `days/8.conf`
- `days/9.conf`
- `menus/calendar.conf`
- `messages/messages_en.conf`
- `messages/messages_tr.conf`
- `theme.conf`

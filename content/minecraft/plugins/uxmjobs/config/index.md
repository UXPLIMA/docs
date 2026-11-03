---
title: Configuration
order: 150
description: Every file the plugin writes, reloading, and what an update does to your files.
icon: settings
---

| Page | What it holds |
|---|---|
| [config.conf](config-conf.md) | Language, storage, slots, permissions, employment, ledger, decay, income cap, milestones, tax, server account, claims, zones, leaderboard, signs, report, explore, placeholder actions, editor, menu sounds |
| [protections.conf](protections-conf.md) | Farm grades and what never pays |
| [economy.conf](economy-conf.md) | What costs are taken in |
| [display.conf](display-conf.md) | What a player sees while working |
| [Job files](../features/jobs-and-prices.md) | `jobs/<id>.conf` |
| [Contract files](contract-files.md) | `contracts/<id>.conf` |
| [Zone files](zone-files.md) | `zones/<id>.conf` |
| [boosters.conf](boosters-conf.md) | Permission boosters |
| [schedule.conf](schedule-conf.md) | Calendar boosters |
| [titles.conf](titles-conf.md) | The title ladder |
| [shop.conf](shop-conf.md) | The job shop |
| [commands.conf](commands-conf.md) | The name, aliases and switch of every command |
| [Menu files](menu-files.md) | The thirteen windows |
| [Messages](messages.md) | `messages/messages_<language>.conf` |
| [Languages](languages.md) | The seven languages and how a player's language is chosen |
| [theme.conf](theme.md) | The colours, glyphs and small capitals every UXM plugin shares |

Every file is HOCON: `key = value`, blocks in braces, `#` comments.

## Reloading

`/uxmjobs reload` reads every file again and swaps the result in as one snapshot, so a reload is never half applied.
A file that is wrong refuses the reload and the server keeps the last good snapshot; the console names the file and the
key. `storage`, `editor.enabled` and `commands.conf` need a restart.

## What an update does

When a release adds a key, the next start adds it to `config.conf`, `protections.conf`, `economy.conf` or
`display.conf` with its comment, and never changes a value you wrote. Your file as it was is kept in `backups/`; the
last five copies are kept. Alignment may move, and a key you deleted comes back.

Related: [config.conf](config-conf.md)

---
title: Configuration
order: 150
description: Every file the plugin writes, which ones the proxy reads, reloading, and what an update does.
icon: settings
---

| Page | What it holds |
|---|---|
| [config.conf](config-conf.md) | Language, storage, names, passwords, hashing, lane, hold, location, notices, purge, account, Bedrock, timeouts, premium, sessions, second factor, mail, email, history, staff, alerts, placeholders, Discord, website table, import |
| [protection.conf](protection-conf.md) | Shared addresses, bans, limits, countries, captcha |
| [ownership.conf](ownership-conf.md) | Rules, schemes, conflicts |
| [network.conf](network-conf.md) | The network |
| [effects.conf](effects-conf.md) | What happens at each moment |
| [screens.conf](screens-conf.md) | The waiting room and every dialog |
| [commands.conf](commands-conf.md) | Names, aliases, switches, Turkish words |
| [Import files](import-files.md) | `imports/<source>.conf` |
| [Mail files](mail-files.md) | `mail/<kind>_<language>.txt` |
| [Messages](messages.md) | `messages/messages_<language>.conf` |
| [Languages](languages.md) | The seven languages |
| [theme.conf](theme.md) | The colours, glyphs and small capitals every UXM plugin shares |

The proxy reads `config.conf`, `ownership.conf`, `network.conf`, and `commands.conf` for `/login` and `/register`.

## Reloading

`/uxmauth reload` reads the files again and swaps them in as one snapshot; a broken edit leaves the running configuration
as it was. The `hashing` block, `lane.threads` and `max-threads`, `mail.threads`, `website-table`, the storage URL and
`commands.conf` need a restart.

## What an update does

When a release adds a key, the next start adds it to your file with the comment that explains it, and it never changes a
value you wrote. Your file as it was is kept in `backups/`; the last five copies are kept.

Related: [config.conf](config-conf.md)

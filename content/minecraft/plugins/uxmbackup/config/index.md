---
title: Configuration
order: 150
description: Every file the plugin writes, environment values, reloading, and what an update does.
icon: settings
---

| Page | What it holds |
|---|---|
| [config.conf](config-conf.md) | Every setting, block by block |
| [commands.conf](commands-conf.md) | The name, aliases and switch of every command |
| [Menu files](menu-files.md) | `menus/archives.conf` |
| [Messages](messages.md) | Every line the plugin writes |
| [Languages](languages.md) | The seven languages and how a player's language is chosen |
| [theme.conf](theme.md) | The colours, glyphs and small capitals every UXM plugin shares |

## Environment values

Any value may be written as `${NAME}` and is filled from the process environment. That keeps a password out of a file
that gets copied:

```hocon
password = ${BACKUP_SFTP_PASSWORD}
```

## Reloading

`/uxmbackup reload` reads the files again and swaps them in whole. A file that does not parse keeps the running
settings and the console says why. `storage` and `commands.conf` need a restart.

## What an update does

A key a release adds is written into your file with its comment, at its default, and a value you wrote is never
changed. The sets and the storages are yours: an entry you removed is not written back. The file as it was is kept in
`backups/`, the last five copies.

Related: [config.conf](config-conf.md)

---
title: Installation
order: 102
description: Requirements, the first start, the files that appear, and checking the install.
icon: download
---

uxmBackup is one jar with uxmLib inside it. This manual describes uxmBackup after 0.1.0: German, Spanish, French,
Portuguese and Russian arrive with the next release, and 0.1.0 itself reads English and Turkish.

## Requirements

| Requirement | Detail |
|---|---|
| Java | 25 or newer |
| Server | Paper 26.2 or newer, or Folia; any fork of Paper |
| Disk | Room for one archive in `work/` beside the server, above `free-space-floor-mb` |
| Database | none: the run history is in SQLite by default |

No external program is needed: databases are dumped without `mysqldump` or `pg_dump`.

## Installing

1. Put `uxmBackup-<version>.jar` in `plugins/` and start the server.
2. The first start writes `config.conf`, `commands.conf`, `menus/archives.conf` and the message files into
   `plugins/uxmBackup/`.
3. Out of the box the `full` set runs every night at 04:00 and the `light` set every six hours, both to the `local`
   storage in `plugins/uxmBackup/archives`.
4. Run `/uxmbackup doctor` to see that everything is in order, and `/uxmbackup run light` for a first archive.

## The files

```
plugins/uxmBackup/
├── config.conf       language, effects, storage, sets, storages, restore, shutdown, discord, window sounds
├── commands.conf     the name, aliases and switch of every command and subcommand
├── menus/archives.conf the archives window
├── messages/         messages_<language>.conf: en, tr, de, es, fr, pt, ru
├── languages.conf    the language each player chose, when uxmLang is not installed
├── archives/         the local storage's archives
├── work/             where an archive is built before it is handed to a storage
├── restores.log      every restore and extraction, who ran it and what it touched
├── data/             backup.db, the run history, while storage.type is sqlite
└── backups/          the last five copies of each file a release rewrote
```

## Checking the install

`/uxmbackup doctor` (permission `uxmbackup.doctor`):

| Check | ok | fail or warn |
|---|---|---|
| `storage` | The backend, its place, the prefix and the schema version | fail: no run history can be read or written |
| `content` | How many storages were read | fail: no storage, so an archive has nowhere to go |
| `windows` | How many window files were read | fail: the archives window cannot open |
| `placeholders` | `published` | warn: PlaceholderAPI is not on the server |

`/uxmbackup storages` asks every storage whether it answers.

## Metrics

The plugin sends anonymous usage counts to bStats (plugin id 34236). Turn it off for every plugin in
`plugins/bStats/config.yml`.

Related: [Concepts](concepts.md), [config.conf](../config/config-conf.md)

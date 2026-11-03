---
title: Welcome
order: 101
description: What uxmBackup does, who it is for, and where to read next.
icon: hand
---

uxmBackup takes backups from inside the server: worlds, files and databases go into one zip archive with a manifest
of every file and its checksum, and the archive goes to one or more storages. Each backup is a named set with its own
schedule, and every storage keeps its own retention rules.

| Part | What it gives you |
|---|---|
| Sets | Named backups, each with its worlds, paths, databases, excludes, storages and schedule |
| Schedules | Five-field cron or a period such as `6h` |
| Storages | Local folders, SFTP, any S3 endpoint, WebDAV and FTPS, as many as you like |
| Retention | Per storage: a count, a size and an age, and the newest archive is never removed |
| Databases | SQLite, MySQL, MariaDB and PostgreSQL dumped by their own engine while the server runs |
| Verifying | Every archive read back against its manifest |
| Restoring | Three levels, from a dry run to an in-place restore behind five protections |
| Alerts | Action lines and a Discord webhook when a run ends |

## Read next

- [Installation](installation.md) and [Concepts](concepts.md).
- [Your first backup](first-backup.md): a nightly copy to another machine.
- [Restoring](../features/restoring.md) before you need it.

Related: [Commands](../commands/overview.md), [Configuration](../config/index.md)

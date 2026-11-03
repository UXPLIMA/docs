---
title: Databases
order: 305
description: Dumping SQLite, MySQL, MariaDB and PostgreSQL databases while the server runs.
icon: database
---

A database is dumped by its own engine and never copied as a file: an open SQLite database holds rows in its `-wal`
that the `.db` file does not have yet, so a file copy is missing the last minutes. Each engine is dumped with the one
statement or transaction that answers consistently while it runs. No program on the path is needed.

```hocon
sets {
  full {
    databases {
      luckperms {
        jdbc-url = "jdbc:mysql://127.0.0.1:3306/luckperms"
        username = "minecraft"
        password = ${LUCKPERMS_PASSWORD}
      }
    }
  }
}
```

| Key | Meaning |
|---|---|
| `jdbc-url` | The connection: `jdbc:sqlite:`, `jdbc:mysql:`, `jdbc:mariadb:` or `jdbc:postgresql:` |
| `username`, `password` | The login, for a database server |
| `command` | A vendor tool to use instead, such as `mysqldump -h 127.0.0.1 -u minecraft luckperms` |

Name the connection of any plugin's data here, ours or anybody's.

## Vendor tools

`databases.allow-external-commands = false` by default, because a plugin that needs a program on the path does not work
on Windows or in a slim container. Turn it on to let a source's `command` run; it may take
`databases.external-timeout-seconds` (600).

## Restoring a dump

A dump in an archive is written to `restore.database-folder` (`uxmbackup-restore`) and never applied: a dump written
into a database another plugin holds open would replace rows underneath a running server. Import it yourself.

Related: [Restoring](restoring.md), [Sets and schedules](sets-and-schedules.md)

---
title: SQLite
order: 902
description: The default backend, where the file lives, and backing it up.
icon: file
---

SQLite is a database in one file, with nothing to install. It is the default and right for a single server.

```hocon
storage {
  type = "sqlite"
  file = "plots.db"
}
```

| Key | Default | Meaning |
|---|---|---|
| `file` | `"plots.db"` | The file name, inside `plugins/uxmPlots/data/` |
| `table-prefix` | `"uxmplots_"` | The prefix of every table |

The file is `plugins/uxmPlots/data/plots.db`, with `plots.db-wal` and `plots.db-shm` beside it while the server
runs. A file an older version left in the plugin folder is moved into `data/` on the first start.

SQLite takes one writer, so the pool uses one connection whatever `pool-size` says.
One file belongs to one server.

## Backing up

Stop the server, then copy `data/plots.db` and, if present, the `-wal` and `-shm` files. A copy taken while the server
runs may miss the last writes. uxmBackup can archive the folder on a schedule.

Related: [Overview](overview.md), [MySQL and MariaDB](mysql.md)

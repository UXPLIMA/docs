---
title: SQLite
order: 902
description: One file, nothing to set up.
icon: file
---

The shipped backend. The database is one file under `plugins/uxmSkyblock/data/`, `skyblock.db` as shipped.

```hocon
storage {
    type = "sqlite"
    file = "skyblock.db"
}
```

The plugin opens SQLite with synchronous FULL and foreign keys on, so a commit a player saw land survives a power loss
and deleting an island removes everything under it. One server only: servers that share islands need a database
server.

To back the file up by hand, stop the server first, or use `/is admin backup database` while it runs.

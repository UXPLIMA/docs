---
title: PostgreSQL
order: 904
description: Connecting to PostgreSQL to share language choices across servers.
icon: server
---

```hocon
storage {
  type     = "postgresql"
  host     = "db.example.net"
  port     = 0                # 0 is 5432
  database = "minecraft"
  username = "lang"
  password = "secret"
}
```

| Key | Meaning |
|---|---|
| `type` | `postgresql`. The driver is downloaded on the first start |
| `host`, `port` | The database server; port `0` is 5432 |
| `database` | The database, which must exist |
| `username`, `password` | A user that may create tables in the database's default schema |

Everything else, the prefix and the pool, reads as for [MySQL and MariaDB](mysql.md). To bring an existing
SQLite file across, see [Moving off SQLite](overview.md#moving-off-sqlite).

Related: [Overview](overview.md)

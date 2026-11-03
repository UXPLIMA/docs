---
title: MySQL and MariaDB
order: 903
description: Connecting to MySQL or MariaDB to share one house across servers.
icon: server
---

```hocon
storage {
  type     = "mysql"          # or "mariadb"
  host     = "db.example.net"
  port     = 0                # 0 is 3306
  database = "minecraft"
  username = "auction"
  password = "secret"
  table-prefix = "auction_"
  pool-size = 10
}
```

| Key | Meaning |
|---|---|
| `type` | `mysql` or `mariadb`. The MariaDB driver is downloaded on the first start |
| `host`, `port` | The database server; port `0` is 3306 |
| `database` | The database, which must exist |
| `username`, `password` | A user with rights to create tables in it |
| `pool-size` | Connections the pool may open, 10 by default |

The user needs `CREATE`, `ALTER`, `INDEX`, `SELECT`, `INSERT`, `UPDATE` and `DELETE` on the database. The tables are
created at the first start. To bring an existing SQLite file across, see
[Moving off SQLite](overview.md#moving-off-sqlite).

Related: [Overview](overview.md), [PostgreSQL](postgresql.md)

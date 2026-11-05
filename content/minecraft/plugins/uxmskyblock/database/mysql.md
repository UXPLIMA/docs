---
title: MySQL and MariaDB
order: 903
description: A database server, for one server or several.
icon: server
---

```hocon
storage {
    type = "mariadb"          # or "mysql"
    host = "localhost"
    port = 0                  # 3306
    database = "minecraft"
    username = "minecraft"
    password = ""
    pool-size = 10
    lock-timeout = "1s"
}
```

Make the database and the user first; the plugin makes its tables. The MariaDB driver is fetched into the server's
libraries folder the first time the plugin loads.

MariaDB and MySQL count `lock-timeout` in whole seconds. A server that dies holding a row keeps it until the database
notices; without the timeout everything behind it would wait fifty seconds on MariaDB.

For a commit to survive a power loss, keep `innodb_flush_log_at_trx_commit = 1`, and `sync_binlog = 1` when the binary
log is on. See [Durability](overview.md#durability).

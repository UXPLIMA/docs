---
title: PostgreSQL
order: 904
description: A database server, for one server or several.
icon: server
---

```hocon
storage {
    type = "postgresql"
    host = "localhost"
    port = 0                  # 5432
    database = "minecraft"
    username = "minecraft"
    password = ""
    pool-size = 10
    lock-timeout = "1s"
    idle-transaction-timeout = "5s"
}
```

Make the database and the user first; the plugin makes its tables. The PostgreSQL driver is fetched into the server's
libraries folder the first time the plugin loads.

`idle-transaction-timeout` ends a transaction that stays open with nothing happening in it and gives its locks back,
so a server that dies mid transaction does not hold rows for ever. Keep `fsync` and `synchronous_commit` on; see
[Durability](overview.md#durability).

---
title: Backends
order: 902
description: The JDBC URLs uxmAuth takes, and what each needs.
icon: server
---

`storage.jdbc-url` names the database. Empty is the SQLite file `plugins/uxmAuth/data/auth.db`. The jar carries every
driver.

| Backend | URL |
|---|---|
| SQLite | `jdbc:sqlite:plugins/uxmAuth/data/auth.db` |
| MySQL | `jdbc:mysql://127.0.0.1:3306/uxm?allowPublicKeyRetrieval=true` |
| MariaDB | `jdbc:mariadb://127.0.0.1:3306/uxm` |
| PostgreSQL | `jdbc:postgresql://127.0.0.1:5432/uxm` |
| H2 | `jdbc:h2:./plugins/uxmAuth/auth-h2` |

MySQL 8 without TLS wants `allowPublicKeyRetrieval=true` to log in with a password. `username` and `password` sit beside
the URL; `pool-size` (4) is how many connections the pool may open. SQLite has one writer, so more than a few buys nothing
there; a shared database wants a few per busy server.

The tables are made at the first start. A change of URL needs a restart.

Related: [Overview](overview.md)

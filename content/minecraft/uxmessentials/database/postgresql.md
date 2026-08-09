---
title: PostgreSQL
order: 1580
---

**PostgreSQL** is the third first-class backend. It behaves exactly like the
[MySQL / MariaDB](mysql.md) path — same tables, same migrations, same features — and differs
only in the driver and the default port. Reach for it if Postgres is what you already run and
operate well.

---

## Why PostgreSQL?

| Pros                              | Cons                                 |
|-----------------------------------|--------------------------------------|
| ✅ Excellent performance at scale  | ❌ Less common in Minecraft hosting   |
| ✅ Strong data integrity           | ❌ More setup than SQLite             |
| ✅ Real row-level locking          | ❌ Needs its own backup strategy      |
| ✅ Required-grade for cross-server | |

**Recommended for:** operators already comfortable with PostgreSQL, and larger networks.

---

## Requirements

- PostgreSQL **13** or newer.
- A database and a user the plugin can connect with.

---

## Step 1: Create the Database

Connect to PostgreSQL and run:

```sql
CREATE DATABASE uxmessentials;
CREATE USER uxmessentials WITH ENCRYPTED PASSWORD 'your_secure_password';
GRANT ALL PRIVILEGES ON DATABASE uxmessentials TO uxmessentials;
```

Then grant schema permissions so the plugin can build its tables:

```sql
\c uxmessentials
GRANT ALL ON SCHEMA public TO uxmessentials;
```

---

## Step 2: Configure the Plugin

Edit the `storage` block in `plugins/uxmEssentials/config.conf`:

```hocon
storage {
  backend = "postgres"              # sqlite | mysql | postgres
  host = "localhost"
  port = 5432                       # default 5432 for PostgreSQL
  database = "uxmessentials"
  username = "uxmessentials"
  password = "your_secure_password"
  read-pool-size = 8
  connection-timeout-ms = 5000
}
```

The only differences from the [MySQL setup](mysql.md) are `backend = "postgres"` and the default
`port = 5432`. As with MySQL, uxmEssentials builds the JDBC connection for you from `host`,
`port` and `database` — there is no raw URL to enter. Pool sizing (`read-pool-size`,
`connection-timeout-ms`) works identically; see [Connection Pool Sizing](mysql.md#connection-pool-sizing).

---

## Step 3: Restart and Verify

Restart the server. On enable, **Flyway** applies the schema migrations automatically. Confirm
the connection with:

```
/uxmess doctor
```

---

## Troubleshooting

| Symptom | Likely cause |
|---------|--------------|
| `password authentication failed` | Wrong credentials, or `pg_hba.conf` disallows the connection method |
| `Connection refused` | PostgreSQL not running, wrong port, or `postgresql.conf` not listening on the network |
| `permission denied for schema public` | Re-run the `GRANT ALL ON SCHEMA public` from Step 1 |

---

## Backing Up

```bash
pg_dump -U uxmessentials uxmessentials > backup.sql
```

Restore with:

```bash
psql -U uxmessentials uxmessentials \< backup.sql
```

<Callout type="warning" title="Back up before switching backends">

Changing `backend` starts a **fresh, empty schema** on the new database — it does not copy
your old data across. Back up your current database first (SQLite file or a `pg_dump`) before
you change the value.

</Callout>

---

## Next Steps

- [MySQL / MariaDB Setup](mysql.md) — the sibling network backend, with pool-sizing details.
- [Cross-Server](../cross-server/overview.md) — share this database across a network of backends.
- [config.conf (Globals)](../config/global-config.md) — every global setting alongside `storage`.

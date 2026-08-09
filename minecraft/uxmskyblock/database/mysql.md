---
title: MySQL
order: 810
---

**MySQL** stores island data in a shared database. Use it when you run **multiple
backend servers** behind a proxy, or simply want centralized, externally-backed
storage. It's required for the [cross-server proxy layer](../proxy/cross-server.md).

---

## Configuration

```yaml
storage:
  type: mysql
  mysql:
    host: localhost
    port: 3306
    database: skyblock
    username: root
    password: ""
    ssl: false
```

| Key | Meaning |
|-----|---------|
| `host` / `port` | Where your MySQL server is |
| `database` | The database name (create it first) |
| `username` / `password` | Credentials |
| `ssl` | Use an SSL connection |

The plugin creates and migrates its tables automatically on first connect.

---

## Setup Steps

1. Create a database:
   ```sql
   CREATE DATABASE skyblock;
   ```
2. Set `storage.type: mysql` and fill in the `mysql:` block.
3. Restart the server. Watch the console for a successful connection.

---

## Using It Across a Network

For multiple backends, **every server** must point at the **same** MySQL database
with identical `host`/`database`. Combined with Redis, this is what powers the
proxy layer.

<Callout type="warning" title="Stamp the server column before enabling the proxy">

If you're migrating from a single server, give existing islands a server name
before turning the proxy on:
```sql
UPDATE islands SET server = 'skyblock-1' WHERE server IS NULL;
```

</Callout>

See [Cross-Server Setup](../proxy/cross-server.md) for the full network guide.

---

## Troubleshooting

| Symptom | Likely cause |
|---------|--------------|
| "Connection failed" on start | Wrong host/port/credentials, or DB doesn't exist |
| Tables missing | The plugin couldn't connect — fix the connection first |
| Islands not syncing across servers | Servers point at different databases, or Redis is down |

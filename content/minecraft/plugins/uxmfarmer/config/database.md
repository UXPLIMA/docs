---
title: Database and multi-server
order: 55
icon: database
---

## Choosing a driver

```yaml
database:
  driver: "SQLITE"
  host: "localhost"
  database: "database"
  username: ""
  password: ""
  port: 3306
  use-ssl: false
  use-extra-flags: false
```

SQLite is the default and is correct for a single server: the file lives in the plugin folder and
needs no setup.

MySQL is required the moment more than one server needs to see the same farmers. Set `driver` to
`MYSQL` and fill in the connection details.

`use-extra-flags` should stay `false` unless UXPLIMA support tells you otherwise — it changes the
JDBC connection string, and the default string is the one that has been tested.

## Multi-server

```yaml
multi-server:
  enabled: false
  redis-host: ""
```

A shared MySQL database is not enough on its own. Two servers reading the same rows will each cache
a farmer and each write over the other. Redis carries the messages that keep them in step — a
change made on one server is announced to the others rather than discovered later.

Enable both together, or neither:

| MySQL | Redis | Result |
|---|---|---|
| No | No | Single server, fine |
| Yes | No | Farmers appear on both servers and silently overwrite each other |
| Yes | Yes | Supported |

## Saving

Data is written on a schedule and on shutdown. `/uxmfarmer save` forces a write immediately, which
is what to run before a manual restart or before taking a backup.

## Backups

Back up the plugin folder and, if you are on MySQL, the database. Both together — the configuration
and the data are separate, and a restored database with a mismatched
`collected-materials.yml` gives you farmers holding products that no longer have levels.

<Callout type="warning" title="Do not copy a SQLite file from a running server">

`plugins/uxmFarmer/database.db` is being written to. Copying it live can produce a file that opens
and is missing recent farmers. Run `/uxmfarmer save`, stop the server, then copy — or move to MySQL,
where your usual dump tooling handles this properly.

</Callout>

## Migrating from SQLite to MySQL

There is no in-plugin migration. The reliable path on a live server is:

1. Announce downtime and stop the server.
2. Back up `plugins/uxmFarmer/` in full.
3. Set up the MySQL database and user.
4. Change `database.driver` to `MYSQL` and fill in the details.
5. Start the server — the schema is created automatically.

Existing SQLite farmers do not come across. Plan the switch for before launch, or open a ticket with
UXPLIMA support if you need to move data that already exists.

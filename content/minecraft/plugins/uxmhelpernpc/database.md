---
title: Database
order: 70
icon: database
---

NPC data — owner, location, entity type, skin, glow, hidden state — is stored through ORMLite,
with SQLite and MySQL as the two drivers.

```yaml
database:
  driver: "SQLITE"
  host: "localhost"
  database: "database"
  username: ""
  password: ""
  port: 3306
  use-ssl: false
```

## SQLite

The default. `database` is the file name, written under the plugin's folder. Nothing else in the
block is read. Right for a single server.

## MySQL

Set `driver: "MYSQL"` and fill in `host`, `port`, `database`, `username`, `password`. `use-ssl`
follows whatever your server requires — leave it `false` for a database on the same host.

## More than one server

Sharing one MySQL database between servers is not enough on its own: each server holds NPCs in
memory, and a change made on one is invisible to the others until a restart. Turn on the
multi-server block as well:

```yaml
multi-server:
  enabled: true
  redis-host: "127.0.0.1"
```

Redis carries the change notifications; MySQL holds the data. Both are needed.

## Saving

Data is written as it changes and on shutdown. `/uxmhelpernpc save` forces a flush, which is worth
running before a hard restart or a backup snapshot.

<Callout type="warning" title="Back up before switching drivers">

Changing `driver` points the plugin at a different, empty store. It does not migrate your NPCs
from the old one. Export first, or expect every NPC to be gone on the next start.

</Callout>

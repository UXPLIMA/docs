---
title: SQLite
order: 902
description: The zero-setup default, and when to move off it.
icon: file
---

The shipped configuration. Nothing to install.

```yaml
database:
  type: 'AUTO'
  username: 'root'       # ignored
  password: 'password'   # ignored
  url: 'jdbc:sqlite:./plugins/uxmClaims/data/claims.db'
```

`username` and `password` are inert: SQLite has no accounts. Leaving them at the shipped values is
harmless here, and a landmine if you later switch to MySQL without editing them.

The path is relative to the server directory. The file and its parent are created on first start.

## When it is the right choice

- one server
- file-level backups
- no need to read the data from anywhere else

That covers most servers. SQLite with WAL handles a claims workload comfortably; the reason to move is
architecture, not performance.

## When to move

- **A second server needs the same claims.** SQLite is a local file; two servers cannot share it
  safely over a network filesystem.
- **Your backups are database-level.** Point-in-time recovery on a file is a file copy.
- **You want to query claims from outside.** A web panel reading MySQL is straightforward; reading a
  live SQLite file over the network is not.

## Backing up

```bash
sqlite3 plugins/uxmClaims/data/claims.db ".backup 'claims-backup.db'"
```

Safe while the server is running. A plain `cp` of a live SQLite file can capture a partially written
transaction.

<Callout type="danger" title="Do not put the file on a network share">

NFS, SMB and most network filesystems implement locking in ways SQLite cannot rely on. The failure
mode is not an error: it is silent corruption discovered days later. Keep the file on local disk, or
move to [MySQL](mysql.md).

</Callout>

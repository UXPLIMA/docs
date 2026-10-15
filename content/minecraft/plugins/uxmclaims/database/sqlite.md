---
title: SQLite
order: 902
description: The zero-setup default, and when to move off it.
icon: file
---

SQLite is the shipped configuration: one local file, nothing to install.

```yaml
database:
  type: 'AUTO'
  username: 'root'       # ignored
  password: 'password'   # ignored
  url: 'jdbc:sqlite:./plugins/uxmClaims/data/claims.db'
```

`username` and `password` are ignored: SQLite has no accounts. Set them anyway before you switch to
MySQL or PostgreSQL, because the shipped values are a placeholder.

The path is relative to the server directory. The missing parent folders and the file are created
on first start, and foreign keys are switched on for every connection.

## When it is the right choice

| Your situation | SQLite fits |
|---|---|
| One server | Yes |
| File-level backups | Yes |
| A web panel or tool that reads claim data | No, use [MySQL](mysql.md) or [PostgreSQL](postgresql.md) |
| Database-level backups or point-in-time recovery | No |

Claim reads come from memory on every backend, so SQLite is not slower in play. The reason to move
is how you run and back up the data.

## Backing up

```bash
sqlite3 plugins/uxmClaims/data/claims.db ".backup 'claims-backup.db'"
```

This is safe while the server is running. A plain `cp` of a live SQLite file can capture a partially
written transaction; stop the server first if you copy the file.

<Callout type="danger" title="Keep the file on local disk">

NFS, SMB and most network filesystems implement locking in ways SQLite cannot rely on. The failure
is not an error: it is silent corruption discovered days later. Keep the file on local disk, or
move to [MySQL](mysql.md).

</Callout>

Related: [Overview](overview.md), [MySQL](mysql.md)

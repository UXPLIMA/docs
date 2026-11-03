---
title: Your first backup
order: 204
description: A nightly copy to another machine over SFTP, checked and kept for two weeks.
icon: footprints
---

## 1. Turn on a remote storage

In `config.conf`, under `storages`, uncomment the `sftp` example and fill it in:

```hocon
offsite {
  type     = "sftp"
  enabled  = true
  host     = "backup.example.com"
  port     = 22
  username = "minecraft"
  key-file = "/home/minecraft/.ssh/id_ed25519"
  known-hosts = "/home/minecraft/.ssh/known_hosts"
  folder   = "backups/lobby"
  keep-count = 14
}
```

A key is better than a password. A password is written `${BACKUP_SFTP_PASSWORD}` and set in the environment where the
server starts, so it never sits in the file.

## 2. Send the nightly set there

```hocon
sets {
  full {
    storages = ["local", "offsite"]
  }
}
```

## 3. Reload and check

`/uxmbackup reload`, then `/uxmbackup storages`: `offsite` should answer.

## 4. Run it once

`/uxmbackup run full`. The console follows the run every `progress.every-seconds`. When it ends, `/uxmbackup list`
shows it and `/uxmbackup verify <id>` reads it back against its manifest.

## 5. Set the restore passphrase now

In the console: `/uxmbackup hash <a long passphrase>`. Paste the line it prints into `restore.passphrase-hash`. Keep
the passphrase somewhere other than the server.

## More

**Player data every hour, kept for two days.** Add a set `players { worlds = [], paths = ["world/playerdata"],
storages = ["local"], schedule = "1h" }` and give the `local` storage `keep-days = 2` if nothing else uses it.

**Say in chat when a backup fails.** Set `effects.failed = ["[broadcast] <red>The <set> backup failed: <detail>"]`.

Related: [Storages](../features/storages.md), [Restoring](../features/restoring.md)

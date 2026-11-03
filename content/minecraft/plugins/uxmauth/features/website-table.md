---
title: Website table
order: 412
description: Keeping a website's login table beside the plugin's own, in the shape many website panels read.
icon: globe
---

`website-table` keeps a website's login table in the format many login plugins share and websites such as LeaderOS and
Minexon read. Everything here applies at the next start.

With `enabled = true`, every password account is written there too, with its password in the format `hash` names, and
that table is where a password lives: a change on the website works at the next login, a change in game works on the
website, and an account the website made logs in and gets this plugin's own row then.

| Key | Default | Meaning |
|---|---|---|
| `enabled` | `false` | |
| `table` | `"authme"` | Made in the plugin's database with these columns when missing; an existing one is used as it is |
| `hash` | `"sha256"` | The format the website reads: `sha256`, `bcrypt`, `pbkdf2` or `argon2`. While on, every new password is written in it whatever `hashing.algorithm` says |
| `columns` | `username`, `realname`, `password`, `ip`, `lastlogin`, `regdate`, `regip` | The column names |

- An account the website made is adopted at its first lookup with the offline UUID of its name, unverified, so the next
  connection asks whether a paid player owns the name.
- A row counts as an account's only once the plugin wrote it, adopted it, or found the account's own hash in it. A row
  someone made on the website under the name of an account the plugin held first opens nothing and is never written.
- Premium and Bedrock accounts never read or write a row, and a registration in game never writes over one.
- Removing an account in game removes its row. The plugin's own row is kept, so turning the table off loses only what the
  website changed since.
- `sha256` is weak: choose it only for a website that reads nothing else.

Every name is letters, digits and underscores.

Related: [config.conf](../config/config-conf.md#website-table), [Database](../database/overview.md)

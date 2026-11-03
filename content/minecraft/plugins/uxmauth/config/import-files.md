---
title: Import files
order: 508
description: How each supported login plugin keeps its accounts, and describing one of your own.
icon: file-input
---

`imports/<source>.conf` says how one login plugin keeps its accounts. `import.source` names the file.

| File | Plugin | Table | Notes |
|---|---|---|---|
| `authme` | AuthMe, its forks, and websites that share its table | `authme` | `$SHA$salt$hash` by default; bcrypt, Argon2 and PBKDF2 read as they are; dates in milliseconds |
| `nlogin` | nLogin | `nlogin` | Ids without dashes, the paid account's Mojang id beside the account's own |
| `openlogin` | OpeNLogin | `openlogin` | Names in lower case with the written name beside, bcrypt |
| `librelogin` | LibreLogin | `librepremium_data` | The hash, its salt and its algorithm in three columns |
| `limboauth` | LimboAuth | `AUTH` | bcrypt, written `$2a$` or `BCRYPT$` |
| `jpremium` | JPremium | none: set `import.table` | Ids without dashes; `SHA256$salt$hash` and others |
| `loginto` | LoginTo | `LoginTo_Players_Accounts` | Keeps no names: each account's name is the one this server last saw its id play |
| `authy` | Authy | `players` | A plain SHA-256 digest, the weakest, moved on at the next login |
| `dynamicbungeeauth` | DynamicBungeeAuth | none: set `import.table` | A salted double SHA-512 |
| `loginsecurity` | LoginSecurity | `ls_players` | Algorithm 7 is bcrypt |
| `authplus` | Auth+ | `Auth/players.yml` | A file of its own: no database keys needed |
| `fastlogin` | FastLogin | `premium` | A list of premium names, for `import.premium.source` |

## A file of your own

A file names a `display`, a `table`, an `id-style`, a `time-style`, a `hash-style` and its `columns`:

```hocon
display = "AuthMe"
table = authme
id-style = none
time-style = millis
hash-style = dollar-sha
columns {
  name = realname
  password = password
  totp = totp
  address = ip
  registered = regdate
  last-login = lastlogin
}
```

Copy the nearest shipped file and change the names to describe any other table.

Related: [Moving accounts](../integrations/migration.md)

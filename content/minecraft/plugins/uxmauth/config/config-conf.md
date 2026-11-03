---
title: config.conf
order: 601
description: Every block of config.conf, its keys and defaults, and the page that explains each.
icon: file-cog
---

## language

| Key | Default |
|---|---|
| `default` | `"en"` |
| `follow-client` | `true` |
| `force` | `""` |

## storage

| Key | Default | Meaning |
|---|---|---|
| `jdbc-url` | `""` | Empty is the SQLite file in `data/`; see [Database](../database/overview.md) |
| `username`, `password` | `""` | |
| `pool-size` | `4` | |
| `move-to.jdbc-url`, `username`, `password` | `""` | The target of `/uxmauth database move` |

## names, passwords, hashing, lane

See [Passwords](../features/passwords.md). `names`: `pattern`, `min-length`, `max-length`, `blocked`,
`reconnect-from-same-address`. `passwords`: `min-length`, `max-length`, `allowed`, `required`, `confirm-on-register`,
`old-on-change`, `max-attempts`, `common-file`, `breach-check`. `hashing`: `algorithm`, `argon2id.*`, `bcrypt.cost`,
`pbkdf2.iterations`. `lane`: `threads`, `max-threads`, `queue`.

## hold

`movement`, `chat`, `commands`, `allowed-commands`, `blocks`, `interact`, `inventory`, `damage`, `vehicles`, `portals`,
`teleports`, `hide-players`, `blindness`, `hide-inventory`, `hide-stats`, `join-message-waits`, all `true` but
`allowed-commands = []`. See [The waiting room](../features/waiting-room.md#what-a-waiting-player-may-not-do).

## location, notices, purge, account, bedrock, timeouts

| Key | Default |
|---|---|
| `location.login-spot` | world `""`, 0.5, 64, 0.5 |
| `location.rescue`, `rescue-reach` | `true`, `6` |
| `notices.last-login` | `true` |
| `purge.enabled`, `after-days`, `keep-premium`, `check-hours` | `false`, `180`, `true`, `24` |
| `account.delete-confirm-seconds` | `60` |
| `bedrock.auto-login` | `true` |
| `timeouts.register-seconds`, `login-seconds`, `reminder-seconds` | `90`, `60`, `5` |

## premium

`backup-password`, `sources`, `cache-minutes`, `mojang-cap-per-10-minutes`, `timeout-millis`, `addresses`; see
[Premium names](../features/premium.md).

## sessions

`mode` (`chosen`), `minutes` (`60`), `skip-second-factor` (`false`); see [Sessions](../features/sessions.md).

## second-factor

`issuer`, `label`, `digits`, `period-seconds`, `window-steps`, `setup-minutes`, `backup-codes`, `backup-code-length`,
`required-permission`, `premium-skips`; see [Second factor](../features/second-factor.md).

## mail, email

See [Mail and recovery](../features/mail.md).

## history, staff, alerts, discord

| Key | Default |
|---|---|
| `history.keep-days` | `90` |
| `staff.recent-logins`, `alts-shown`, `date-format`, `in-game` | `10`, `20`, `"yyyy-MM-dd HH:mm"`, `[]` |
| `alerts.*` | see [Staff tools](../features/staff.md#alerts) |
| `discord.url`, `server-name`, `events.*` | see [Staff tools](../features/staff.md#discord) |

## placeholders

`date-format = "yyyy-MM-dd HH:mm"`, Java's pattern letters, in the server's own time zone.

## website-table

See [Website table](../features/website-table.md).

## import

`source`, `jdbc-url`, `username`, `password`, `table`, and the same five under `premium`; see
[Moving accounts](../integrations/migration.md).

Related: [Configuration](index.md)

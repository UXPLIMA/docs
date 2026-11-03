---
title: Alerts
order: 308
description: Action lines for each moment of a run, and the Discord webhook.
icon: bell
---

A run that finishes at four in the morning has nobody watching. `effects` in `config.conf` says what each moment does.
Nothing is on by default: a backup is not news to a player.

| Moment | When |
|---|---|
| `started` | The run began |
| `done` | It finished and reached every storage |
| `partial` | It finished but did not reach every storage |
| `failed` | It failed |
| `refused` | It never ran: another run was in flight, or the disk would have dropped below the floor |

| Verb | Payload |
|---|---|
| `[broadcast]` | A line to everybody |
| `[console]` | A command as the console, which is how a webhook or a Discord relay is reached |
| `[sound]` | Played to everybody the line before reached |
| `[message]` | The same as broadcast here, since a run belongs to nobody in particular |

A text starting with `@` names a line of the messages files, rendered for each reader in their own language. A line can
carry `<set>`, `<state>`, `<size>`, `<bytes>`, `<duration>`, `<storages>` and `<detail>`.

```hocon
effects {
  done   = ["[broadcast] <gray>The <set> backup finished: <size> in <duration>.", "[sound] BLOCK_NOTE_BLOCK_PLING"]
  failed = ["[broadcast] <red>The <set> backup failed: <detail>"]
}
```

## Discord

| Key | Default | Meaning |
|---|---|---|
| `discord.webhook-url` | `""` | A webhook; the URL is a credential, keep it in `${...}` |
| `discord.on-success` | `false` | Post when a run succeeds |
| `discord.on-failure` | `true` | Post when a run fails |

Nothing is ever sent to us. The server's operator gets the alert.

Related: [config.conf](../config/config-conf.md#effects)

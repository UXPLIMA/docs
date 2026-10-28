---
title: Upgrading to 2.2.0
order: 14
description: What to check before moving a 2.1.x server to uxmDiscordSync 2.2.0, and what behaves differently afterwards.
icon: circle-arrow-up
---

Your files carry over. On the first start the plugin copies the old files into
`backups/backup-<number>/`, adds new settings with their defaults, and removes settings that no longer
do anything. Values you changed are kept.

## Before you start

1. Back up `plugins/uxmDiscordSync/` and your database.
2. If several servers use the same bot or the same database, update them all at the same time. A
   server that finds a newer version on the shared database does not start.
3. Replace the jar and start the server. Read the console: every setting the upgrade changed on its
   own is named there.

Going back is not supported. Once 2.2.0 has written the files, an older jar refuses to start on them.

## Changed for you on the first start

| Setting | What happens |
|---|---|
| `multi-server.enabled: true` without a `redis` section | Turned off. It did nothing before 2.2.0, and left on it would now stop the server from answering Discord. See [Multi-server](../features/multi-server.md) to set it up for real |
| `two-factor.ip-whitelist` still the old default list | Emptied. Its ranges never matched before, and now that they do they would let every player behind a local proxy skip 2FA |
| `commands.admin.aliases` still `[uxmdiscordsync]` | `dsadmin` is added as a shorter name |
| `localization.language` | Removed. The language has always come from `plugin.language` |

## Removed

| Removed | Why, or what replaces it |
|---|---|
| Punishment sync and `punishment-sync.yml` | It was never connected to anything and did nothing |
| `/2fa` and `commands.twofactor` | The command did nothing. 2FA needs no command |
| `two-factor.freeze-player` | A player waiting for 2FA is always frozen. `freeze-restrictions` choose what they cannot do |
| `multi-server.link-mode` | A link always counts on every server. `separate` is refused with a warning |
| `boost-rewards.yml`: `send-discord-message`, `notification-channel-id` | Use the `boost-claim` audit log |
| `discord.linking.notification-channel-id` | Use the `account-link` audit log |
| `embeds.colors` | Every embed has its own `color` |
| `plugin.auto-save-interval`, `localization.formats.date` and `.time` | Nothing read them |
| `advanced.yml`: `async`, `performance`, `cache.cache-linked-accounts`, the retry settings, `cleanup.delete-old-logs-after-days` | Nothing read them. Webhook retries are now `discord.webhooks.max-retries` and `retry-delay` |

## What behaves differently

| Area | 2.1.x | 2.2.0 |
|---|---|---|
| First-link rewards | Paid again when a Discord account linked to another Minecraft account | Paid once per Minecraft account and once per Discord account, ever |
| `post-link-commands` in `rank-sync.yml` | Never ran | Run on the first link, for the highest-priority rank the player has |
| Unlinking | Linked roles stayed | Linked roles and synced rank roles are removed, and `post-unlink-commands` also run for an offline player |
| Several servers on one bot | Every server answered every click | One server answers, through Redis |
| `/broadcast` | Sent to the server that caught it | Takes a `server` option and is sent to that server only |
| Level XP | Every member | Only linked members, unless `linked-only: false` |
| Voice XP | Anyone in a listed channel | Not while deafened, alone or in the AFK channel |
| Levels missing from `xp-per-level` | Counted from zero with the formula | Counted on from the nearest listed level below |
| 2FA request | No location | The city and country, with a warning when the country changed, and country rules |
| Webhook logs | A failed post was lost | Retried, and audit posts that come too fast wait their turn |
| Discord members joining and leaving | Not logged | Logged through `join-leave-logging.discord` |
| Permissions | Unknown to the server until granted | Declared, with defaults: see [Permissions](../permissions.md) |
| A YAML error in a file | The plugin started on defaults | The plugin does not start, and names the file and the line |
| An unknown `database.type` | Fell back to flatfile | The plugin does not start |
| Update check | Asked GitHub | Asks uxplima.com |

## Moving off flatfile

New installations use SQLite. An existing server keeps the database it had. If it is on `flatfile`,
moving to SQLite or a SQL server is worth doing now; [Database](../config/database.md#moving-off-flatfile)
explains the import.

## After the first start

Link and unlink a test account, check that the linked roles come and go, and if you use 2FA, log in
once with an account that is challenged. Then check `/dsadmin stats` for the number of linked
accounts you expect.

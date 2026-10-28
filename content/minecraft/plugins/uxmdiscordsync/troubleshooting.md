---
title: Troubleshooting
order: 80
description: What to check when the plugin does not start, the bot stays offline, linking fails or a feature does nothing.
icon: life-buoy
---

Start with the console: the plugin names most problems there, often with the fix. When it says
nothing, set `plugin.debug: true` and `error-handling.log-stack-traces: true` in `advanced.yml`, run
`/dsadmin reload` and try again.

## The plugin does not start

| Console says | Fix |
|---|---|
| `License key is not set` | Set `plugin.license-key`. No key yet: open a ticket in the UXPLIMA Discord |
| `License validation failed` | The key is wrong or not valid for this server. Ask UXPLIMA |
| `these files have YAML errors`, with a file and a line | Fix that line, or delete the file to get a fresh one |
| `plugin.server-name '...' may only contain letters` | Use letters, digits, spaces, `-` and `_` only |
| `Failed to connect to database` or `Unknown database.type` | Check the `database` section. See [Database](config/database.md) |
| `config.yml was written by uxmDiscordSync ...` | The files are from a newer version. Install that version or newer |
| `runs uxmDiscordSync ..., newer than this server's` | Another server on the same database runs a newer version. Update this one |
| `the multi-server settings need fixing` | Each server needs its own `server-name`, not `main`, and a shared database. See [Multi-server](features/multi-server.md) |

## The bot stays offline

The console shows `Failed to start Discord bot:` and a reason.

| Reason mentions | Fix |
|---|---|
| `token not configured`, or the token is invalid | Copy the token again from the Developer Portal into `discord.token`, then restart |
| Disallowed intents | Turn on both intents in the Developer Portal. See [Creating the bot](getting-started/discord-bot.md#the-application) |
| `Guild not found` | `discord.guild-id` is wrong, or the bot is not in that Discord server |

## Linking

| Problem | Cause and fix |
|---|---|
| No link message in the channel | The console names it: the channel id is wrong, or the bot lacks **View Channel**, **Send Messages**, **Embed Links** or **Read Message History** there |
| **This interaction failed** on the button | Another program is logged in with the same token: an old copy of the plugin, or a second server without multi-server mode. The console warns about it |
| The code is refused | It expired, was already used, or the player asked for a newer one, which replaces it |
| `Too many wrong codes` | Wait the seconds it names |
| No linked roles | The console says `Cannot give the linked role`: move the bot's role above it |
| No rewards for linking | The player is told the account was linked before. Rewards are paid once per account |

## Rank and nickname sync

| Problem | Cause and fix |
|---|---|
| Nothing happens | LuckPerms is missing (the console says so at startup), the role ids are placeholders, or the player is offline. Test with `/dsadmin forcesync <player>` |
| `Cannot add the Discord role` | Move the bot's role above that role, or give it **Manage Roles** |
| A player in `default` gets no role | They were given a rank with `parent set` and are no longer in `default`. Map their rank |
| A role given by hand disappears | That role is in `rank-mappings` and `remove-old-roles` is on |
| A nickname does not change | The member is the owner or ranks above the bot. With `skip-on-permission-error: true` this is silent |

## Two-factor login

| Problem | Cause and fix |
|---|---|
| Nobody is asked | Only linked players are asked. Check `require-permission`, `ip-whitelist` and remembered sessions |
| Everyone skips it | Behind a proxy without IP forwarding everyone shares one address. Remove it from `ip-whitelist` |
| The request never arrives | The member blocks direct messages from server members. The console says `Failed to open DM for 2FA` |
| `could not be checked right now` on joining | With several servers, Redis cannot be reached |
| The location shows `Unknown` | The GeoLite2 file is not where `geoip-database` points |

## Levels and boosts

| Problem | Cause and fix |
|---|---|
| No XP | The level system is off, the member is not linked while `linked-only` is on, or the channel is not listed. A thread has its own id |
| No voice XP | The member is deafened, alone, or in the AFK channel |
| `/discordreward` finds nothing | Only levels with an entry under `rewards` pay, and each once |
| `/boostprize` says not boosting | They boost another Discord server than `discord.guild-id` |
| `/boostprize` names another player | The boost belongs to that account. See [boostowner](commands.md#boostowner) |

## Logs and Discord commands

| Problem | Cause and fix |
|---|---|
| `Invalid ... webhook URL` | Paste a webhook URL starting with `https://discord.com/api/webhooks/`, not a channel link |
| Some chat is missing from the log | It was longer than `max-length`, started with an ignored prefix, or went past the limit per minute |
| A slash command is missing | Its feature is off, its file name is not a valid command name, or the bot was invited without `applications.commands` |
| `/servercommand` is refused | You are not in `authorized-users` on the server you named, or the channel is not allowed |

## Several servers

| Console says | Fix |
|---|---|
| `Duplicate server-name ... this instance stays passive` | Two running servers share a name. Rename one |
| `Redis unavailable` | Check Redis. Discord actions wait until it is back |
| `maxmemory-policy is ...` | Set `maxmemory-policy noeviction` in Redis |
| `uses this database too, but multi-server mode is off` | Two servers share the database and the bot without multi-server mode. Turn it on everywhere |

## Still stuck

Open a ticket in the UXPLIMA Discord with the plugin and server versions, the console from startup
with debug on, and the settings you changed.

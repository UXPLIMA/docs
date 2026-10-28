---
title: Troubleshooting
order: 80
description: What to check when the plugin will not start, an event will not run, players cannot join or Discord stays quiet.
icon: life-buoy
---

Start with the console: the plugin explains most problems there. When it says nothing, turn on debug
output by adding `debug: true` to the top of `config.yml` and running `/dragon reload`. See
[settings](config/settings.md#debug-output).

## The plugin turns itself off

| Console says | Fix |
|---|---|
| `License key is not set` or `License validation failed` | Set `license-key` in `config.yml` to the key from your UXPLIMA purchase |
| `DragonEvent requires WorldGuard to work` | Install WorldGuard |
| `DragonEvent requires PlaceholderAPI to work` | Install PlaceholderAPI |
| `You are running Folia` and a link to Worlds | Install Worlds by TheNextLvl |
| `You are running Paper 26.x.x without a supported world provider` | Install Worlds or MyWorlds |

Check also that the jar matches the server: see [Installation](getting-started/installation.md#which-jar).

## An event does not start

| Sign | Cause and fix |
|---|---|
| Staff are told `spawn-not-set` | No lobby spawn. Run `/dragon setlobbyspawn`, then `/dragon save` |
| Staff are told `start-failed` | The event world could not be built. The console shows the error just above it |
| `/dragon start` answers `already-starting` | The world is still being built. Wait a few seconds |
| The event starts and ends at once, with `force-stop` | A custom or vanilla world without a dragon spawn. Run `/dragon setdragonspawn`. See [A custom event world](getting-started/custom-world.md) |
| Scheduled events start hours early or late | `auto-start.timezone` is not recognised and counts as GMT. Use a region such as `Europe/Istanbul` |
| Scheduled events stopped for the rest of the month | `max-per-month` was reached. See [Scheduling](features/scheduling.md#max-per-month) |
| One scheduled event is missing | An event was already running then, or the server was off. Missed starts are not made up |

## Players cannot join

| Players are told | Cause and fix |
|---|---|
| `not-active` | No event is running |
| `can-not-join` | The event world or its spawn is not ready. Check the console for world errors |
| `economy-not-available` | `price-required` is on but Vault or the economy plugin is missing. Install them, or set `price-required: false` |
| `not-enough-money` | The player cannot pay `event-price` |
| `join-via-portal` | `/dragon join` is turned off. See [Joining](features/joining.md#the-command) |
| `same-ip-limit-reached` | Another player from the same address is in the event |
| Nothing at all | See below |

When walking into the entrance does nothing:

- `lobby-world-name` must be the world the entrance is in, and `worldguard-region-name` the exact name
  of the region around it.
- With `all-end-portals-leads-to-event: true` the region is not used; only portals are. See
  [Setting up the event](getting-started/setup.md#3-the-entrance).
- In portal mode, `world-list` names the world the portal stands in, and the server must allow that
  dimension: `allow-end` in `bukkit.yml` for end portals.

A player who is sent to the lobby spawn over and over is standing in the entrance: the lobby spawn
is inside the region. Set it somewhere else.

## During the event

| Problem | Cause and fix |
|---|---|
| No command works for players | `disable-all-commands: true` refuses every command and ignores the list. Set it to `false` to use the list. See [Commands](commands.md#commands-during-an-event) |
| A whitelisted command is refused | The list is matched with the slash and the same capital letters, and aliases are separate entries. A command typed with a plugin prefix is refused by `block-namespaced-commands` |
| The dragon has less health than set | The server caps health at 2048. See [The dragon](features/the-dragon.md#health) |
| Two boss bars | Set `dragon.boss-bar-visible: false` to hide the vanilla one |
| Staff hits do nothing to the dragon | Only players in the event can damage it. Staff who came in with `/dragon teleport` are watching, not playing |
| A player cannot teleport out | Players in the event leave with `/dragon leave` |

## After the event

| Problem | Cause and fix |
|---|---|
| Nobody got rewards | Rewards are only given when the dragon is killed. Time running out, `/dragon stop` and reloads give none |
| One player got no reward | They were offline or had left the event when it ended |
| A reward command did nothing | Run the same command from the console with a real name to see the error. Write it without `/` |
| The all-time leaderboards did not change | They update every 10 minutes. Run `/dragon save`, then `/dragon refresh` |
| A message shows as `messages.something` | That message is missing from `config.yml`. Add it back, or let the next update add it |

## Placeholders

Placeholders that show as raw text in another plugin: check that the expansion is loaded with
`/papi parse me %dragonevent_players%`, which should answer a number. The schedule placeholders stay
empty while `auto-start.enabled` is `false`.

## Discord

With `debug: true`, start an event and read the console lines that begin with `[Discord]`:

| Debug line mentions | Fix |
|---|---|
| `token is empty` | Set `token` in `discord.yml`, then `/dragon reload bot` |
| `bot is not initialized` | The bot did not log in: check the token, then `/dragon reload bot` |
| `channel-id is not set` | Set `channel-id` |
| `was not found in cache` | The id is wrong, or the bot cannot see that channel. The console lists the Discord servers the bot is in |
| `not a text/news/thread channel` | The id belongs to a voice channel or a category |
| `Failed to send` | Usually a missing permission: **View Channel**, **Send Messages** and **Embed Links** |

No countdown before an event started with `/dragon start` is expected: the countdown is only for
scheduled events.

## Still stuck

Open a ticket in the UXPLIMA Discord with the plugin version, the server version, the console from
startup, and the settings you changed.

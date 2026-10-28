---
title: Languages
order: 55
description: The message files, adding a language, and which texts live in the config files instead.
icon: languages
---

Every message the plugin sends in game, and the short answers the bot gives in Discord, are in
`languages/<language>.yml`. `en_US` and `tr_TR` ship, and `plugin.language` in `config.yml` picks
one. A change applies on `/dsadmin reload`.

```yaml
plugin:
  language: "en_US"
```

## The format

Messages in game are written in MiniMessage, and most start with the shared `prefix`:

```yaml
prefix: "<#00a2ff>uxmDiscordSync</#00a2ff> <dark_gray>►</dark_gray> "
cooldown: "<gradient:#FF6B6B:#C0392B>Please wait <yellow>{time}</yellow> seconds!</gradient>"
```

Values the plugin fills in are written in braces, such as `{code}`, `{time}` or `{player}`. When you
edit a message, keep the braces that are there; a placeholder you add in braces that the message does
not offer is shown as written. PlaceholderAPI placeholders such as `%player_name%` also work in
messages sent to a player.

The answers the bot gives in Discord, under `discord-commands`, `network`, `level-system` and
similar sections, are plain text with Discord formatting, not MiniMessage.

| Section | Holds |
|---|---|
| `linking`, `status` | `/link`, `/unlink` and `/linkstatus` |
| `boost`, `discord-reward` | `/boostprize` and `/discordreward` |
| `admin`, `stats`, `network` | `/dsadmin` and its subcommands |
| `two-factor` | The freeze, the kick messages, the notice to staff, and the location words in the 2FA request |
| `discord-commands`, `server-commands`, `broadcast` | Bot answers to custom commands, `/servercommand` and `/broadcast` |
| `level-system` | Bot answers to `/level` and `/leaderboard`, and the leaderboard lines |

## Adding a language

1. Set `plugin.language` to the new name, such as `de_DE`, and restart. The plugin creates
   `languages/de_DE.yml` as a copy of `en_US.yml`.
2. Translate the values in it, keeping the keys and the braces.
3. Run `/dsadmin reload`.

A message missing from your file is taken from the shipped English text, so a key you deleted shows in
English, not as nothing. New messages that come with an update are added to `en_US.yml` and
`tr_TR.yml` only; compare your own file with `en_US.yml` after an update.

## Texts that are not in the language files

The embeds, buttons and other texts that belong to one feature are set where that feature is, so a
full translation means editing those too:

| Text | Where |
|---|---|
| The link message, button, window and its answers | `discord.linking` in `config.yml` |
| The 2FA request, its buttons and status words, the kick messages | `two-factor.yml` |
| The stats message and its field names | `server-stats.embed` in `config.yml` |
| The bot's status | `discord.presence.messages` in `config.yml` |
| `/servercommand` and `/broadcast` confirmations, the broadcast prefix | `config.yml` |
| The level-up message, `/level` and `/leaderboard` embeds | `level-system.yml` |
| Every log message | `logging.yml` |
| Custom command answers | `discord-commands/` |
| The chat bridge formats | `chat-bridge` in `config.yml` |
| Slash command descriptions of the built-in commands | Fixed, in English |

Related: [config.yml](config-yml.md), [Placeholders](../placeholders.md)

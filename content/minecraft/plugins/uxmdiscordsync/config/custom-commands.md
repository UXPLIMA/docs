---
title: Custom Discord commands
order: 53
description: One file per Discord command, who may use it, the player it is about, and its placeholders.
icon: message-square-code
---

Every `.yml` file in `discord-commands/` is a Discord command of your own. The file name is the
command: `ip.yml` answers `/ip` and `!ip`. Six examples ship: `ip`, `serverip`, `store`, `stats`,
`balance` and `profile`. To make a new one, copy one, rename it and edit it.

`discord-custom-commands.yml` holds one switch for all of them:

```yaml
enabled: true
```

New, changed and removed files take effect on `/dsadmin reload`; no restart is needed. A slash
command can take a few seconds to show up in Discord.

## A command file

```yaml
enabled: true
description: "Show server IP address"
command-type: "both"
allowed-channels: []
required-roles: []
required-user-ids: []
response-type: "embed"
ephemeral: false
player-field-enabled: false
cooldown: 5
server: ""

embed:
  color: "#5865F2"
  title: "Server IP Address"
  description: |
    **Java Edition:** `play.yourserver.com`
  footer: "Click to copy!"
  fields: []

text: ""
```

| Key | Default | Meaning |
|---|---|---|
| `enabled` | `true` | `false` turns this command off |
| `description` | | Shown in Discord's command list, at most 100 characters |
| `command-type` | `both` | `slash` for `/ip`, `chat` for `!ip`, or `both` |
| `allowed-channels` | `[]` | Channel ids it works in. Empty means every channel |
| `required-roles` | `[]` | Role ids. The member needs at least one of them. Empty means everyone |
| `required-user-ids` | `[]` | User ids. Only these users may use it. Empty means everyone |
| `response-type` | `text` | `embed` answers with `embed`, `text` with `text` |
| `ephemeral` | `false` | Only the person who used it sees the answer. Slash commands only |
| `player-field-enabled` | `false` | Takes a player, see below |
| `cooldown` | `0` | Seconds before the same member can use it again |
| `server` | `""` | With several servers: the `server-name` that answers, so placeholders show that server's data. Empty means any |

`embed` takes the keys listed in [Concepts](../getting-started/concepts.md#embeds). Discord allows 25
fields, and 1024 characters in each field's value.

A slash command name can only hold letters, digits, `-` and `_`, up to 32 characters; a file with any
other name is skipped with a warning. Do not name a file `level`, `leaderboard`, `broadcast` or
`servercommand`: those are the plugin's own commands, and only one of the two would answer.

`chat` commands are typed at the start of a message, as `!name`. `slash` is the better choice: Discord
lists the command, shows its description, and the answer can be private.

## Who may use it

`required-roles`, `required-user-ids` and `cooldown` are all checked, and a member has to pass every
one. An empty list means no limit of that kind, not nobody.

| A member who | Gets |
|---|---|
| Uses a slash command in a channel not in `allowed-channels` | An answer that it does not work there |
| Uses a chat command in such a channel | No answer |
| Lacks the role or is not listed | An answer that they may not use it |
| Is on cooldown | The seconds left |

## The player it is about

Placeholders such as `%player_name%` need a player. With `player-field-enabled: false`, that is the
Minecraft account linked to whoever used the command. If they are not linked, the player placeholders
are shown as written, so keep them out of commands meant for everyone.

With `player-field-enabled: true`, the command takes a player:

| Typed | Player |
|---|---|
| `/profile player:Steve` or `!profile Steve` | Steve, who must have played on this server before |
| `/profile player:@member` or `!profile @member` | The Minecraft account linked to that member |
| `/profile` or `!profile` | Your own linked account. If you are not linked, you are told to link |

<Callout type="warning" title="A player field lets anyone look anyone up">

With `player-field-enabled: true`, everyone who can use the command can see any player's answer. A
profile showing coordinates, health or the world is a way to find people on a survival server.
Leave those placeholders out, or limit the command with `required-roles`.

</Callout>

## Placeholders

| Placeholder | Value |
|---|---|
| `%server_name%` | `plugin.server-name` |
| `%online_players%`, `%max_players%` | Players online and the maximum |
| `%server_total_players%` | Everyone who has ever joined |
| `%linked_accounts%` | Linked accounts |
| `%total_boosts%` | Boosts of your Discord server |
| `%player%`, `%player_name%` | The player's name |
| `%player_uuid%` | The player's UUID |
| `%player_displayname%` | Their display name while online, otherwise their name |
| `%world%` | Their world while online, otherwise empty |

With PlaceholderAPI installed, any PlaceholderAPI placeholder works too, such as
`%vault_eco_balance_formatted%` or `%statistic_deaths%`. For an online player they all work; for an
offline one, only those the expansion can answer without the player.

The answer never pings anyone, even if a player name or the text contains a mention.

Related: [Commands](../commands.md), [Placeholders](../placeholders.md)

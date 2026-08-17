---
title: Custom Discord commands
order: 53
description: "One file per Discord command: type, restrictions, embed and placeholders."
icon: message-square-code
---

Every `.yml` file in `discord-commands/` becomes one Discord command. The file name is the command
name: `ip.yml` gives you `!ip` and `/ip`.

Six ship as examples — `ip`, `serverip`, `store`, `stats`, `balance`, `profile`. Copy one, rename it,
edit it.

The master switch is in `discord-custom-commands.yml`:

```yaml
enabled: true
```

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

embed:
  color: "#5865F2"
  title: "🌐 Server IP Address"
  description: |
    **Java Edition:**
    ```
    play.yourserver.com
    ```
  thumbnail: ""
  footer: "Click to copy!"
  footer-icon: ""
  fields: []

text: ""
```

| Key | Meaning |
|---|---|
| `enabled` | `false` disables this one command |
| `description` | Shown in Discord's slash command list |
| `command-type` | `chat` (`!ip`), `slash` (`/ip`) or `both` |
| `allowed-channels` | Channel ids; empty means everywhere |
| `required-roles` | Role ids; empty means everyone |
| `required-user-ids` | User ids; empty means everyone |
| `response-type` | `embed` or `text` |
| `ephemeral` | Only the person who ran it sees the reply |
| `player-field-enabled` | Accept a player name argument |
| `cooldown` | Seconds, per user |
| `embed` | Used when `response-type` is `embed` |
| `text` | Used when `response-type` is `text` |

`chat` commands need the **Message Content** intent. `slash` commands do not, which makes `slash` the
better default.

## Placeholders

Command files are resolved through PlaceholderAPI, so anything your server exposes is available:

```yaml
description: |
  **Player:** %player_name%
  • Level: %player_level%
  • Balance: $%vault_eco_balance_formatted%
  • World: %player_world%
```

With `player-field-enabled: true`, the command takes a player name — `!ip Notch` or
`/profile player:Notch` — and placeholders resolve against that player. Without it, they resolve
against the Discord user's own linked account.

`fields` builds embed fields:

```yaml
fields:
  - name: "🎯 Kills & Deaths"
    value: "Kills: %statistic_PLAYER_KILLS%\nDeaths: %statistic_deaths%"
    inline: true
```

Discord allows 25 fields per embed and 1024 characters per field value.

<Callout type="warning" title="Placeholders can leak information">

`player-field-enabled: true` lets anyone in Discord look up any player. The shipped `profile.yml`
returns coordinates. On a survival server that is a location-sharing feature you did not intend —
remove the `%player_x%` fields, or restrict the command with `required-roles`.

</Callout>

## Restricting a command

The three restriction lists are checked together and all must pass:

```yaml
allowed-channels:
  - "111111111111111111"
required-roles:
  - "222222222222222222"
required-user-ids: []
```

That command works only in that channel and only for holders of that role. An empty list means no
restriction of that kind — not "nobody".

## Adding and removing

Adding a file, or changing `command-type`, needs a **restart**: slash commands are registered with
Discord when the bot connects. Editing an existing command's text, embed or restrictions only needs
`/uxmdiscordsync reload`.

Deleting a file removes the command on the next restart. Setting `enabled: false` is the reversible
way to do the same thing.

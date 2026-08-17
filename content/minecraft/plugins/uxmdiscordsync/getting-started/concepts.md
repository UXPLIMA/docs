---
title: Concepts
order: 13
icon: lightbulb
---

## The link

One Minecraft UUID paired with one Discord user id. Everything else in the plugin reads that pair —
rank sync needs to know which Discord user to give a role to, 2FA needs to know who to ask, boost
rewards need to know whose boost it is.

A link is created by the player: `/link` in Minecraft produces a code, and the button in the linking
channel takes it. Staff can create one directly with `/uxmdiscordsync forcelink`.

Codes expire after `discord.linking.code-expiration` seconds — 300 by default — and a player may
generate at most `security.code-generation-rate-limit` of them per minute.

## Server name

`plugin.server-name` identifies this server. On a single server it is cosmetic — it appears in
messages and embeds. On a network it decides who is who, and every server must have a different one.

## Link mode and reward mode

With `multi-server.enabled: true`, two settings decide how servers share:

| Setting | Value | Meaning |
|---|---|---|
| `link-mode` | `shared` | The player links once and is linked everywhere |
| | `separate` | The player links on each server independently |
| `reward-mode` | `per-server` | A reward can be claimed once per server |
| | `global` | A reward can be claimed once, ever |

`separate` combined with `global` is contradictory in practice and `shared` with `per-server` is the
combination most networks want: one identity, and rewards that are worth claiming on each server
you play.

<Callout type="warning" title="Rewards multiply if you get this wrong">

`link-mode: separate` with `reward-mode: per-server` means a player can claim the link reward once
per server. With five servers that is five diamonds, or five rank grants. Decide this before you
open the plugin to players, not after.

</Callout>

## Webhooks versus the bot

Two different paths reach Discord:

- **The bot** does anything interactive — buttons, modals, slash commands, roles, nicknames, bans.
- **Webhooks** do one-way logging — chat, join/leave, audit events.

They have separate rate limits, which is the point. A busy chat log posting through a webhook does
not slow down the bot's ability to answer a slash command. `discord.webhooks.rate-limit-delay`
throttles webhook posts to stay inside Discord's 30-per-minute allowance.

## Embeds

Almost everything the plugin posts is a configurable embed: a title, a description, a colour, a
thumbnail, a footer and optional fields. Colours are hex strings with a leading `#`, and the shared
palette lives under `embeds.colors` in `config.yml`.

Descriptions accept placeholders — `%player_name%`, `%discord_user%`, `%server_name%`,
`%timestamp%` and others depending on the event.

## Databases

Five backends: `flatfile`, `sqlite`, `mysql`, `mariadb` and `postgresql`. Flatfile is the default
and stores JSON in the plugin folder. Anything shared between servers needs a real database — see
[Database](../config/database.md).

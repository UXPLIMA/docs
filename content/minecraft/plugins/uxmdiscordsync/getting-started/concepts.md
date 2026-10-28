---
title: Concepts
order: 13
description: The link, first-link rewards, server names, the embed format every file shares and the two placeholder styles.
icon: lightbulb
---

## The link

A link pairs one Minecraft account with one Discord account, in both directions: a Minecraft account
has at most one Discord account, and a Discord account at most one Minecraft account. Every other
feature reads this pair. Rank sync needs to know whose roles to change, 2FA whom to ask, and boost
rewards whose boost it is.

A player creates the link with a code from `/link`, entered in Discord. The code is made in game,
where the player has already proven who they are, so someone who only has the Discord account cannot
claim a Minecraft account. Staff can also link two accounts directly with `/dsadmin forcelink`.

To move a link to another Discord account, the player unlinks first, then links again.

## First-link rewards

The commands in `linking.post-link-commands`, and those of the player's rank in `rank-sync.yml`, run
once per account, ever. A Minecraft account that was linked before gets nothing when it links again,
and neither does a Discord account that was linked to another Minecraft account. Unlinking and
linking again, or moving a Discord account to an alt, pays nothing.

A player who is offline when their code is entered gets these rewards the next time they join.

## Server name

`plugin.server-name` names this server in embeds, logs and placeholders. On a single server it is a
label. When several servers share the bot it is how they tell each other apart, so every server needs
its own; see [Multi-server](../features/multi-server.md).

## Embeds

Almost everything the bot posts is an embed you can edit. Every embed in every file takes the same
keys; leave a key empty or remove it and that part is left out.

| Key | What it is |
|---|---|
| `title` | The title |
| `description` | The main text. Discord formatting works: `**bold**`, `*italic*`, line breaks |
| `color` | The bar on the left, as `"#5865F2"`. The `#` is required; without it no colour is set |
| `thumbnail-url` | The small picture at the top right. `thumbnail` works too |
| `image-url` | The large picture at the bottom. `image` works too |
| `author-name`, `author-icon-url`, `author-url` | The line above the title, its icon and its link |
| `footer-text`, `footer-icon-url` | The line at the bottom and its icon. `footer` and `footer-icon` work too |
| `timestamp` | `true` shows the time of posting next to the footer |
| `fields` | A list of `name`, `value` and `inline`. A field with an empty name or value is skipped |

Pictures and links must be full `https://` addresses. Discord allows 25 fields and 1024 characters
per field value.

## Two placeholder styles

| Style | Used in |
|---|---|
| `%name%` | Config files: embeds, commands, formats |
| `{name}` | Language files, the chat bridge formats and the level-up message |

When you edit a text, keep the style it already uses: `{player}` in a config embed is printed as it
is. The placeholders each text accepts are listed on [Placeholders](../placeholders.md).

## The bot and webhooks

The bot does everything that answers someone: the link button, slash commands, roles, nicknames,
direct messages and the chat bridge. The logs go through webhooks instead, so a busy chat log never
slows down the bot's answers.

The bot never pings anyone by accident. Text it repeats from players or Discord users cannot
mention `@everyone`, `@here` or a role, and chat bridged from the game cannot mention anyone at all.
A role ping in an embed you write does not ping either.

## Storage

The plugin keeps links, codes, levels and cooldowns in a database. SQLite, a single file in the
plugin folder, is the default and needs no setup. Several servers sharing one bot need MySQL,
MariaDB or PostgreSQL. See [Database](../config/database.md).

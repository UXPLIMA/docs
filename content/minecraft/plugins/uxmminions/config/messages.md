---
title: Messages
order: 609
description: How a line is written, the prefixes, the blocks of keys, and the content words.
icon: message-square
---

Every line comes from `messages/messages_<language>.conf`. A line is MiniMessage and names roles, not colours; the
colours are in [theme.conf](theme.md). A key that is missing falls back to the default the code carries.

| Block | Said when |
|---|---|
| `admin.*` | The operator commands: version, usage, doctor, reload and what needs a restart |
| `error.*` | Refusals shared by every command |
| `minion.*` | Placing, refusals, trust, naming, consuming, teleport, the hologram lines, talk lines, upgrade requirement messages |
| `editor.*` | The editor's own sentences |
| `menu.*` | Every window's titles, tiles and buttons |
| `language.*`, `command.*` | Languages, the command layer |

The upgrade requirements name `@minion.upgrade.money`, `@minion.upgrade.item` and `@minion.upgrade.actions`; a level
that asks for something else names a key of your own.

## Content words

The shipped content names its words with `@content...`: `content.types.miner.display-name`,
`content.skins.skins.ember.name`.

Related: [Languages](languages.md), [theme.conf](theme.md)

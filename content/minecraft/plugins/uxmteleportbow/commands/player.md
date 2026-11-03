---
title: Player commands
order: 302
description: The language subcommands a player uses.
icon: user
---

A player needs no command to use the bow. The only player commands are the language the plugin is read in: the
replies, the refusals and the bow's own name and lore.

| Command | Permission | What it does |
|---|---|---|
| `/uxmteleportbow lang` | `uxmteleportbow.language` | Says which language you read uxmTeleportBow in, and which the server has. Player only |
| `/uxmteleportbow lang set <language>` | `uxmteleportbow.language` | Reads uxmTeleportBow in that language from now on. Player only |
| `/uxmteleportbow lang reset` | `uxmteleportbow.language` | Forgets your choice; you read your client's language again. Player only |

`<language>` is a tag such as `tr` or `pt-BR`; tab completion offers the languages the server has. With
`language.force` set, `lang set` answers `language.forced` and nothing changes. With uxmLang installed, the choice is
uxmLang's. See [Languages](../config/languages.md).

Related: [Overview](overview.md), [Operator commands](admin.md)

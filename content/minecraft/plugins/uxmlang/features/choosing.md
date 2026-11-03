---
title: Choosing a language
order: 301
description: The window, the command, following the client, forcing a language, and what a choice does.
icon: mouse-pointer-click
---

## Three ways to a language

| Way | Who |
|---|---|
| The [window](../menus/language.md), from `/lang` | A player, one click per flag; the compass follows the client |
| `/lang set <language>` | A player |
| `/uxmlang set <player> <language>` | An operator, for a player on this server |

A language is written as its tag: `en`, `tr`, `pt-BR`. The tag is the part of the file name between `messages_` and
`.conf`.

## Following the client

A player who chose nothing reads the language their client is set to, while `language.follow-client` is `true` and a
file exists for it. Otherwise they read `language.default`. The client's language is written to the database only when
it changes, so the next server knows it before the player's first line.

## Forcing a language

`language.force = "tr"` makes everybody read Turkish. A player's own choice is kept and comes back when the force is
cleared. `/lang set` and `/lang reset` answer `language.forced`.

## What a choice does

The `effects` block of [config.conf](../config/config-conf.md#effects) runs action lines at each moment:

| Moment | Shipped |
|---|---|
| `changed` | `[message] @language.changed`: "You now read this server in Türkçe" |
| `already` | `[message] @language.already` |
| `reset` | `[message] @language.reset` |

Add a sound or a title: `"[sound] entity.player.levelup 0.6 1.2"`. An empty list is silence. `<language>` is the
language's name as its readers write it. The line runs after the change, so it is already in the new language.

Related: [Player commands](../commands/player.md), [languages.conf](../config/languages-conf.md)

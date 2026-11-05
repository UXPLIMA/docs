---
title: Languages
order: 608
description: The seven languages and how a player's language is chosen.
icon: languages
---

## The shipped languages

| Tag | Language |
|---|---|
| `en` | English |
| `tr` | Turkish |
| `de` | German |
| `es` | Spanish |
| `fr` | French |
| `pt` | Portuguese (Brazil) |
| `ru` | Russian |

Every file under `messages/` is a language: drop in `messages_it.conf` and Italian is offered.

## Which language a player reads

| Step | Decided by |
|---|---|
| 1 | `language.force` in [config.conf](config-conf.md): when set, everyone reads it |
| 2 | The language the player chose with `/is lang <language>` |
| 3 | Their client's language, when `language.follow-client` is true and that language ships |
| 4 | `language.default`, `en` as shipped |

`/is lang` with no word shows the language you read and the choices. `/is lang reset` forgets your choice. The choice
is kept in `languages.conf` and holds for every UXM plugin on the server. Staff set or clear another player's with
`/is lang set-player` and `/is lang reset-player`.

Words the plugin reads back stay English in every language: `on` and `off`, the times and weathers of a Brix plot, and
the display kinds `sign`, `head` and `text`.

## Small capitals

The interface is drawn in small capitals for the languages listed under `small-caps` in [theme.conf](theme.md): English,
Turkish, German, Spanish, French and Portuguese. Russian keeps ordinary letters.

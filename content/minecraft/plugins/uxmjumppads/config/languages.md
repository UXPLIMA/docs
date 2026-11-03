---
title: Languages
order: 405
description: The seven shipped languages, how a player's language is chosen, adding a language, and uxmLang.
icon: languages
---

Every player reads uxmJumpPads in their own language: the replies, the operator replies and the effect lines that name an `@key`.

## Shipped

| File | Language | Small capitals |
|---|---|---|
| `messages_en.conf` | English | yes |
| `messages_tr.conf` | Turkish | yes |
| `messages_de.conf` | German | yes |
| `messages_es.conf` | Spanish | yes |
| `messages_fr.conf` | French | yes |
| `messages_pt.conf` | Portuguese | yes |
| `messages_ru.conf` | Russian | no: Cyrillic has no small capitals |

Every file carries every key. A key a translation lacks reads in `language.default`, then in the line built into the
plugin.

## Which language a player reads

The first that answers wins:

1. `language.force` in [config.conf](config-conf.md#language), when it names a language: everybody reads it.
2. The language the player chose with `/jumppad lang set`.
3. The language their client is set to, when `language.follow-client` is `true` and a file exists for it.
4. `language.default`.

A broadcast reaches each player in their own language, so one `[broadcast]` reaches the server in seven.

## Adding a language

Copy `messages_en.conf` to `messages_<tag>.conf`, such as `messages_it.conf` or `messages_pt-BR.conf`, and translate
the values. Keep every key and every value tag such as `<type>`. Run `/uxmjumppads reload`: the language exists,
`/jumppad lang` offers it, and a client set to it reads it. Nothing lists the languages; the files are the list.

To draw a Latin-alphabet language in small capitals, add it to `small-caps` in [theme.conf](theme.md#small-capitals).

## Where a choice is kept

| Server | The choice lives in |
|---|---|
| With uxmLang | uxmLang, shared by every UXM plugin and, on a network sharing uxmLang's database, by every server |
| Without uxmLang | `languages.conf` in this plugin's folder, for this plugin alone |

## Notes

- **Command words per language** are set in [commands.conf](commands-conf.md#words-per-language).
- **A language file that cannot be read** is reported in the console at the start and on reload, and its players
  read the default.

Related: [Messages](messages.md), [config.conf](config-conf.md#language), [Player commands](../commands/player.md)

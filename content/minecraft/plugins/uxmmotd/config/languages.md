---
title: Languages
order: 604
description: The seven shipped languages, which one a viewer reads, and adding a language.
icon: languages
---

| File | Language | Small capitals |
|---|---|---|
| `messages_en.conf` | English | yes |
| `messages_tr.conf` | Turkish | yes |
| `messages_de.conf` | German | yes |
| `messages_es.conf` | Spanish | yes |
| `messages_fr.conf` | French | yes |
| `messages_pt.conf` | Portuguese | yes |
| `messages_ru.conf` | Russian | no: Cyrillic has no small capitals |

## Which language a viewer reads

1. `language.force`, when it names a language.
2. The language the client reports, when `language.follow-client` is on and a file exists for it.
3. `language.default`.

uxmMOTD has no command to choose a language: a viewer of the server list has not joined. On Paper the kick screen is in
the server's default language, since there is no client yet.

## Adding a language

Copy `messages_en.conf` to `messages_<tag>.conf` and translate the values, keeping every key and value tag. Run
`/uxmmotd reload`. To draw a Latin-alphabet language in small capitals, add it to `small-caps` in
[theme.conf](theme.md#small-capitals).

Related: [Messages](messages.md)

---
title: Languages
order: 606
description: The seven shipped languages, adding a language, and what every other UXM plugin needs for it.
icon: languages
---

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

Every file carries every key.

## Adding a language

1. Copy `messages_en.conf` to `messages_<tag>.conf`, such as `messages_it.conf` or `messages_pt-BR.conf`, and translate
   the values. Keep every key and every value tag such as `<language>`.
2. Name it and give it a flag in [languages.conf](languages-conf.md).
3. Add the same file to every other UXM plugin you run, or they read their default for it.
4. `/uxmlang reload`. The language is in the window and `/lang set` takes it.

To draw a Latin-alphabet language in small capitals, add it to `small-caps` in [theme.conf](theme.md#small-capitals).

Related: [Messages](messages.md), [Other plugins](../features/other-plugins.md)

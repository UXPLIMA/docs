---
title: Languages
order: 213
description: Every message and menu in eight languages, chosen with one setting.
icon: languages
---

`general.language` in `config.yml` sets the language of every chat line, action bar, boss bar, title
and menu. It applies to the whole server.

## Supported languages

| Code | Language |
|---|---|
| `en` | English |
| `tr` | Turkish |
| `es` | Spanish |
| `pt-BR` | Portuguese (Brazil) |
| `de` | German |
| `fr` | French |
| `ru` | Russian |
| `pl` | Polish |

`pt`, `pt_br` and `TR` are read as you would expect. An unsupported code logs a warning and falls back to
English.

## Where the files are

| Language | Edit |
|---|---|
| English | `messages.yml` and `menu/*.yml`, as always |
| Any other | `lang/<code>/messages.yml` and `lang/<code>/menu/*.yml` |

A language's folder is written the first time it is chosen. After that it is yours: switching to
another language and back never overwrites what you changed.

## Switching

```yaml
general:
  language: tr
```

Then `/builders admin reload`. No restart is needed.

## Missing lines

Every file falls back in three steps: your copy, then the shipped translation, then the shipped English.
A line an update adds appears in your language even though your file predates it, and a line a
translation lacks appears in English rather than as a blank.

## Notes

- Command names and their arguments stay English in every language: `/builders warehouse deposit all`
  is the same command everywhere.
- Builder and package names come from `builders.yml` and `packages.yml`, which are not translated:
  name them in your players' language there.
- Money formatting is separate: `economy.locale` decides thousands and decimal marks.

Related: [Messages and menus](../config/messages-and-menus.md), [config.yml](../config/config-yml.md)

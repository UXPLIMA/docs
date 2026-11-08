---
title: Languages
order: 313
description: Every message and menu in eight languages, chosen with one setting.
icon: languages
---

`general.language` in `config.yml` sets the language of every chat line, action bar, boss bar, title,
menu, builder and package name, and Discord text. It applies to the whole server.

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

Letter case does not matter, `_` reads as `-`, and `pt` or `br` mean `pt-BR`. An unsupported code logs
a warning and falls back to English.

## Where the texts are

There is one set of files, at the usual paths, written in the language in use:

| File | Texts |
|---|---|
| `messages.yml` | Every message |
| `menu/*.yml` | Every menu |
| `builders.yml` | Builder names, descriptions and level names |
| `packages.yml` | Category names |
| `webhooks.yml` | The Discord texts |
| `config.yml` | The NPC nameplate format and the seed name for new packages |

The files are first written in English. The language they are in is remembered in `data/metadata.yml`.

## Switching

```yaml
general:
  language: tr
```

Then `/builders admin reload`, or a restart. The plugin first copies every file to
`backups/language-<old>-<time>/`, then, file by file:

| Your file | What happens |
|---|---|
| Untouched since it was written | Replaced by the new language's copy, comments included |
| Edited | Each text still equal to the old language's is rewritten; every text you changed is kept |

The console says how many texts were updated and how many edited ones were kept. Switching back later
works the same way, so your own wording survives any number of switches. If a file cannot be switched,
the console says why and the switch is tried again on the next reload or restart.

## Missing lines

A message missing from your `messages.yml` uses the English text built into the plugin, whatever the
language; a menu button missing from your file is taken from the shipped copy. See
[Messages and menus](../config/messages-and-menus.md#updating).

## Notes

- Command names and their arguments stay English in every language: `/builders warehouse deposit all`
  is the same command everywhere.
- Prices, stats and slots in `builders.yml`, `packages.yml` and the menus are the same in every
  language; a switch only touches texts.
- The blueprint item's name and lore are messages (`blueprintName`, `blueprintLore`), so they follow the
  language. Its material and model stay in `config.yml` under `start.item`.
- Money formatting is separate: `economy.locale` decides thousands and decimal marks.

Related: [Messages and menus](../config/messages-and-menus.md), [config.yml](../config/config-yml.md)

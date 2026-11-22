---
title: Languages
order: 144
description: The shipped languages, per-player languages, where texts live, theme.yml and /uxm lang update.
icon: languages
---

Every text a player sees is in a language file you can edit: kernel texts in `lang/<code>/`, each module's in
`modules/<id>/lang/<code>/`. The server has one default language; per-player languages are optional.

## Commands

| Command | What it does | Permission |
|---|---|---|
| `/uxm lang update` | Lists the texts kept at an upgrade because they may be your edits, per file | `uxmclaims.admin` |
| `/uxm lang update <file>` | The same, only for files whose path contains `<file>` (for example `lands`) | `uxmclaims.admin` |
| `/uxm lang update confirm [file]` | Replaces those texts with this version's, after a backup, and reloads (`--force` does the same) | `uxmclaims.admin` |
| `/uxm lang update keep [file]` | Keeps them as your edits; they are not offered again | `uxmclaims.admin` |
| `/uxm reload` | Reads every language, menu and theme file again | `uxmclaims.admin` |

## Shipped languages

| Code | Language |
|---|---|
| `en` | English, complete in every module; the fallback for any missing text |
| `tr` | Turkish |

Any other code (`de`, `pt-BR`) is accepted: its folders are created from the English copy, and you translate
them. A code that is not a language code falls back to `en` with a warning.

## Where texts live

| File | Holds |
|---|---|
| `lang/<code>/messages.yml` | Kernel messages: errors, help, database, presets, setup |
| `lang/<code>/commands.yml` | Local names of the `/uxm` commands |
| `modules/<id>/lang/<code>/messages.yml` | One module's messages, flag and permission names |
| `modules/<id>/lang/<code>/commands.yml` | Local names, aliases, usage and descriptions of a module's commands |
| `modules/<id>/lang/<code>/menus/` | Menus, only when you copy one there, see [Menus](menus.md) |
| `theme.yml` | Colour tokens shared by every language |

A message is a string (sent to chat), or a section with `type` (`CHAT`, `ACTIONBAR`, `TITLE`, `NONE`), `text`,
`sound`, `volume` and `pitch`. Texts are MiniMessage. A text is looked up in the file on disk, then in the jar
copy of the same language, then in the jar's English copy.

In `commands.yml`, a `label` adds a local name next to the English one and `aliases` adds more. The English
names always work. Command names change after a restart.

## Per-player languages

| Key in `config.yml` | Default | What it does |
|---|---|---|
| `language.default` | `en` | The server language, used for the console, logs, webhooks, maps and every player when per-player is off |
| `language.per-player` | `false` | Players get their client language when it is loaded. [restart] |
| `language.enabled` | `[en]` | The languages loaded besides the default |
| `language.locale-map` | `pt_br: pt-BR`, `pt_pt: pt-BR` | Client locale to language code |

A client locale is matched through `locale-map`, then as the code itself (`pt_br` to `pt-BR`), then by its first
two letters (`tr_tr` to `tr`). A locale that matches no loaded language gets the server language. The last
known locale is stored, so offline lookups use it too. Command names always follow the server language.

## theme.yml

```yaml
config-version: 1
tokens:
  primary: '<#3BA7FF>'
  secondary: '<gold>'
  success: '<green>'
  warning: '<yellow>'
  error: '<red>'
  muted: '<gray>'
  highlight: '<white>'
```

Any message or menu text can write `<t:error>` instead of a colour. Change a token once and every language
follows. A value is a colour, a gradient opening tag or styles such as `<bold>`. Applied on `/uxm reload`.

## Shipped-text sync

When an update changes a shipped text, you do not replace your files. On the first start of the new version
the plugin compares each language, menu and theme file on disk with its record of what was shipped last time
(`data/shipped/`):

| The value on disk | What happens |
|---|---|
| Still the old shipped text | Replaced with the new text |
| Your own edit | Kept, and listed once in the log |
| Missing | Added from the jar |
| No record yet (first start after upgrading from a version without one) | Replaced when it equals an older shipped text the jar knows; otherwise kept as "unconfirmed" |

Unconfirmed texts may be your edits or old texts the plugin cannot recognise. The start-up log gives their
count; `/uxm lang update` lists them per file with up to three keys each, and `confirm` or `keep` settles them.
`confirm` copies each file to `backups/config/` first. `config.update-unchanged-defaults: false` turns the
sync off and only adds missing keys.

## Notes

- `language.default` is read at start-up only; restart after changing it. The comment in `config.yml` lists
  eight shipped languages, but 4.0 ships `en` and `tr` only.
- With per-player languages on, some texts stay in the server language: town and nation list entries, the warp
  menu's visibility word, signs, holograms, claim items and amounts in some group notices.
- There is no player command to pick a language; the client language decides.

Related: [Menus](menus.md), [Kernel config](kernel.md), [Configuration](index.md)

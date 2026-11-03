---
title: Installation
order: 202
description: Requirements, the files that appear, and checking the install.
icon: download
---

uxmHeads is one jar with uxmLib inside it. Put `uxmHeads-<version>.jar` in `plugins/` and start the server: joining
players get the welcome at once. This manual describes uxmHeads after 0.0.2: German, Spanish, French, Portuguese and
Russian arrive with the next release, and 0.0.2 itself reads English and Turkish.

## Requirements

| Requirement | Detail |
|---|---|
| Java | 25 or newer |
| Server | Paper 26.2 or newer, or Folia; any fork of Paper |
| Network | Outbound HTTPS to the skin hosts, `textures.minecraft.net` by default, and to the account service for `find-by-name` |

No database: faces are kept in memory and skins as files.

## Optional plugins

| Plugin | Adds |
|---|---|
| PlaceholderAPI | The faces in every other plugin's lines |
| Geyser or Floodgate | Bedrock players are drawn the Bedrock face |
| uxmLang | One language choice for every UXM plugin |

## The files

```
plugins/uxmHeads/
├── config.conf       language, face, bedrock, skins, welcome
├── commands.conf     the name, aliases and switch of every command and subcommand
├── messages/         messages_<language>.conf: the welcome lines and replies, en, tr, de, es, fr, pt, ru
├── languages.conf    the language each player chose, when uxmLang is not installed
├── skins/            downloaded skin files, a few kilobytes each
└── backups/          the last five copies of each file a release rewrote
plugins/uxmTheme/
└── theme.conf        the colours and glyphs every UXM plugin reads
```

## Checking the install

`/uxmheads doctor` (permission `uxmheads.admin.doctor`):

| Check | Says |
|---|---|
| `faces` | How many faces are in memory and how many skins are kept on disk |
| `fetches` | `every skin asked for was read`, or a warning naming the last skin that could not be read |
| `fallback` | The fallback skin, `none`, or fail: the address is not on a host under `skins.hosts` |
| `placeholders` | Published, or a warning that only this plugin's own lines draw faces |

uxmHeads sends no usage metrics.

Related: [Concepts](concepts.md), [config.conf](../config/config-conf.md)

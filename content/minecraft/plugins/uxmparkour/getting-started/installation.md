---
title: Installation
order: 102
description: Requirements, the plugins it can use, the first start, and the files that appear.
icon: download
---

uxmParkour is one jar with uxmLib inside it. Put it in `plugins/`, start once, then build an arena.

## Requirements

| Requirement | Detail |
|---|---|
| Java | 25 or newer |
| Server | Paper 26.2 or newer, or Folia; any fork of Paper, such as Purpur or Leaf |
| Internet on first start | The server downloads the MariaDB and PostgreSQL drivers from Maven Central into its library folder, once |
| Database | A SQLite file out of the box; MySQL, MariaDB or PostgreSQL to share the records of a network ([Database](../database/index.md)) |

## Optional plugins

uxmParkour starts without any of these. Each is used when it is installed; nothing has to be switched on.

| Plugin | Adds | Without it |
|---|---|---|
| PlaceholderAPI | The `%uxmparkour_...%` placeholders, for holograms, scoreboards and chat ([Placeholders](../placeholders/reference.md)), and other plugins' placeholders inside this plugin's lines | The placeholders answer nothing outside the plugin; `/parkour top` and `/parkour stats` still work |
| Vault or VaultUnlocked with an economy | Milestones that pay `vault` or `vault-unlocked` money | A `vault` line pays nothing and says so once in the console |
| PlayerPoints | Milestones that pay `playerpoints` | A `playerpoints` line pays nothing |
| EcoBits | Milestones that pay an EcoBits currency | An `ecobits:<currency>` line pays nothing |
| Treasury | Milestones that pay through Treasury | A `treasury` line pays nothing |
| mcMMO | `%mcmmo_level_<skill>%` inside a moment line, read from mcMMO itself | The token stays as written |
| Geyser or Floodgate | Bedrock forms instead of chest menus for Bedrock players | Bedrock players get the chest menus |

Experience points and levels need no plugin, so the shipped milestones pay on any server. See
[Rewards](../arenas/rewards.md).

## Installing

1. Put `uxmParkour-<version>.jar` in `plugins/` and remove any older uxmParkour jar.
2. Start the server. The plugin writes its files into `plugins/uxmParkour/` and opens its SQLite database.
3. Build an arena: [Your first arena](first-arena.md) walks through it.
4. Open the arena with `/uxmparkour enable <name>`. Players join it with `/parkour`.

<Callout type="danger" title="An arena's box must be empty air">

A course places its blocks wherever the generator puts them inside the box, whatever stood there, and sets them
back to air when the player has passed. A box drawn over part of your map deletes that part. Put the box in the sky,
or over an area with nothing in it.

</Callout>

## The files

```
plugins/uxmParkour/
├── config.conf          language, storage, run rules, boards, difficulties, themes, rewards, moments, items, cues
├── arenas.conf          the arenas, written by the admin commands and the editor
├── commands.conf        the name, aliases and switch of every command and subcommand
├── languages.conf       the language each player chose, when uxmLang is not installed
├── menus/
│   ├── themes.conf      the theme picker
│   ├── arenas.conf      the arena list, for operators
│   └── arena-editor.conf  the arena editor, for operators
├── messages/            messages_<language>.conf, one per language: en, tr, de, es, fr, pt, ru
├── data/
│   └── parkour.db       the SQLite database, when SQLite is the backend
└── backups/             the last five copies of each file a release rewrote
plugins/uxmTheme/
└── theme.conf           the colours and glyphs every UXM plugin reads, written by the first one installed
```

A file you delete is written again from the jar at the next start.

## Updating

Stop the server, replace the jar and start it. A release that adds a setting writes it into your files with the
comment that explains it, and never changes a value you set; the file as it was goes to `backups/`. The lists you
own, the difficulties, the themes and the milestones, are left as you wrote them: an entry you removed is not put
back. The database moves forward on its own at the start.

## Checking the install

`/uxmparkour doctor` (permission `uxmparkour.admin.doctor`) answers one line per check:

| Check | ok | warn or fail |
|---|---|---|
| `storage` | The backend, where the rows are, the table prefix and the schema version | fail: the database did not answer, and no run or record is kept |
| `windows` | How many menu files were read | fail: no menu file was read, so no menu opens |
| `placeholders` | `published` | warn: PlaceholderAPI is not installed, so the placeholders publish nothing |

`/uxmparkour version` prints the version the server runs.

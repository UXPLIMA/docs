---
title: "Guide"
order: 10
description: "How uxmQuests works, how to set it up and what each file holds."
---

## Install

Put `uxmQuests-<version>.jar` in `plugins/` and start the server. uxmLib is inside the jar, so nothing else
is required. The plugin runs on Paper 26.2 or later and on Folia.

Optional plugins, each reached only when present:

| Plugin | What it adds |
|---|---|
| PlaceholderAPI | `%placeholders%` in reward lines and in `placeholder` tasks. |
| Vault | Money lines in rewards, such as `[take-money]` and `[give-money]`, read through the economy the server has. |
| CoreProtect | Anti-glitch level three, see [objectives](objectives.md#anti-glitch). |
| mcMMO | A skill level named as `%mcmmo_level_<skill>%` in a line, read from mcMMO itself. |
| Floodgate, Geyser | Bedrock players get forms where a window would be a chest. |

The first start downloads the MariaDB and PostgreSQL drivers into the server's libraries folder, so it needs
access to Maven Central. A server that only uses SQLite still needs this once at start.

## Files

The plugin writes these into `plugins/uxmQuests/` on first start and never overwrites a file that exists.

| File | What it holds |
|---|---|
| `config.conf` | Language, storage, battle pass switches, window sounds, anti-glitch, feedback, sampling, rank permissions, overall milestones. |
| `commands.conf` | The name, aliases and on/off switch of every command and subcommand. |
| `groups/<id>.conf` | One group of quests. The file name is the id. `daily.conf` and `catalogue.conf` ship. |
| `passes/<id>.conf` | One battle pass. The file name is the id. `season.conf` ships. |
| `menus/<name>.conf` | One window: title, rows, slots, materials, sounds. `quests`, `board`, `passes` and `pass` ship. |
| `messages/messages_<tag>.conf` | Every sentence a player reads, one file per language. `en` and `tr` ship. |
| `languages.conf` | The language each player chose. Written by the plugin. |
| `data/quests.db` | The SQLite database, when the storage type is `sqlite`. |

The colours of every message live in the shared `plugins/uxmTheme/theme.conf`, which every UXPLIMA plugin on
the server reads. The first plugin to start writes it.

Every file explains its keys in comments. When a release adds a key to `config.conf`, the next start adds it
with its comment and keeps every value already written; the file as it was is kept as `config.conf.bak`.
The merge rewrites the file, so alignment inside a block may move. A line in a shipped `messages_<tag>.conf`
that nobody edited takes the wording of the new release.

## Reload

`/uxmquests reload` reads `config.conf`, the groups, the passes, the windows, the messages and the theme
again. A reload is applied as a whole or not at all:

- If a group file does not parse, the running groups stay as they were, the console names the file, and the
  reload says the groups were kept. `/uxmquests problems` lists what the last load could not read.
- `commands.conf` is read only at start. The server builds its command tree once.

`/uxmquests doctor` checks the storage and the windows and prints one line per check.

## Languages

The languages are the files in `messages/`. Write `messages_de.conf` and the server has German; nothing lists
them. A player reads, in this order: the language they chose with `/quests lang set <language>` (or an operator
chose for them with `/uxmquests lang set-player`), their client's language when `language.follow-client` is
on, and then `language.default`. `language.force` puts everybody on one language. A key a translation lacks falls back to
the default language.

A quest, group or pass names its words as `@some.key`, which is a key of `messages/`, so one quest reads in
every language the server has. Text without a leading `@` is shown as written, for a server with one language.

## Ranks

A permission carries a number. `uxmquests.count.6` is a board of six quests, and `uxmquests.count.daily.6` is
a board of six in the `daily` group only. `uxmquests.rerolls.3` is three rerolls per board. The highest number
a player holds wins, and the number in the group file is the floor. The prefixes are `ranks.board-size` and
`ranks.rerolls` in `config.conf`.

## Windows

`/quests` opens the window `menus/quests.conf`, and the board of one group opens from it. A server that
deletes the window gets the board in chat instead. Slots, materials, titles and sounds are all in the menu
files; `menu.phase-display` in `config.conf` picks one material per quest phase.

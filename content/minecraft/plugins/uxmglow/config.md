---
title: Configuration
order: 40
description: config.conf, the picker file, the language files and the palette.
icon: sliders
---

Four kinds of file, and each one owns a different decision. A value that changes behaviour is in
`config.conf`, a word a player reads is in a language file, a window is a file of its own, and a
colour of the interface is in the shared theme.

## config.conf

Read on enable and on `/uxmglow reload`, and swapped in whole, so a reload is never half applied.

| Path | Default | What it controls |
|---|---|---|
| `language.default` | `en` | What a player reads when nothing else answers |
| `language.follow-client` | `true` | Whether a player who chose nothing reads their client's language |
| `language.force` | empty | One language for everybody. Empty means none |
| `storage.type` | `sqlite` | Where the choices live: `sqlite`, `mysql`, `mariadb` or `postgresql`. Any other word stops the plugin |
| `storage.file` | `glow.db` | The SQLite file, inside the plugin folder |
| `storage.host` / `storage.port` | `localhost` / `0` | The database server. A port of 0 is the engine's own |
| `storage.database` | `minecraft` | The database on that server |
| `storage.username` / `storage.password` | `minecraft` / empty | The connection, for a database server |
| `storage.table-prefix` | `uxmglow_` | Written in front of every table, so two plugins can share one database |
| `storage.pool-size` | `4` | Connections in the pool. A SQLite file is opened with one writer whatever this says |
| `storage.sync-seconds` | `2` | How often this server reads the colours the other servers on the same database changed for a player who is here. Ignored for a SQLite file; 0 turns it off |
| `storage.local-only` | `false` | True keeps this one server on its SQLite file under the same prefix, whatever `storage.type` says: the creative or build server of a network that shares one database |
| `storage.copy-from-file` | `false` | True copies the rows of the SQLite file into the empty tables of the network database when the plugin starts, and the log says how many. Set it with the move off `sqlite`; tables that already hold rows are left alone |
| `cycle.tick-ms` | `100` | How often the one shared animation task wakes |
| `cycle.minimum-interval-ms` | `200` | The shortest step a player may ask for |
| `cycle.default-interval-ms` | `1000` | The step used when a player names no number |
| `cycle.maximum-colours` | `8` | How many colours one cycle may hold |

One task drives every cycling player, not one task per player. The cost of the feature is the
number of players who are cycling, and it does not grow with the number of colours they chose.

### Effects

The `effects` block decides what each moment looks and sounds like. Every moment is a list, and each
line of the list is one thing that happens, in the order you wrote them.

| Moment | When it fires |
|---|---|
| `effects.set` | A solid colour was turned on |
| `effects.cycling` | A cycle of colours was turned on |
| `effects.cleared` | The glow was turned off |
| `effects.restored` | The glow came back when the player logged in |
| `effects.lapsed` | The glow did not come back, because the colour is no longer theirs |

A line starts with what it does: `[message]`, `[broadcast]`, `[title]`, `[subtitle]`, `[actionbar]`,
`[bossbar]`, `[sound]`, `[particle]`, `[console]` or `[player]`. Text that starts with `@` names a line
of the language file, so the words stay in the language the player reads, for example
`"[message] @glow.set"`. A brace block says when and how often a line runs, for example
`"[sound] {chance=0.2} ui.button.click 1 1"`. Delete a moment to keep the shipped default, and write
it as `[]` for silence. The comment above the block in `config.conf` lists the arguments of every
verb.

When a release adds a key, the next start adds it to your file with the comment that explains
it, and it never changes a value you wrote. Your file as it was is kept beside it as
`config.conf.bak`.

## The picker

`menus/glow.conf`. The rows, the slots the sixteen colours are paged through, the material of
every square, the sounds a click plays, where the buttons sit, and which block of the language
file each tooltip reads. Nothing about the window is decided in the code.

Every colour ships as `tinted:LEATHER_CHESTPLATE`, which is dyed with the exact colour the
player will glow in. Leather is the only vanilla item that takes an exact value, so it is the
only material that can show sixteen colours as sixteen different squares. Name another material
and you get it untinted.

## The words

`messages/messages_en.conf` and `messages/messages_tr.conf` ship. The languages this plugin has
are the files in that folder and nothing counts them: write `messages_de.conf` and German exists
at the next reload.

Which language a player reads is decided in one order: the language `language.force` names, then
the player's own choice, then the language their client reports when `language.follow-client` is
on, then `language.default`. The choice is read from uxmLang when that plugin is on the server
and from this plugin's own `languages.conf` otherwise, so the chooser works either way.

## The palette

`theme.conf` sits beside the plugin folders rather than inside one, so every UXPLIMA plugin on
the server reads one palette and an operator styles the suite once. A colour of a message or a
menu comes from there, never from a hex value written into a sentence.

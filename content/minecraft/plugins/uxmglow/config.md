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
| `storage.jdbc-url` | empty | Where the choices live. Blank means a SQLite file in the plugin folder |
| `storage.username` / `storage.password` | empty | The connection, for a real database |
| `storage.pool-size` | `4` | Connections in the pool. A SQLite file is opened with one writer whatever this says |
| `cycle.tick-ms` | `100` | How often the one shared animation task wakes |
| `cycle.minimum-interval-ms` | `200` | The shortest step a player may ask for |
| `cycle.default-interval-ms` | `1000` | The step used when a player names no number |
| `cycle.maximum-colours` | `8` | How many colours one cycle may hold |

One task drives every cycling player, not one task per player. The cost of the feature is the
number of players who are cycling, and it does not grow with the number of colours they chose.

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

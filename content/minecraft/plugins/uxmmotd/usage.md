---
title: "Guide"
order: 10
description: "How uxmMOTD works, how to set it up and what each file holds."
---

## Install

- **One server:** put `uxmMOTD-paper-<version>.jar` in `plugins/` and start the server.
- **A network:** put `uxmMOTD-velocity-<version>.jar` in the proxy's `plugins/` and start the proxy. The
  proxy answers the ping, so the Paper jar is not needed on the servers behind it.

Nothing else is needed: uxmLib is inside each jar. The Paper jar needs Paper 26.3 or newer.

## Files

The plugin writes these files into its folder on first start (`plugins/uxmMOTD/` on Paper,
`plugins/uxmmotd/` on Velocity). The files are the same on both platforms.

| File | What it holds |
|---|---|
| `config.conf` | The server list entries, the hover lines, the player count, the version text, centring, icons, maintenance and the language settings. |
| `commands.conf` | The name, aliases and on/off switch of the command and each word under it. Read at start only. |
| `theme.conf` | The palette, the colour roles every line names, and the gradient wheel. |
| `messages/messages_<lang>.conf` | The text of every chat message and the maintenance kick screen, one file per language. |
| `icons/` | The two shipped icons, `uxm-mark.png` and `uxm-glow.png`, and any 64 by 64 PNG you add. |
| `maintenance.conf` | Whether maintenance is on. Written by the command; do not edit it while the server runs. |

`/uxmmotd reload` reads `config.conf`, `theme.conf`, the messages and the icons again. A reload is
applied as a whole or not at all: when a file is invalid the previous configuration stays in use and the
console holds the reason. `commands.conf` needs a restart.

When a release adds a key to `config.conf`, the next start adds it with its comment and keeps every value
already written; the file as it was is kept beside it as `config.conf.bak`.

## The server list entry

`motd.entries` is a list. Each entry has `line-1`, `line-2` and an optional `icon`. `motd.rotation`
chooses the next entry for each ping: `in-order` walks the list and starts again, `random` picks one.

Lines are MiniMessage. Name a colour role instead of a colour, for example `<value>`, `<good>`, `<warn>`,
`<bad>`, `<info>`, `<rank>`, `<accent>`, `<icon>` or `<subtext>`; the colours behind the names are in
`theme.conf`, so one edit there repaints every line. Start a line with `<center>` and it is measured in
font pixels and pushed to the middle of the box the client draws; `centring.box-width-pixels` is that box.
`<g:'NAME':wheel>` paints a name across the theme's wheel. English words are drawn in small capitals;
wrap a word in `<plain>` to keep ordinary letters, which an address needs. `<online>` and `<max>` become
the two numbers this ping reports.

`hover` is the list a client shows when the cursor rests on the player count. The protocol carries those
lines as player names, so a client draws a handful of them and no more.

`players.mode` decides the numbers beside the entry: `real` shows what the server reports, `fixed` always
shows `players.maximum`, and `just-one-more` shows a maximum one above the shown count so the list never
says the server is full. `players.offset` is added to the online count first and may be negative; the
count never goes below zero.

`version.enabled` replaces the protocol name with `version.text`. Leave it off unless you want it: the
text shows only when the client's protocol does not match, so `version.force-mismatch` sends protocol -1
and every client then marks the entry as incompatible, though they all can still join.

## Icons

An icon is a PNG of exactly 64 by 64 pixels in the plugin folder, named relative to it, for example
`icons/uxm-mark.png`. `icons.normal` is used when an entry names none. A file that is missing, unreadable
or the wrong size is reported once in the console and the ping is answered with the normal icon.

## Maintenance

`/uxmmotd maintenance on` closes the server: the list shows the `maintenance` entry (its own two lines,
hover, version text and icon, from `maintenance.*` in `config.conf`) and a player who is not exempt is
refused with the kick screen from the messages file. `off` opens it again and `status` says which it is.
The switch is saved in `maintenance.conf`, so a restart keeps it.

Who still gets in, by any one of three ways:

- `maintenance.exempt.uuids`: a player's UUID. It survives a name change, so prefer it.
- `maintenance.exempt.names`: a player's name, compared without case.
- The permission `uxmmotd.maintenance.bypass`, on a Velocity proxy only. A Paper server checks the player
  before the player exists, and no Paper event that runs then carries a permission, so there the two
  lists are the only way.

On Paper the kick screen is written in the server's default language, since there is no client yet.

## Languages

A viewer reads the language `language.force` names, else their client's language when
`language.follow-client` is on, else `language.default`. A key a translation has not reached falls back to
`language.default`. A new language is one more `messages/messages_<lang>.conf`. The text of the server
list entry is not in the message files: it is your own writing in `config.conf`.

## Troubleshooting

`/uxmmotd doctor` runs two checks and prints one line each: `content` (how many entries were read) and
`icons` (how many of the named icons could be read). A failed check names the cause; the console carries
the full reason for anything that went wrong at start.

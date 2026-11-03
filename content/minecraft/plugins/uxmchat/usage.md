---
title: "Guide"
order: 10
description: "How uxmChat works, how to set it up and what each file holds."
---

## One server

Put `uxmChat-<version>.jar` in `plugins/` and start the server. The plugin writes its files into
`plugins/uxmChat/` and runs with a SQLite file. Every feature is a module, listed with a line each under
`modules` in `config.conf`; turn one off there and `/uxmchat reload`.

| File | What it holds | Read |
|---|---|---|
| `config.conf` | storage, network, modules, chat rules, tokens, mentions, limits | every reload |
| `commands.conf` | the name, aliases and switch of every command | at start |
| `channels.conf` | the channels a new database starts with | once, then in game |
| `formats.conf` | how a line is drawn | every reload |
| `currencies.conf` | the currencies tags and colours are sold in | every reload |
| `modules/filters/filters.conf` | filters, the infraction ladder, automatic replies | every reload |
| `modules/filters/words/<language>.txt` | swear words, one file per language, one word per line | every reload |
| `modules/emojis/emojis.conf` | emoji | every reload |
| `modules/colours/colours.conf` | chat colours | every reload |
| `modules/tags/tags.conf` | tag categories and rules; the tags a new database starts with | every reload, tags once |
| `modules/announcements/announcements.conf` | the announcement bundles a new database starts with | once, then in game |
| `modules/events/events.conf` | join, quit, death, kick, advancement and network lines | every reload |
| `modules/custom-commands/custom-commands.conf` | commands of your own | at start |
| `modules/games/games.conf` | chat games: kinds, timing, rewards | every reload |
| `modules/games/words/<language>.txt` | the words of the word games, one file per language | every reload |
| `modules/games/trivia/<language>.conf` | the trivia bank, one file per language; a question keeps its id in each | every reload |
| `messages/` | every line a player reads, one file per language | every reload |
| `menus/` | the windows | every reload |

A folder from a release before the module folders moves on the first start: each file goes to its module
folder, `words_<language>.txt` goes to `modules/filters/words/`, and the words and trivia that `games.conf`
held are written into their language files (the old file stays as `games.conf.bak`). The console names
everything it moved. A new language is one new file in each word folder.

Channels, tags and announcements live in the database after the first start, so every server of a
network shares them; change them in game with `/chateditor`.

An announcement with `centred = true` draws each of its chat lines in the middle of the chat, measured with
the client's own font, so a title, a line or two of text and a link read as one framed notice. An empty line
in `lines` is a blank line of the notice.

An announcement also takes `enabled` and a `condition`: `permission:<node>`, `world:<name>`,
`gamemode:<mode>` or a placeholder against a value such as `%vault_eco_balance% >= 1000`, joined with `&&`
and `||` and turned round with `!`. A condition that does not read is heard by nobody. Lines may hold
PlaceholderAPI placeholders. A random bundle never says the same announcement twice in a row. How long a
title stays and the boss bar's colour, notches and time are under `announcements` in `config.conf`.

`/announcements` lists every announcement as `bundle/id`, `/announcements preview <bundle/id>` shows you one
as a player sees it, and `/announcements say <bundle/id>` says it now. `/chateditor announcement` edits each
announcement on its own: its lines, whether it is on and centred, where it shows, its sound, its condition,
permission and worlds, and who has played long enough to hear it.

## A network

1. Give every server one shared MySQL, MariaDB or PostgreSQL database in `storage`, and a `server-name`
   equal to its name in `velocity.toml`, in lower case.
2. Put `uxmChat-Velocity-<version>.jar` in the proxy's `plugins/` and start it once. It writes
   `plugins/uxmchat/secrets.conf` with one secret per server. Never put that file in git.
3. On each server set `network.mode = "direct"`, `network.host` and `network.port` to the proxy's
   broker, and `network.secret` to that server's line from `secrets.conf`. Restart.
4. For network join, switch and quit lines, set `network.enabled = true` in `modules/events/events.conf` on every server.
5. Run the same uxmChat version on every server and the proxy: what they send each other changes between
   versions, so update them together.

`redis` is the other mode: every server talks through a Redis server instead of the proxy's broker. Set
`network.mode = "redis"` and `network.redis-uri` on every server and in the proxy's
`plugins/uxmchat/config.conf`; the proxy joins Redis like a server and needs no secrets. Redis has no
per-server secret, so keep it closed to anything but the network.

## Discord

Put `uxmChat-Discord-<version>.jar` in `plugins/` beside uxmChat and start once. Fill in
`plugins/uxmChat-Discord/discord.conf`: a webhook per bridged channel posts lines as the player, and a
bot token brings Discord messages into the game and answers `/online` and `/playerlist`. Run the bot on
one server only. Keep the token and webhooks out of git; `env:NAME` reads one from an environment variable.

## Languages

A player reads the language their client is set to, or the one they chose with `/chat lang set`. English
and Turkish ship. Write `messages/messages_<tag>.conf` and the language exists; a key missing there falls
back to the line the code carries.

A tag, a tag category, a rarity, a channel name or a colour name may be a language key instead of text:
`display = "@custom.tag.vip"` reads the `custom.tag.vip` line of each messages file. Every reader sees it in
their own language, in chat, in menus and in replies. A chat line holding one is drawn once per language,
and a line sent to another server carries every language this server has.

## Tag and colour effects

The `effects` block of `modules/tags/tags.conf` and of `modules/colours/colours.conf` lists what happens when a player's tag or chat colour
is put on (`set`), taken off (`cleared`), or taken back at their join because they lost it or it is gone
(`lapsed`). Each list holds action lines run in order: `[message]`, `[title]`, `[actionbar]`, `[bossbar]`,
`[sound]`, `[particle]`, `[console]`, `[player]`, `[broadcast]` and more. A text that starts with `@` is that
line of the messages files in each reader's language, so the shipped `[message] @tag.worn` reads as before
and a sound or a title can go next to it. Leave a list out to keep the shipped one; write `[]` for silence.

## Moving from uxm-tags and uxm-chat-color

With `uxmTags` or `uxmChatColor` still in `plugins/`, the first start reads their tags, presets, worn tags,
grants and colour choices, and copies the language lines their tags name into `messages/`. Their files are
only read. Each import runs once; delete its `.imported-*` marker to run it again. Remove the old jars after,
and show `%rel_uxmchat_tag_mini%` wherever `%rel_uxmtags_tag%` was.

## uxmEssentials

uxmEssentials steps back from every chat feature uxmChat runs: chat format, private messages, social spy,
mail, helpop, the announcer, join and quit lines, broadcast, clear chat, `/me` and the chat toggle. A
feature turned off in uxmChat stays with uxmEssentials.

Mutes go the other way. When uxmEssentials runs its moderation module, or LiteBans, LibertyBans,
AdvancedBan or Essentials is installed, that plugin keeps `/mute` and `/unmute` with its records and
history: uxmChat registers neither and mutes through that plugin's command when the automatic
punishments call for it. It reads the plugin's mutes to keep muted players quiet, except LibertyBans', whose
own chat block does that.

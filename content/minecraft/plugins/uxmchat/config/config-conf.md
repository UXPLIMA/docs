---
title: config.conf
order: 501
description: Every key of config.conf, block by block, with its default and what it does.
icon: file-cog
---

Read at start and on every `/uxmchat reload`, except `storage` and `network`, which need a restart.

## language

| Key | Default | Meaning |
|---|---|---|
| `default` | `"en"` | What a player reads when nothing else answers, and the fallback for a key a translation has not reached |
| `follow-client` | `true` | A player who chose nothing reads the language their client is set to |
| `force` | `""` | One language for everybody, whatever they chose; empty for none |

## Server

| Key | Default | Meaning |
|---|---|---|
| `server-name` | `"server"` | This server's id on the network, in the chat log and the staff list. On a network, its name in `velocity.toml`, in lower case. Letters, digits, `_` and `-` |
| `time-zone` | `""` | The time zone `[time]` and the chat log show, such as `Europe/Istanbul`; empty uses the machine's own |

## storage

| Key | Default | Meaning |
|---|---|---|
| `type` | `"sqlite"` | `sqlite`, `mysql`, `mariadb` or `postgresql`. A network needs one of the last three |
| `file` | `"chat.db"` | The SQLite file, inside `data/` |
| `host`, `port` | `"localhost"`, `0` | The database server; port 0 is 3306 for MySQL and MariaDB, 5432 for PostgreSQL |
| `database`, `username`, `password` | `"minecraft"`, `"minecraft"`, `""` | Never put the file in git with a real password |
| `table-prefix` | `"uxmchat_"` | Before every table name |
| `pool-size` | `4` | Connections the pool may open; SQLite uses one |
| `local-only` | `false` | This one server keeps its own rows in the SQLite file |
| `copy-from-file` | `false` | Copies the SQLite file's rows into an empty network database at the next start |

See [Database](../database/overview.md).

## network

| Key | Default | Meaning |
|---|---|---|
| `mode` | `"none"` | `none`, `direct` (the proxy's broker) or `redis` |
| `host`, `port` | `"127.0.0.1"`, `25590` | Where the proxy's broker listens |
| `secret` | `""` | This server's line from the proxy's `secrets.conf`. Never put it in git |
| `redis-uri` | `"redis://127.0.0.1:6379"` | For `redis`; a password goes in as `redis://:pass@host:6379` |
| `proxy-online-mode` | `false` | The proxy runs in online mode while this server runs offline behind it |

See [A network](../features/network.md).

## modules

Every module is on unless written as `false`. A module turned off registers no listener, task, command or placeholder,
and a reload turns it on or off without a restart.

| Module | Default | Off means |
|---|---|---|
| `channels` | `true` | One public chat; no `/channel` |
| `filters` | `true` | No filter reads anything |
| `auto-replies` | `true` | No automatic answers |
| `mentions` | `true` | `@name` is plain text |
| `emojis` | `true` | Shortcuts stay as typed; no `/emoji` |
| `snapshots` | `true` | No `[inv]`, `[ender]`, `[shulker]` |
| `quotes` | `true` | No quote button after lines |
| `private-messages` | `true` | No `/msg`, `/reply`, conversations, quick replies, recent window |
| `ignore` | `true` | No `/ignore` |
| `friends` | `true` | No friends list |
| `mail` | `true` | No letters |
| `nicks` | `true` | No nicknames |
| `tags` | `true` | No tags |
| `colours` | `true` | No chat colours |
| `mutes` | `true` | No `/mute` of uxmChat's own |
| `punishment-plugins` | `true` | Mutes of other plugins are not read |
| `auth` | `true` | Chat does not wait for AuthMe, nLogin or LibreLogin |
| `ai` | `false` | No AI review |
| `helpop` | `true` | No `/helpop` |
| `spies` | `true` | No copies for staff |
| `staff-delete` | `true` | No remove button after lines |
| `staff-list` | `true` | No `/stafflist` |
| `macros` | `true` | No `/macro` |
| `polls` | `true` | No `/poll` and `/vote` |
| `rules` | `true` | Nobody accepts rules |
| `chat-log` | `true` | No chat log and no `/chatlog` |
| `announcements` | `true` | No announcements |
| `events` | `true` | The server's own join, quit and death lines |
| `network-events` | `true` | No network join, switch and quit lines or resent lines |
| `me` | `true` | No `/me` |
| `custom-commands` | `true` | No commands of your own |
| `bubbles` | `true` | No chat bubbles |
| `newcomer` | `true` | No newcomer badge or limits |
| `games` | `true` | No chat games |
| `editors` | `true` | No in-game editors |
| `network` | `true` | This server stays alone whatever `network.mode` says |

## announcements

| Key | Default | Meaning |
|---|---|---|
| `title.fade-in-ms`, `stay-ms`, `fade-out-ms` | `500`, `3000`, `500` | How a title appears, stays and goes |
| `boss-bar.colour` | `"blue"` | pink, blue, red, green, yellow, purple or white |
| `boss-bar.overlay` | `"progress"` | progress, notched-6, notched-10, notched-12 or notched-20 |
| `boss-bar.seconds` | `8` | How long the bar stays |

## chat

| Key | Default | Meaning |
|---|---|---|
| `model` | `"system"` | `system` or `signed`; see [Formats](../features/formats.md#small-touches) |
| `default-channel` | `"global"` | The channel a player speaks in until they pick one |
| `max-length` | `256` | The longest line; a channel can set its own; 0 has no limit |
| `nobody-heard` | `true` | Tell a player when nobody could read their line |
| `grammar` | `false` | Capital letter first, full stop last |
| `stretch-limit` | `3` | The most times a letter may repeat; 0 leaves lines alone |
| `tokens-per-line` | `8` | How many tokens one line may draw |

## Tokens and snapshots

`tokens.kinds` holds one block per token, `{ names = [...], permission = "..." }`: `item`, `ping`, `pos`, `time`,
`gametime`, `money`, `inv`, `ender`, `shulker`. See [Mentions and tokens](../features/mentions-and-tokens.md#tokens).

| Key | Default | Meaning |
|---|---|---|
| `tokens.commands` | `["/spawn", "/rules", "/discord"]` | Commands a player may put in a line as `[/command]` |
| `tokens.item-guard.max-bytes` | `8192` | An item over this is shown by name with no tooltip |
| `tokens.item-guard.blocked` | `["WRITTEN_BOOK", "WRITABLE_BOOK"]` | Kinds always shown by name |
| `snapshots.keep-minutes` | `30` | How long a snapshot can be opened |

## mentions

| Key | Default | Meaning |
|---|---|---|
| `style` | `"<accent><name></accent>"` | How a mention is drawn |
| `bare-names` | `false` | A name without `@` mentions too |
| `max-per-line` | `5` | The most players one line may mention |
| `cooldown-seconds` | `10` | Before mentioning the same player again |
| `sound` | `"minecraft:entity.experience_orb.pickup"` | What a mentioned player hears |

## World groups and time

```hocon
world-groups {
  survival = [ "world", "world_nether", "world_the_end" ]
}
```

Worlds that share world channels and local lines. A world in no group is its own group.

## newcomer

| Key | Default | Meaning |
|---|---|---|
| `playtime-minutes` | `0` | Less playtime than this makes a newcomer; 0 makes nobody one |
| `slowmode-seconds` | `10` | A newcomer's wait between two lines |
| `badge` | `"@format.newcomer"` | Before a newcomer's name; empty shows nothing |
| `mentions` | `1` | Players a newcomer may mention in one line |

## Private messages, friends, mail, quick replies, nicks

| Key | Default | Meaning |
|---|---|---|
| `private-messages.max-length` | `256` | The longest message |
| `private-messages.cooldown-millis` | `1000` | Between two messages of one player |
| `private-messages.offline-queue` | `20` | Messages waiting for one offline player |
| `friends.cap` | `100` | The most friends |
| `friends.request-days` | `7` | How long a request waits |
| `mail.inbox-cap` | `50` | The most letters one inbox holds |
| `mail.page-size` | `8` | Letters per page |
| `mail.retention-days` | `60` | How long a read letter is kept |
| `quick-replies.cap` | `9` | Quick replies per player |
| `nick.allowed` | `"[A-Za-z0-9_]+"` | What a nick may be made of |
| `nick.min-length`, `max-length` | `3`, `16` | |
| `nick.blocked` | `admin`, `owner`, `staff`, `moderator`, `console`, `server` | Nicks nobody may take |

## ai

| Key | Default | Meaning |
|---|---|---|
| `api-key` | `""` | The OpenRouter key; nothing is sent without one. Never put it in git |
| `model` | `""` | Such as `openai/gpt-4o-mini`; empty uses OpenRouter's default |
| `rules` | `"No insults, no hate, no threats, no sexual content."` | What the model is told the rules are |
| `per-minute` | `30` | The most lines reviewed per minute |
| `points` | `5` | Ladder points for a line against the rules |
| `min-length` | `4` | Shorter lines are not sent |

## Bubbles, newcomers, rules and staff

| Key | Default | Meaning |
|---|---|---|
| `bubbles.seconds` | `5` | How long a bubble stays |
| `bubbles.distance` | `16` | How far it is seen |
| `bubbles.max-length` | `64` | Longer lines are cut |
| `bubbles.format` | `"<white><message>"` | How it is drawn |
| `bubbles.groups` | none | A format per permission group |
| `bubbles.height` | `0.8` | Over the head, in blocks |
| `bubbles.background` | `"#40000000"` | `#AARRGGBB` |
| `bubbles.shadow` | `false` | |
| `rules.version` | `0` | Raise it to ask every player again; 0 asks nobody |
| `rules.lines` | three lines | The rules, MiniMessage allowed |
| `helpop.cooldown-seconds` | `60` | Between two questions |
| `macros` | `discord`, `rules` | `name = "line"`, said by `/macro <name>` |
| `spy.secret-commands` | `login`, `l`, `register`, `reg`, `changepassword`, `changepass`, `unregister` | Their words never show to a command spy |
| `chat-log.retention-days` | `30` | How long the log keeps a line |

## Menus and filtering surfaces

| Key | Default | Meaning |
|---|---|---|
| `filter-surfaces` | `false` | Signs, books and anvil names pass the chat filters too |
| `menu.sounds.click` | `block.note_block.pling`, 0.6, 1.5 | A button that does something |
| `menu.sounds.page` | `item.book.page_turn`, 0.7, 1.0 | A page arrow |
| `menu.sounds.denied` | `block.note_block.bass`, 0.6, 0.9 | A button the player may not use |

A sound is named by its vanilla key; an empty name silences it.

Related: [Configuration](index.md), [Modules](../getting-started/concepts.md#modules)

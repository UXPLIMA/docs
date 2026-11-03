---
title: "Guide"
order: 10
description: "How uxmJumpPads works, how to set it up and what each file holds."
---

## Install

Put `uxmJumpPads-<version>.jar` in `plugins/` and start the server. It needs nothing else: uxmLib is inside
the jar.

## Make a pad

There are two ways, and a server can use both.

**By block.** Every block that matches a pad type in `config.conf` is a pad. The shipped `classic` pad is a
light weighted pressure plate on a redstone block, and `bounce` is a slime block on an emerald block. Build
those in the world and they throw; the same plate on any other block stays a plate. A pad by block throws the
way the player faces.

**On one block.** Stand on the block (on a plate, or on top of a full block), face the way the pad should
throw and run `/uxmjumppads set <type>`. That block throws every player that way, whichever way they face, with
the speed and effects of the type. `/uxmjumppads remove` on the same block takes it off, and
`/uxmjumppads list` shows every pad set this way. They are kept in `pads.conf`.

A pad throws a player once each time they walk onto it. Landing back on the pad that threw them does not throw
them again until they have stood on other ground, and `cooldown-ms` keeps a pad from throwing the same player
twice in a moment.

## Configure

The plugin writes these files into `plugins/uxmJumpPads/` on first start:

| File | What it holds |
|---|---|
| `config.conf` | The pad types, the cooldown, the fall damage grace and the languages. |
| `commands.conf` | The name, aliases and on/off switch of every command and subcommand. |
| `pads.conf` | The pads set on single blocks. Written by `set` and `remove`; edit it only with the server stopped. |
| `messages/messages_<lang>.conf` | The text of every message, one file per language. |

A pad type has `block`, `under`, `forward` and `upward` speeds in blocks per tick (0 to 4), an optional
`permission`, and `effects`: action lines such as `[sound]`, `[particle]`, `[message]` or `[title]`, run in
order when the pad throws. The comments in `config.conf` explain each key.

`/uxmjumppads reload` reads `config.conf` and the messages again. A pad type that cannot be read is left out
and named in the console and in `/uxmjumppads doctor`; the others keep working. The pad types are your own
list: one you delete is not put back by the next start.

After a throw the first fall costs nothing for `fall-damage.grace-ms` milliseconds; the next fall is the
player's own. Set it to 0 to leave fall damage alone.

## Languages

A player reads the language they chose with `/jumppad lang set`, else their client's language when
`language.follow-client` is on, else `language.default`. `language.force` puts everybody on one language. A
new language is one more file in `messages/`.

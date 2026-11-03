---
title: "Guide"
order: 10
description: "How uxmJoinItems works, how to set it up and what each file holds."
---

uxmJoinItems hands players the items a lobby hotbar needs, such as a server selector or a cosmetics opener, when
they join, join for the first time, respawn or change world. Each item can be locked in place, limited to
WorldGuard regions, held back while another plugin says the player is busy, and runs actions on a left or right
click.

## Install

Put `uxmJoinItems-<version>.jar` in `plugins/` and start the server. The first start writes `config.conf`,
`commands.conf`, `theme.conf`, three items under `items/` (`selector`, `cosmetics`, `lobby`), one file per event
under `events/`, and the message files into `plugins/uxmJoinItems/`. `/uxmjoinitems reload` reads them again.

## Items

Each `items/<id>.conf` is one item; the file name is its id (lower-case letters, digits, `-` and `_`).

| Key | Meaning |
|---|---|
| `material` | The only required key. An unknown name becomes STONE and says so once in the console. |
| `slot` | 0 to 8 the hotbar, 9 to 35 the inventory, 36 to 39 armour, 40 the off hand. |
| `name`, `lore` | MiniMessage. A single space as the name shows nothing; `@key` reads a line from `messages/`. |
| `custom-model-data`, `glow`, `unbreakable` | The look. `-1` keeps the ordinary model. |
| `triggers` | Which events hand it out: `join`, `first-join`, `respawn`, `world-change`. Empty: only `/uxmjoinitems give`. |
| `cooldown` | How long before a click works again. Left out, `items.default-cooldown`. `"0s"` for none. |
| `restrictions.locked` | The item cannot be moved, dragged, swapped, dropped or placed, and is kept out of death drops. |
| `skip-when` | PlaceholderAPI texts that hold the item back while any of them answers something. |
| `regions` | WorldGuard regions it is given in, matched without case. Empty: everywhere. |
| `left-click`, `right-click` | Actions a click runs. |

An item is not given when its slot is already taken. A `skip-when` answer of nothing, `0`, `-`, `false` or `no`
means "not busy", and a placeholder no plugin fills is passed over.

Click actions use the shared action lines (`[message]`, `[title]`, `[sound]`, `[console]`, `[player]` and the rest)
plus two of this plugin's own: `[server] <name>` sends the player through the proxy, and `[menu] <command>` closes
the open screen and runs the command. Holding the button down runs the actions once per cooldown.

## Events

Each `events/<event>.conf` holds `enabled`, `delay` and `actions` for one event. A switched off event hands out
nothing. The actions run after the items are placed; `first-join` runs beside `join`, not instead of it.

## Configuration

| Key | Default | Meaning |
|---|---|---|
| `items.default-cooldown` | `"500ms"` | The cooldown of an item whose file names none. |
| `network.proxy-channel` | `"BungeeCord"` | The channel `[server]` writes on. BungeeCord, Waterfall and Velocity all read it. |

## Commands

| Command | Permission | What it does |
|---|---|---|
| `/uxmjoinitems` |  | Administer the join items. |
| `/uxmjoinitems give <player> <item>` | `uxmjoinitems.admin.give` | Give one join item to a player. |
| `/uxmjoinitems giveall <player>` | `uxmjoinitems.admin.giveall` | Give a player every join item they may hold. |
| `/uxmjoinitems clear <player>` | `uxmjoinitems.admin.clear` | Take every join item off a player. |
| `/uxmjoinitems reload` | `uxmjoinitems.admin.reload` | Read the configuration and the content again. |
| `/uxmjoinitems version` | `uxmjoinitems.admin.version` | Read the version this server runs. |
| `/uxmjoinitems doctor` | `uxmjoinitems.admin.doctor` | Say how this plugin is, in one line per thing that can be wrong. |
| `/uxmjoinitems lang` | `uxmjoinitems.language` | Say what language this player reads, and what else this server has. Players only. |
| `/uxmjoinitems lang set <language>` | `uxmjoinitems.language` | Read this plugin in one language, whatever the client is set to. Players only. |
| `/uxmjoinitems lang reset` | `uxmjoinitems.language` | Follow the client again, as a player who never chose anything does. Players only. |
| `/uxmjoinitems lang set-player <player> <language>` | `uxmjoinitems.admin.language` | Set the language another player reads. |
| `/uxmjoinitems lang reset-player <player>` | `uxmjoinitems.admin.language` | Clear the language another player chose. |

All nodes and their defaults are in [reference.md](reference.md).

## Examples

**A selector only at spawn.** Set `regions = ["spawn"]` in `items/selector.conf`.

**Hide the hotbar during a parkour run.** Set `skip-when = ["%uxmparkour_arena%"]`: while the placeholder names an
arena, the item is not handed out.

**Send players to another server.** `right-click = ["[server] survival"]`.

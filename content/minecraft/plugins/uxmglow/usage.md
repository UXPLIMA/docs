---
title: "Guide"
order: 10
description: "How uxmGlow works, how to set it up and what each file holds."
---

uxmGlow lets players glow in one of the 16 vanilla team colours, or cycle through several. The choice is kept in
the plugin's database, comes back when the player joins, and is dropped when the player no longer holds the colour.

## Install

Put `uxmGlow-<version>.jar` in `plugins/` and start the server. The first start writes `config.conf`,
`commands.conf`, `theme.conf`, `menus/glow.conf` and the message files into `plugins/uxmGlow/`. Players open the
picker with `/glow`. `/uxmglow reload` reads the files again.

## Colours and permissions

The colours are the vanilla ones: `black`, `dark_blue`, `dark_green`, `dark_aqua`, `dark_red`, `dark_purple`, `gold`,
`gray`, `dark_gray`, `blue`, `green`, `aqua`, `red`, `light_purple`, `yellow` and `white`. Each needs
`uxmglow.colour.<colour>`; `uxmglow.colour.*` grants them all. A cycle needs `uxmglow.cycle` and the permission of
every colour in it.

A player who loses a colour, or the cycle permission, keeps glowing until they leave. On their next join the stored
glow is forgotten and the `lapsed` effects run.

## Configuration

| Key | Default | Meaning |
|---|---|---|
| `cycle.tick-ms` | `250` | How often the one cycle task runs. It is also the finest step a cycle can take. |
| `cycle.minimum-interval-ms` | `250` | The shortest step a player may ask for. |
| `cycle.default-interval-ms` | `1000` | The step when `/glow cycle` is given no number. |
| `cycle.maximum-colours` | `8` | How many colours one cycle may hold. A cycle needs at least two. |
| `effects.set`, `cycling`, `cleared`, `restored`, `lapsed` | a message, and a sound on the first three | What happens at each moment, as `[verb] payload` lines. `[]` makes one silent. |
| `storage` | SQLite | Where glows are kept: `sqlite`, `mysql`, `mariadb` or `postgresql`. |

The picker window is drawn from `menus/glow.conf`: its rows, slots, materials, sounds and buttons.

## Commands

| Command | Permission | What it does |
|---|---|---|
| `/uxmglow` |  | Administer the plugin. |
| `/uxmglow reload` | `uxmglow.admin.reload` | Read the configuration and the content again. |
| `/uxmglow version` | `uxmglow.admin.version` | Read the version this server runs. |
| `/uxmglow doctor` | `uxmglow.admin.doctor` | Say how this plugin is, in one line per thing that can be wrong. |
| `/glow` | `uxmglow.use` | Choose the colour you glow in. |
| `/glow set <colour>` |  | Glow in one colour. Players only. |
| `/glow cycle <colours> [interval-ms]` |  | Glow through several colours. Players only. |
| `/glow off` |  | Stop glowing. Players only. |
| `/glow colours` |  | List the colours this player holds. Players only. |
| `/glow lang` | `uxmglow.language` | Say what language this player reads, and what else this server has. Players only. |
| `/glow lang set <language>` | `uxmglow.language` | Read this plugin in one language, whatever the client is set to. Players only. |
| `/glow lang reset` | `uxmglow.language` | Follow the client again, as a player who never chose anything does. Players only. |
| `/glow lang set-player <player> <language>` | `uxmglow.admin.language` | Set the language another player reads. |
| `/glow lang reset-player <player>` | `uxmglow.admin.language` | Clear the language another player chose. |

The `/glow` subcommands need `uxmglow.use`, which every player has. All nodes and their defaults are in
[reference.md](reference.md).

## Placeholders

| Placeholder | Value |
|---|---|
| `%uxmglow_glowing%` | `true` or `false`. |
| `%uxmglow_mode%` | `solid`, `cycle`, or empty. |
| `%uxmglow_colour%` | The readable name, or empty. |
| `%uxmglow_colour_id%` | The id, or empty. |
| `%uxmglow_tag%` | The MiniMessage tag of the colour, or empty. |

## Examples

**Gold for donors.** Grant `uxmglow.colour.gold` to the donor group. They pick it in `/glow` or with `/glow set gold`.

**A rainbow for staff.** Grant `uxmglow.cycle` and `uxmglow.colour.*`; staff run
`/glow cycle red,gold,yellow,green,aqua,blue 500`.

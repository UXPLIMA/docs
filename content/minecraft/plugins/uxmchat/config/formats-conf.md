---
title: formats.conf
order: 604
description: The default format, the group order, group formats and the seven shipped formats.
icon: brush
---

Read at start and on every reload.

| Key | Default | Meaning |
|---|---|---|
| `default` | `"default"` | The format a line takes when nothing more specific names one |
| `group-order` | `owner`, `admin`, `moderator`, `helper`, `vip`, `default` | When a player is in several groups, the first one listed decides |
| `group-formats` | none | A format per group, for channels that name none: `vip = "vip"` |
| `formats` | seven formats | Each a list of `parts` |

A part is `{ name, template, hover, click }`. The placeholders, clicks and the order in which a format is chosen are
in [Formats](../features/formats.md).

| Format | Draws |
|---|---|
| `default` | `<tag><newcomer><prefix><display>: <message>` |
| `global` | `[G] ` (hover, click to switch), then as default with the server in the name's hover |
| `local` | `[L] `, no tag, the message in white |
| `world` | `[<world>] `, no tag |
| `trade` | gold `[Trade] `, no tag or prefix |
| `help` | aqua `[Help] `, no tag |
| `staff` | `[Staff] ` in the bad colour, the name's hover shows the server |

Related: [Formats](../features/formats.md), [channels.conf](channels-conf.md)

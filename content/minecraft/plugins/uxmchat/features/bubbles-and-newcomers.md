---
title: Bubbles and newcomers
order: 417
description: Words over a player's head, and the badge and limits for players new to the server.
icon: message-circle
---

## Chat bubbles

Module: `bubbles`. A player's line floats over their head for players near them who turned bubbles on in their
settings.

| Key under `bubbles` | Default | Meaning |
|---|---|---|
| `seconds` | `5` | How long a bubble stays |
| `distance` | `16` | How far away it is seen, in blocks |
| `max-length` | `64` | Longer lines are cut |
| `format` | `<white><message>` | How it is drawn; an `@key` is a line of the reader's language |
| `groups` | none | A format per permission group, over the one above |
| `height` | `0.8` | How far over the top of the head it starts; raise it for tall nametags |
| `background` | `#40000000` | The colour behind the words, `#AARRGGBB`; `#00000000` for none |
| `shadow` | `false` | A shadow under the words |

## Newcomers

Module: `newcomer`. A player with less playtime on this server than `newcomer.playtime-minutes` is a newcomer.

| Key under `newcomer` | Default | Meaning |
|---|---|---|
| `playtime-minutes` | `0` | 0 makes nobody a newcomer |
| `slowmode-seconds` | `10` | The longest a newcomer waits between two lines, over any channel's own slowmode |
| `badge` | `@format.newcomer` | Before a newcomer's name in chat, through `<newcomer>`: a green `[New]` |
| `mentions` | `1` | How many players a newcomer may mention in one line |

Related: [config.conf](../config/config-conf.md#bubbles-newcomers-rules-and-staff)

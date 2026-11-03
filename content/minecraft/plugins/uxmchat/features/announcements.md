---
title: Announcements
order: 312
description: Bundles of announcements on chat, action bar, title, boss bar or sound, with conditions, centring and playtime gates.
icon: megaphone
---

Module: `announcements`. A bundle says its announcements one after another, every `interval-seconds`.

## The shipped bundles

| Bundle | Every | Order | Announcements |
|---|---|---|---|
| `tips` | 300 s | sequential | `discord`, `rules`, `channels` (from 5 minutes of playtime), `settings`, all in chat |
| `welcome` | 60 s | sequential | `newcomer`: on the action bar, once per visit |

## A bundle

| Key | Meaning |
|---|---|
| `interval-seconds` | The time between two announcements of the bundle |
| `order` | `sequential`, or `random`; a random bundle never says the same announcement twice in a row |
| `from`, `until` | A time of day, such as `"18:00"`, between which the bundle speaks; empty for all day |

## An announcement

| Key | Meaning |
|---|---|
| `lines` | MiniMessage lines; a line that starts with `@` is a line of the messages files in each reader's language; an empty line is a blank line |
| `surfaces` | `chat`, `action-bar`, `title`, `boss-bar`, `sound` |
| `sound` | For the sound surface, such as `minecraft:block.note_block.bell` |
| `permission` | Who hears it; empty is everyone |
| `worlds` | The worlds it is heard in; empty is every world |
| `min-online` | Said only with at least this many players online |
| `min-playtime-minutes` | Heard only by players with this much playtime |
| `once-per-session` | Heard once by each player per visit |
| `centred` | Each chat line drawn in the middle of the chat |
| `enabled` | `false` keeps it with its lines and never says it |
| `condition` | Who hears it, tested for each player |

A line may hold PlaceholderAPI placeholders, filled in for each player who reads it. A player who turned announcements
off in their settings hears none.

## Centred announcements

With `centred = true`, each chat line is drawn in the middle of the chat, measured with the client's own font, so a
title, a line or two of text and a link read as one framed notice.

## Conditions

| Test | Example |
|---|---|
| A permission | `permission:uxmchat.vip` |
| A world | `world:hub` |
| A game mode | `gamemode:survival` |
| A placeholder against a value | `%vault_eco_balance% >= 1000` |

Join tests with `&&` (all) and `||` (any), and turn one round with `!`: `permission:uxmchat.vip && !world:pvp`. A
condition that does not read is heard by nobody.

## The other surfaces

How long a title stays and the boss bar's colour, notches and time are under `announcements` in
[config.conf](../config/config-conf.md#announcements):

| Key | Default |
|---|---|
| `title.fade-in-ms`, `stay-ms`, `fade-out-ms` | `500`, `3000`, `500` |
| `boss-bar.colour` | `blue`: pink, blue, red, green, yellow, purple or white |
| `boss-bar.overlay` | `progress`, or `notched-6`, `notched-10`, `notched-12`, `notched-20` |
| `boss-bar.seconds` | `8` |

## Managing them

`announcements.conf` is read once into the database. After that:

| Command | Does |
|---|---|
| `/announcements` | Lists every announcement as `bundle/id`, where it shows and whether it is on |
| `/announcements preview <bundle/id>` | Shows you one as a player sees it |
| `/announcements say <bundle/id>` | Says it now |
| `/chateditor announcements` | The bundles: interval, order, hours, announcements |
| `/chateditor announcement` | Each announcement with every field above |

Related: [announcements.conf](../config/announcements-conf.md), [In-game editors](editors.md)

---
title: Outcomes
order: 305
description: The eight outcomes of a move, the action lines each runs, the verbs and the placeholders.
icon: message-circle
---

Every move ends in one of eight outcomes, and `effects` in [config.conf](../config/config-conf.md#effects) holds a list
of action lines for each, run in order.

| Outcome | When | Shipped |
|---|---|---|
| `connecting` | A move is under way | A chat line, an action bar line, an enderman teleport sound |
| `already-there` | The player is on that lobby already | A chat line, a low note |
| `unreachable` | The lobby is not answering | A chat line, a low note |
| `full` | The lobby is full | A chat line, a low note |
| `not-permitted` | The player may not join it | A chat line, a low note |
| `cooldown` | The player moved too recently | An action bar line, a hat sound |
| `no-such-lobby` | No lobby has that name | A chat line |
| `nothing-available` | `/quicklobby` found no lobby with room | A chat line, a low note |

## Verbs

| Verb | Payload |
|---|---|
| `[message]` | A chat line |
| `[actionbar]` | A line over the hotbar |
| `[subtitle]` | `<title> \| <subtitle> <fade-in> <stay> <fade-out>`, the times in seconds |
| `[sound]` | `<key> <volume> <pitch>` |
| `[particle]` | `<name> <count> <spread> [data]`, the data a colour `#ff0000`, a block or an item |
| `[console]` | A command run by the server |
| `[player]` | A command run by the player |

Text that starts with `@` names a line of the messages files, so it reads in the player's language: each outcome has
`chat`, `title`, `subtitle` and `action-bar` lines ready, such as `@full.chat` and `@connecting.title`. An empty list
silences the outcome.

## Placeholders

| Placeholder | In |
|---|---|
| `<lobby>` | Every outcome that names a lobby |
| `<players>`, `<maximum>` | `full` |
| `<seconds>` | `cooldown` |
| `<name>` | `no-such-lobby`: what the player typed |

A file that still carries the old `feedback` block keeps working: it is read as the list it is equivalent to. Write the
lists to keep what you have, because the block goes away.

## Example

A title and a firework sound when a player is on the way:

```hocon
connecting = [
  "[subtitle] @connecting.title | @connecting.subtitle 0.2 1.5 0.3",
  "[sound] entity.firework_rocket.launch 0.6 1.0"
]
```

Related: [Messages](../config/messages.md), [Moving](moving.md)

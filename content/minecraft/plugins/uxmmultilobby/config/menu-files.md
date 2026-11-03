---
title: Menu file
order: 504
description: The keys of menus/lobbies.conf, the tile line, the tokens, the condition and the actions.
icon: layout-template
---

`menus/lobbies.conf` is the window: its rows, the slots the lobbies are paged through, the panes, the buttons, the
sounds and the words on every tile. Delete it and there is no window: `/lobby` answers `menu.missing`.

| Key | Meaning |
|---|---|
| `title` | `@menu.title`, or your own words |
| `rows` | `3` |
| `open-actions` | A page-turn sound |
| `items` | `lobbies` (the list, 10 to 16), `filler` (panes, 0 to 26), `previous` (21), `next` (23), and a commented `best` (22) |

```hocon
lobbies {
  slots = ["10-16"]
  priority = 5
  list {
    source = "lobby:lobbies"
    template {
      material = "%lobby_icon%"
      name = " "
      lore = ["tile:%lobby_colour% @menu.lobby %lobby_players% %lobby_status% action:@menu.action.%lobby_action%"]
      decor { glow = "%lobby_current%" }
      click {
        left {
          requirements = ["lobby:permitted"]
          click = ["lobby:join", "sound:BLOCK_NOTE_BLOCK_PLING 0.6 1.5"]
          deny  = ["lobby:refuse", "sound:BLOCK_NOTE_BLOCK_BASS 0.6 0.9"]
        }
      }
    }
  }
}
```

The number of slots in `lobbies` is the number of lobbies one page holds.

## The tile line

`tile:<colour> @<block> <facts>` draws the house tooltip from a block of the messages files. A word written as
`action:@<key>` names the catalogue line the closing sentence comes from, and a `%token%` inside it is filled first:
that is how one tile says a different thing about the click in each state.

## Tokens

| Token | Value |
|---|---|
| `%lobby%` | The lobby's name, as the catalogue writes it |
| `%lobby_id%` | Its id |
| `%lobby_icon%` | Its `icon` |
| `%lobby_colour%` | Its `colour` |
| `%lobby_current%` | `true` on the lobby the viewer is on |
| `%lobby_players%` | Names the player count fact; nothing for a lobby that has told nothing |
| `%lobby_status%` | Names the status fact: `online`, `full`, `offline` or `unknown` |
| `%lobby_load%` | `current`, `open`, `busy`, `full` or `closed` |
| `%lobby_load_icon%` | The material `menu.load-icons` gives that load |
| `%lobby_action%` | `join`, `current`, `locked`, `full`, `offline` or `unknown`: a line under `menu.action` |

## Condition and actions

| Name | Does |
|---|---|
| `lobby:lobbies` | The list source |
| `lobby:permitted` | A condition: the viewer may enter the lobby in the square |
| `lobby:join` | Sends the viewer to the clicked lobby |
| `lobby:refuse` | Says why that lobby is not theirs |
| `lobby:best` | Sends them wherever there is the most room |

Every window also knows `close`, `open:<menu>`, `command:<line>`, `message:<line>` and
`sound:<name> <volume> <pitch>`.

Related: [Lobby window](../menus/lobbies.md), [Messages](messages.md)

---
title: "Guide"
order: 10
description: "How uxmPlayerHider works, how to set it up and what each file holds."
---

uxmPlayerHider gives every player a hotbar item and a command that choose how much of the crowd they see: everybody, only the important players, or nobody. The choice is kept between sessions and only applies in the worlds you list.

## Install

1. Put the jar in `plugins/`.
2. Start the server once. The plugin writes its files into `plugins/uxmPlayerHider/`.
3. Name your lobby worlds in `worlds` in `config.conf`, then run `/uxmplayerhider reload`.

PlaceholderAPI is optional. When it is installed the plugin registers its placeholders.

## Files

| File | What it holds |
|---|---|
| `config.conf` | storage, worlds, cooldown, the hotbar tool and the effect of each state |
| `commands.conf` | the name, aliases and on or off switch of every command |
| `messages/messages_<language>.conf` | the words players read, including the tool's name and lore per state |
| `theme.conf` | the colours of the messages |

A reload reads every file again and swaps them in as one snapshot.

## The three states

| State | The player sees |
|---|---|
| `all` | every player |
| `important` | only players with `uxmplayerhider.important` |
| `none` | nobody |

Using the tool or typing `/hide` moves to the next state: `all`, then `important`, then `none`, then `all` again. `/hide state <state>` goes straight to one. A player who has chosen nothing sees everybody.

## Worlds

`worlds` lists the worlds where the tool and the command work, matched without case. An empty list means every world.

```hocon
worlds = ["lobby", "hub"]
```

Outside those worlds the player sees everybody and the command refuses to change the state, but the choice is kept: walking back into a listed world restores it.

## Cooldown

`cooldown.ms` is the shortest gap between two changes, in milliseconds. `0` allows a change on every click. A player with `uxmplayerhider.cooldown.bypass` never waits. A change refused because of the world does not start the cooldown.

## The hotbar tool

| `tool` key | Meaning |
|---|---|
| `enabled` | hand out the item; the command works either way |
| `slot` | the hotbar slot, 0 to 8; if it is taken the tool goes in the first free slot |
| `lock-in-slot` | stop the player moving, dropping or off-handing it |
| `give-on-join`, `give-on-respawn`, `give-on-world-change` | when it is handed out again |
| `select-on-give` | make it the held item when it is handed out |
| `icons.<state>` | the `material`, `glow` and `custom-model-data` (`0` for none) shown in each state |

The tool's name and lore are in the messages file under `tool.<state>.name` and `tool.<state>.lore`.

## Effects

`feedback.effects` holds a list of actions for each state, run when the player moves to it. Each line is `[verb] payload`, run in order; an empty list is silence and stays empty.

The verbs are `[message]`, `[broadcast]`, `[actionbar]`, `[title]`, `[subtitle]`, `[bossbar]`, `[particle]`, `[sound]`, `[effect]`, `[console]` and `[player]`. Each may carry `{delay=, chance=, repeat=, every=, if=}` after the verb. A `[subtitle]` line is `<title> | <subtitle> <in> <stay> <out>`, the times in seconds. Text that starts with `@` names a line of the messages file.

```hocon
feedback {
  effects {
    none = [
      "[subtitle] @state.none.title | @state.none.subtitle 0.1 1.2 0.3",
      "[sound] block.note_block.bass 0.7 0.8",
      "[message] @state.none.chat"
    ]
  }
}
```

## Storage

`storage.type` is `sqlite` (the default, in `data/playerhider.db`), `mysql`, `mariadb` or `postgresql`. For a database server fill `host`, `port` (`0` is the default port), `database`, `username` and `password`; every server pointed at it shares the choices. `table-prefix` lets several plugins share one database, `pool-size` caps the connections, and `sync-seconds` is how often a server reads what other servers changed for a player who is here (`0` turns it off).

`local-only = true` keeps one server of a network on its own SQLite file. To move from SQLite to a database server, set the new type with `copy-from-file = true` and restart once; rows are copied only into empty tables.

## Commands

| Command | Permission | What it does |
|---|---|---|
| `/hide` | `uxmplayerhider.use` | move to the next state |
| `/hide state <all\|important\|none>` | `uxmplayerhider.use` | move to a named state |
| `/hide status` | `uxmplayerhider.use` | say the current state and how many players it hides |
| `/hide lang`, `lang set <language>`, `lang reset` | `uxmplayerhider.language` | your own language |
| `/hide lang set-player <player> <language>`, `lang reset-player <player>` | `uxmplayerhider.admin.language` | another player's language |
| `/uxmplayerhider reload` | `uxmplayerhider.admin.reload` | read every file again |
| `/uxmplayerhider version` | `uxmplayerhider.admin.version` | show the running version |
| `/uxmplayerhider doctor` | `uxmplayerhider.admin.doctor` | report the plugin's health |

The names and aliases come from `commands.conf`. The [reference](reference.md) lists the default aliases and every permission.

## Placeholders

| Placeholder | Value |
|---|---|
| `%uxmplayerhider_state%` | `all`, `important` or `none`, as it applies in the player's world |
| `%uxmplayerhider_hiding%` | `true` when anybody is hidden |
| `%uxmplayerhider_hidden%` | how many online players are hidden right now |

The values are the ids, not translated words, so a condition that compares them works in every language.

## Troubleshooting

- **The tool does nothing in a world.** The world is not in `worlds`; an empty list allows every world.
- **Staff disappear in `important`.** They need `uxmplayerhider.important`.
- **A player can toggle as fast as they click.** `cooldown.ms` is `0`, or they hold `uxmplayerhider.cooldown.bypass`, which operators have by default.

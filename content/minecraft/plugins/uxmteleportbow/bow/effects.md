---
title: Effects
order: 405
description: The nine moments of the bow, what each does by default, how an action line is written, and the values it can use.
icon: sparkles
---

Every moment of the bow runs a list of action lines from the `effects` block of `config.conf`, in the order written.
Write as many lines as you like; an empty list, `shot = []`, is silence.

## The moments

| Key | Runs when | Values | Ships as |
|---|---|---|---|
| `shot` | The arrow leaves the bow | | `[sound] entity.arrow.shoot 1 1.4` |
| `departure` | Just before the player is moved, where they stand | | `[particle] portal 40 0.4` |
| `arrival` | Just after the player is moved, where they now stand | | `[particle] reverse_portal 40 0.4`, `[sound] entity.enderman.teleport 1 1` |
| `refused-cooldown` | A shot inside the cooldown | `<seconds>` | `[message] @refused.cooldown`, `[sound] block.note_block.bass 1 0.7` |
| `refused-world` | A shot in a world the filter closes | `<world>` | `[message] @refused.world`, `[sound] block.note_block.didgeridoo 1 0.7` |
| `refused-departure` | The player stands in a region the filter closes | | `[message] @refused.departure`, the didgeridoo sound |
| `refused-destination` | The arrow landed in a region the filter closes | | `[message] @refused.destination`, the didgeridoo sound |
| `refused-void` | The arrow stopped below `void-floor-y` | | `[message] @refused.void`, the didgeridoo sound |
| `refused-headroom` | No room to stand where the arrow stopped | | `[message] @refused.headroom`, the didgeridoo sound |

`<seconds>` is the cooldown left with one decimal, such as `2.4`; `<world>` is the world's name. A key left out of
the file runs what it ships as; a key written as an empty list runs nothing.

The six refusals are six lists because they say six different things. A server that wants one answer to all of them
writes the same list six times.

## Writing a line

A line is `[verb] payload`. Text that starts with `@` names a line of the [messages files](../config/messages.md),
read in the player's language, with the moment's values filled in. Any other text is your own, MiniMessage with the
[theme](../config/theme.md) roles, and goes through as written: `<seconds>` and `<world>` are filled in `@` lines
only.

| Verb | Payload | Does |
|---|---|---|
| `[message]` | `<text>` | Sends the text to the player |
| `[broadcast]` | `<text>` | Sends the text to everybody on the server; an `@` line is read by each in their own language |
| `[actionbar]` | `<text>` | Shows the text over the hotbar |
| `[title]` | `<text>` | Shows the text across the middle of the screen |
| `[subtitle]` | `<title> \| <subtitle> [in stay out]` | A title with a line under it. The fade in, stay and fade out are in seconds, `0.5 3.5 1` when left out |
| `[bossbar]` | `<seconds> [colour] [overlay] [drain] \| <text>` | A bar across the top of the screen for that long |
| `[sound]` | `<key> [volume] [pitch]` | Plays a sound to the player alone, vanilla or from a resource pack |
| `[effect]` | `<effect> <seconds> [level] [hidden]` | Gives a potion effect, such as `slow_falling 3`; level 1 is the plain effect |
| `[particle]` | `<name> [count] [spread] [data]` | Particles where the player stands; data is a colour `#ff0000`, a block or an item |
| `[console]` | `<command>` | Runs a command as the server, without the slash |
| `[player]` | `<command>` | Runs a command as the player |
| `[close]` | | Closes whatever menu the player has open |
| `[take-money]`, `[give-money]` | `[economy] <amount>` | Takes from or pays into an economy. uxmTeleportBow reaches Vault only |
| `[take-item]` | `<item> [amount]` | Takes items from the player's inventory |

`%player_name%` in a `[console]` or `[player]` line is the player's name. A line that cannot be read is skipped with a
warning in the console, and the lines after it still run. A `[take-money]` or `[take-item]` the player cannot pay
skips that line quietly.

A sound is heard by the player the moment happened to, not by the room, and a particle is drawn where that player
stands. A lobby that wants the room to know writes a `[broadcast]`.

## When and how often

A brace block right after the verb says when a line runs:

```hocon
"[sound] {delay=10} entity.player.levelup 1 1"
"[particle] {repeat=5, every=2} end_rod 4 0.2"
"[message] {chance=0.05} <accent>Whoosh!"
```

| Key | Meaning |
|---|---|
| `delay` | Ticks before the line runs; 20 ticks is one second |
| `repeat`, `every` | How many times it runs, and the ticks between |
| `chance` | The probability it runs, from `0` to `1` |
| `if` | A condition, `<left> <operator> <right>`, with `==`, `!=`, `>=`, `<=`, `>`, `<` or `?=`; a side may be a PlaceholderAPI placeholder, or `%mcmmo_level_<skill>%` read from mcMMO itself |

## Examples

A title on landing, and a slow fall for a long drop:

```hocon
arrival = [
  "[particle] reverse_portal 40 0.4"
  "[sound] entity.enderman.teleport 1 1"
  "[subtitle] <accent>Whoosh | <body>You are there 0.2 1 0.3"
  "[effect] slow_falling 2 1 hidden"
]
```

The refusals over the hotbar instead of in chat:

```hocon
refused-headroom = [
  "[actionbar] @refused.headroom"
  "[sound] block.note_block.didgeridoo 1 0.7"
]
```

A silent bow:

```hocon
shot = []
arrival = []
```

Related: [Shooting](shooting.md), [Messages](../config/messages.md)

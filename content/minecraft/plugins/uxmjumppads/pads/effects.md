---
title: Effects
order: 404
description: The action lines a pad runs as it throws, and how a line is written.
icon: sparkles
---

Each pad type has an `effects` list, run in order every time a pad of that type throws. It is how a pad sounds,
looks and says anything.

```hocon
effects = [
  "[sound] entity.wind_charge.wind_burst 1 1.2"
  "[particle] cloud 20 0.3"
]
```

An empty list, `effects = []`, makes a silent pad. Effects carry no values of their own.

## Writing a line

A line is `[verb] payload`. Text that starts with `@` names a line of the [messages files](../config/messages.md),
read in the player's language; any other text is MiniMessage with the [theme](../config/theme.md) roles.

| Verb | Payload | Does |
|---|---|---|
| `[message]` | `<text>` | Sends the text to the thrown player |
| `[broadcast]` | `<text>` | Sends the text to everybody on the server |
| `[actionbar]` | `<text>` | Shows the text over the hotbar |
| `[title]` | `<text>` | Shows the text across the middle of the screen |
| `[subtitle]` | `<title> \| <subtitle> [in stay out]` | A title with a line under it. The fade in, stay and fade out are in seconds, `0.5 3.5 1` when left out |
| `[bossbar]` | `<seconds> [colour] [overlay] [drain] \| <text>` | A bar across the top of the screen for that long |
| `[sound]` | `<key> [volume] [pitch]` | Plays a sound to the player, vanilla or from a resource pack |
| `[effect]` | `<effect> <seconds> [level] [hidden]` | Gives a potion effect, such as `slow_falling 5`; level 1 is the plain effect |
| `[particle]` | `<name> [count] [spread] [data]` | Particles where the player stands; data is a colour `#ff0000`, a block or an item |
| `[console]` | `<command>` | Runs a command as the server, without the slash |
| `[player]` | `<command>` | Runs a command as the player |
| `[close]` | | Closes whatever menu the player has open |
| `[take-money]`, `[give-money]` | `[economy] <amount>` | Takes from or pays into an economy. uxmJumpPads reaches Vault only |
| `[take-item]` | `<item> [amount]` | Takes items from the player's inventory |

`%player_name%` in a `[console]` or `[player]` line is the player's name. A line that cannot be read is skipped with
a warning in the console.

## When and how often

A brace block right after the verb says when a line runs:

```hocon
"[sound] {delay=10} entity.player.levelup 1 1"
"[particle] {repeat=5, every=2} end_rod 4 0.2"
"[message] {chance=0.05} <accent>Whee!"
```

| Key | Meaning |
|---|---|
| `delay` | Ticks before the line runs; 20 ticks is one second |
| `repeat`, `every` | How many times it runs, and the ticks between |
| `chance` | The probability it runs, from `0` to `1` |
| `if` | A condition, `<left> <operator> <right>`, with `==`, `!=`, `>=`, `<=`, `>`, `<` or `?=`; a side may be a PlaceholderAPI placeholder |

## Examples

A pad that also slows the fall:

```hocon
effects = [
  "[sound] entity.wind_charge.wind_burst 1 1.2"
  "[effect] slow_falling 3 1 hidden"
]
```

A trail of particles along the flight:

```hocon
effects = [
  "[particle] {repeat=10, every=2} cloud 3 0.1"
]
```

Related: [Pad types](types.md), [Messages](../config/messages.md)

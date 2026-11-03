---
title: Effects
order: 404
description: What happens when a player moves into a state, the shipped lines, and how an action line is written.
icon: sparkles
---

When a player moves into a state, the list for that state under `feedback.effects` in `config.conf` runs, in the
order written. Write as many lines as you like; an empty list is silence, and stays empty through updates.

## The shipped lists

| State | Lines |
|---|---|
| `all` | `[subtitle] @state.all.title \| @state.all.subtitle 0.1 1.2 0.3`, `[actionbar] @state.all.action-bar`, `[sound] block.note_block.pling 0.7 1.5` |
| `important` | The same with `important`, and the pling at pitch `1.2` |
| `none` | The same with `none`, and `block.note_block.bass 0.7 0.8` |

Each list also carries a chat line commented out, `# , "[message] @state.<state>.chat"`. Take the `#` off to send
it. The words are in the messages files under `state.<state>`:

| Key | `all` | `important` | `none` |
|---|---|---|---|
| `title` | `Everybody` | `Only the people who matter` | `Nobody` |
| `subtitle` | `You can see the whole lobby.` | `Staff and friends stay visible.` | `The lobby is yours alone.` |
| `action-bar` | `Everybody is visible.` | `Only staff and friends are visible.` | `Everybody is hidden.` |
| `chat` | `You can see everybody again.` | `The crowd is hidden. Staff and friends stay.` | `Everybody is hidden.` |

## Writing a line

A line is `[verb] payload`. Text that starts with `@` names a line of the [messages files](../config/messages.md),
read in the player's language. Any other text is your own, MiniMessage with the [theme](../config/theme.md) roles.
A state's effects carry no values of their own.

| Verb | Payload | Does |
|---|---|---|
| `[message]` | `<text>` | Sends the text to the player |
| `[broadcast]` | `<text>` | Sends the text to everybody on the server; an `@` line is read by each in their own language |
| `[actionbar]` | `<text>` | Shows the text over the hotbar |
| `[title]` | `<text>` | Shows the text across the middle of the screen |
| `[subtitle]` | `<title> \| <subtitle> [in stay out]` | A title with a line under it. The fade in, stay and fade out are in seconds, `0.5 3.5 1` when left out |
| `[bossbar]` | `<seconds> [colour] [overlay] [drain] \| <text>` | A bar across the top of the screen for that long |
| `[sound]` | `<key> [volume] [pitch]` | Plays a sound to the player alone, vanilla or from a resource pack |
| `[effect]` | `<effect> <seconds> [level] [hidden]` | Gives a potion effect; level 1 is the plain effect |
| `[particle]` | `<name> [count] [spread] [data]` | Particles where the player stands; data is a colour `#ff0000`, a block or an item |
| `[console]` | `<command>` | Runs a command as the server, without the slash |
| `[player]` | `<command>` | Runs a command as the player |
| `[close]` | | Closes whatever menu the player has open |
| `[take-money]`, `[give-money]` | `[economy] <amount>` | Takes from or pays into an economy. uxmPlayerHider reaches Vault only |
| `[take-item]` | `<item> [amount]` | Takes items from the player's inventory |

`%player_name%` in a `[console]` or `[player]` line is the player's name. A line that cannot be read is skipped with a
warning in the console, and the lines after it still run. A `[take-money]` or `[take-item]` the player cannot pay
skips that line quietly.

## When and how often

A brace block right after the verb says when a line runs:

| Key | Meaning |
|---|---|
| `delay` | Ticks before the line runs; 20 ticks is one second |
| `repeat`, `every` | How many times it runs, and the ticks between |
| `chance` | The probability it runs, from `0` to `1` |
| `if` | A condition, `<left> <operator> <right>`, with `==`, `!=`, `>=`, `<=`, `>`, `<` or `?=`; a side may be a PlaceholderAPI placeholder, or `%mcmmo_level_<skill>%` read from mcMMO itself |

## Examples

Only the action bar, no title:

```hocon
feedback {
  effects {
    none = [
      "[actionbar] @state.none.action-bar"
      "[sound] block.note_block.bass 0.7 0.8"
    ]
  }
}
```

The chat line turned on:

```hocon
all = [
  "[subtitle] @state.all.title | @state.all.subtitle 0.1 1.2 0.3"
  "[actionbar] @state.all.action-bar"
  "[sound] block.note_block.pling 0.7 1.5"
  "[message] @state.all.chat"
]
```

Related: [States](states.md), [Messages](../config/messages.md)

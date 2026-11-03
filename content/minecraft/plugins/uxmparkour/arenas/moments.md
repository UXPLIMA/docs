---
title: Moments
order: 306
description: The lines a fall, a full arena, a milestone, a broadcast and a personal best run, and how a line is written.
icon: sparkles
---

A moment is a list of lines that runs when something happens in a run: a message, a title, a sound, a particle, a
command, or nothing at all. The five moments live in the `effects` block of
[`config.conf`](../config/config-conf.md#effects). Every line runs, in the order written.

## The five moments

| Moment | Runs when | Values its `@` lines read | Shipped |
|---|---|---|---|
| `fell` | The player fell out of the run | `<jumps>` | `@run.fell`, a villager "no", a cloud |
| `exhausted` | The arena ran out of room and the run ended | `<jumps>` | `@run.exhausted` |
| `milestone` | A milestone was reached and paid | `<jumps>`, `<reward>` | `@milestone.reached`, a level-up sound, green sparkles |
| `broadcast` | A milestone with `broadcast = true` was reached; runs for the whole server | `<player>`, `<jumps>` | `@milestone.broadcast` |
| `personal-best` | A run that fell or ran out of room beat the player's best | `<jumps>`, `<best>` | `@run.personal-best`, a level-up sound |

```hocon
effects {
  fell = [
    "[message] @run.fell",
    "[sound] entity.villager.no 0.8 1.0",
    "[particle] cloud 12 0.4"
  ]
  milestone = [
    "[message] @milestone.reached",
    "[title] @milestone.reached",
    "[sound] entity.player.levelup 0.7 1.2"
  ]
}
```

An empty list, `fell = []`, makes a moment silent. A run that ends any other way, by `/parkour leave`, a restart or
a disconnect, plays no moment; the player gets one line from the messages file.

## Writing a line

A line is `[verb] payload`. Text in the payload that starts with `@` names a line of the
[messages files](../config/messages.md), so the words stay in the language each player reads and you only decide
where they arrive. Any other text is written as you wrote it, in MiniMessage, with the
[theme](../config/theme.md) roles such as `<value>` and `<accent>`. The values of a moment, such as `<jumps>`,
are filled into its `@` lines; to show a value in a line of your own, add the line to the messages files and name
it with `@`.

| Verb | Payload | Does |
|---|---|---|
| `[message]` | `<text>` | Sends the text to the player |
| `[broadcast]` | `<text>` | Sends the text to everyone online, each in their own language |
| `[actionbar]` | `<text>` | Shows the text over the hotbar |
| `[title]` | `<text>` | Shows the text across the middle of the screen |
| `[subtitle]` | `<title> \| <subtitle> [in stay out]` | A title with a line under it, each half its own text or `@key`, the times in ticks |
| `[bossbar]` | `<seconds> [colour] [overlay] [drain] \| <text>` | A bar across the top of the screen for that long |
| `[sound]` | `<key> [volume] [pitch]` | Plays a vanilla sound to the player, such as `entity.player.levelup` |
| `[particle]` | `<name> [count] [spread] [data]` | A puff of particles where the player stands; data is a colour `#ff0000`, a block or an item |
| `[console]` | `<command>` | Runs a command as the server, without the slash |
| `[player]` | `<command>` | Runs a command as the player |
| `[close]` | | Closes whatever menu the player has open |
| `[take-money]`, `[give-money]` | `[economy] <amount>` | Takes from or pays into an economy, named as in [Rewards](rewards.md#the-money-lines) |
| `[take-item]` | `<item> [amount]` | Takes items from the player's inventory |

`%player_name%` in a `[console]` or `[player]` line is the player's name. A `[take-money]` or `[take-item]` the
player cannot afford stops that line only; the rest of the list still runs.

## When and how often

A brace block right after the verb says when a line runs. Every part is optional:

```hocon
"[message] {delay=40} <accent>Two seconds later"
"[sound] {repeat=3, every=5} ui.button.click 1 1"
"[console] {chance=0.1} give %player_name% diamond 1"
"[broadcast] {if=%player_level% >= 30} <accent>A veteran reached a milestone"
```

| Key | Meaning |
|---|---|
| `delay` | Ticks before the line runs; 20 ticks is one second |
| `repeat` | How many times it runs, at least one |
| `every` | Ticks between repeats |
| `chance` | The probability it runs, from `0` to `1` |
| `if` | A condition that must hold, as `<left> <operator> <right>` |

The operators of `if` are `==`, `!=`, `>=`, `<=`, `>`, `<` and `?=` (contains). A side may be a
PlaceholderAPI placeholder, `%mcmmo_level_<skill>%` with mcMMO installed (read from mcMMO itself, without
PlaceholderAPI), a number or a word.

## The cues that are not moments

The jump sound and the particles of a block appearing or vanishing fire several times a second, so they are fixed
settings under `feedback`, not moments: see [config.conf](../config/config-conf.md#feedback). The jump sound rises
in pitch with every jump of a streak.

## Examples

A milestone that puts the count on the screen and pays a crate key through another plugin:

```hocon
milestone = [
  "[subtitle] @milestone.reached | <accent>Keep going 10 40 10",
  "[sound] entity.player.levelup 0.7 1.2",
  "[console] crates key give %player_name% parkour 1"
]
```

Because there is one `milestone` list for every milestone, a `[console]` line runs at each of them.

A fall that only plays a sound:

```hocon
fell = ["[sound] entity.villager.no 0.8 1.0"]
```

Related: [Rewards](rewards.md), [Messages](../config/messages.md), [config.conf](../config/config-conf.md#effects)

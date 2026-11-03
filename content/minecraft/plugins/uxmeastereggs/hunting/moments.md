---
title: Moments
order: 305
description: The seven moments of a hunt, the effects of a type, the values each reads, and how a line is written.
icon: sparkles
---

A moment is a list of lines that runs when something happens in the hunt. Seven of them live in the `effects` block
of [`config.conf`](../config/config-conf.md#effects); what one egg does when it is found lives in the `effects` list of
its own [type file](../config/type-files.md#effects). Every line runs, in the order written.

## What runs on a find

1. The type's `effects`.
2. `progress`, on a first find, while `find.announce-progress` is on.
3. `milestone`, for each milestone the find crossed, and `milestone-broadcast` for those with `broadcast = true`.
4. `completion`, when the find completed the map, and `completion-broadcast` while `completion.broadcast` is on.

## The moments

| Moment | Runs when | Values | Shipped |
|---|---|---|---|
| a type's `effects` | A player finds an egg of that type, first or repeat | `<player>`, `<type>`, `<found>`, `<total>` | depends on the type |
| `progress` | After a first find | `<found>`, `<total>` | `@egg.progress` |
| `milestone` | A milestone was reached | `<count>` | `@milestone.reached`, a level-up sound |
| `milestone-broadcast` | A milestone with `broadcast = true` was reached | `<player>`, `<count>` | `[broadcast] @milestone.broadcast` |
| `completion` | The player found every egg | none | `@completion.reached`, a challenge-complete sound |
| `completion-broadcast` | The same, while `completion.broadcast` is on | `<player>` | `[broadcast] @completion.broadcast` |
| `radar-on`, `radar-off` | `/eggs radar` | none | `@radar.on`, `@radar.off` |

`<found>` and `<total>` are the player's count and the eggs on the map. `<type>` is the type's display name, in the
reader's language. An empty list, `progress = []`, makes a moment silent; a moment left out of the file runs its shipped
lines.

## Writing a line

A line is `[verb] payload`. Text in the payload that starts with `@` names a line of the
[messages files](../config/messages.md), so the words stay in the language each player reads; an `@key` may carry
values after it, such as `@egg.found type=gold`. Any other text is written as you wrote it, in MiniMessage, with the
[theme](../config/theme.md) roles such as `<value>` and `<good>`. The values of a moment, such as `<found>`, are filled into its `@` lines only,
not into words you write in the list itself.

| Verb | Payload | Does |
|---|---|---|
| `[message]` | `<text>` | Sends the text to the finder |
| `[broadcast]` | `<text>` | Sends the text to everybody on the server, each in their own language |
| `[actionbar]` | `<text>` | Shows the text over the hotbar |
| `[title]` | `<text>` | Shows the text across the middle of the screen |
| `[subtitle]` | `<title> \| <subtitle> [in stay out]` | A title with a line under it, each half its own text or `@key`. The fade in, stay and fade out are in seconds, `0.5 3.5 1` when left out |
| `[bossbar]` | `<seconds> [colour] [overlay] [drain] \| <text>` | A bar across the top of the screen for that long |
| `[sound]` | `<key> [volume] [pitch]` | Plays a sound, vanilla or from a resource pack |
| `[effect]` | `<effect> <seconds> [level] [hidden]` | Gives a potion effect, such as `speed 10 2`; level 1 is the plain effect |
| `[particle]` | `<name> [count] [spread] [data]` | A puff of particles where the player stands; data is a colour `#ff0000`, a block or an item |
| `[console]` | `<command>` | Runs a command as the server, without the slash |
| `[player]` | `<command>` | Runs a command as the player |
| `[close]` | | Closes whatever menu the player has open |
| `[take-money]`, `[give-money]` | `[economy] <amount>` | Takes from or pays into an economy, named as in [Rewards](rewards.md#the-money-lines) |
| `[take-item]` | `<item> [amount]` | Takes items from the player's inventory |

## When and how often

A brace block right after the verb says when a line runs. Every part is optional:

```hocon
"[message] {delay=40} <accent>Keep going"
"[sound] {repeat=3, every=5} ui.button.click 1 1"
"[console] {chance=0.1} give %player_name% diamond 1"
```

| Key | Meaning |
|---|---|
| `delay` | Ticks before the line runs; 20 ticks is one second |
| `repeat` | How many times it runs, at least one |
| `every` | Ticks between repeats |
| `chance` | The probability it runs, from `0` to `1` |
| `if` | A condition, as `<left> <operator> <right>`, with `==`, `!=`, `>=`, `<=`, `>`, `<` or `?=` (contains) |

A side of a condition may be a PlaceholderAPI placeholder, `%mcmmo_level_<skill>%` with mcMMO installed, a number or
a word.

## Example

A golden egg that sets off a firework of particles and tells the server. The values of a moment reach only `@`
lines, so the broadcast is a line of the messages files: add
`type.golden.found-by = "<tag:'EGGS'> <value><player><body> found a golden egg."` under `type` in every language,
then:

```hocon
effects = [
  "[sound] entity.player.levelup 1.0 1.4",
  "[message] @egg.found",
  "[subtitle] @type.golden.name | @type.golden.hint 0.3 2.5 0.5",
  "[particle] firework 30 0.5",
  "[broadcast] @type.golden.found-by"
]
```

Related: [Rewards](rewards.md), [Type files](../config/type-files.md), [Messages](../config/messages.md)

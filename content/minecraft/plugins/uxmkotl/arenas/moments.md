---
title: Moments
order: 305
description: The eight moments of an arena, who each one reaches, the values its lines read, and how a line is written.
icon: sparkles
---

A moment is a list of lines that runs when something happens in an arena: a message, a title, a sound, a particle,
a command, or nothing at all. The eight moments live in the `effects` block of
[`config.conf`](../config/config-conf.md#effects). Every line runs, in the order written.

## The eight moments

| Moment | Runs when | For | `[broadcast]` reaches | Values | Shipped |
|---|---|---|---|---|---|
| `crowned` | A player takes the crown | The new king | Everybody in the arena | `<arena>`, `<king>` | `@crown.claimed`, `@crown.announce` to the arena, a level-up sound |
| `dethroned` | A reign ends, except by death or quit | The old king | The arena when they left it; only them when they stepped off or lost the plate to a claim | `<time>` | `@crown.lost` |
| `claim-blocked` | A claim is refused because the king is protected | The refused player | Everybody in the arena | `<time>`, the protection left | `@crown.blocked` |
| `knocked-off` | A kill is credited | The killer | Only the killer | `<player>`, the victim | `@kill.credited` |
| `fell` | A player in the arena dies | The player | Only them | none | `@kill.died`, a villager "no" |
| `paid` | A reward money line paid | The player paid | Only them | `<amount>`, such as `10.00 vault` | `@reward.paid` |
| `joined` | A player gets in, by walking or by command | The player | Everybody in the arena, them included | `<arena>` | `@arena.joined` |
| `left` | A player leaves by command or by stepping out | The player | The players still in the arena | `<arena>` | `@arena.left` |

`<arena>` is the arena's display name. A quit, a stop of the server and a `reset` play no moment.

```hocon
effects {
  crowned = [
    "[message] @crown.claimed",
    "[broadcast] @crown.announce",
    "[sound] entity.player.levelup 0.8 1.2"
  ]
  fell = [
    "[message] @kill.died",
    "[sound] entity.villager.no 0.7 1.0"
  ]
}
```

An empty list, `fell = []`, makes a moment silent. A moment you delete from the file comes back with its shipped
lines at the next start.

## Writing a line

A line is `[verb] payload`. Text in the payload that starts with `@` names a line of the
[messages files](../config/messages.md), so the words stay in the language each player reads and you only decide
where they arrive. Any other text is written as you wrote it, in MiniMessage, with the [theme](../config/theme.md)
roles such as `<value>` and `<accent>`. The values of a moment, such as `<king>`, are filled into its `@` lines; to
show a value in a line of your own, add the line to the messages files and name it with `@`.

| Verb | Payload | Does |
|---|---|---|
| `[message]` | `<text>` | Sends the text to the player the moment is for |
| `[broadcast]` | `<text>` | Sends the text to the players in the table above, each in their own language. Never the whole server |
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
player cannot afford stops that line only; the rest of the list still runs. A line that cannot be read is skipped
with a warning in the console.

## When and how often

A brace block right after the verb says when a line runs. Every part is optional:

```hocon
"[message] {delay=40} <accent>Still king"
"[sound] {repeat=3, every=5} ui.button.click 1 1"
"[console] {chance=0.1} give %player_name% diamond 1"
"[broadcast] {if=%player_level% >= 30} <accent>A veteran took the crown"
```

| Key | Meaning |
|---|---|
| `delay` | Ticks before the line runs; 20 ticks is one second |
| `repeat` | How many times it runs, at least one |
| `every` | Ticks between repeats |
| `chance` | The probability it runs, from `0` to `1` |
| `if` | A condition that must hold, as `<left> <operator> <right>` |

The operators of `if` are `==`, `!=`, `>=`, `<=`, `>`, `<` and `?=` (contains). A side may be a PlaceholderAPI
placeholder, `%mcmmo_level_<skill>%` with mcMMO installed (read from mcMMO itself, without PlaceholderAPI), a number
or a word.

## Examples

A claim announced to the whole arena on the screen, with the king's name:

```hocon
crowned = [
  "[message] @crown.claimed",
  "[broadcast] @crown.announce",
  "[title] @crown.claimed",
  "[sound] entity.player.levelup 0.8 1.2"
]
```

A kill that also plays a sound to the killer and drops a firework of particles:

```hocon
knocked-off = [
  "[message] @kill.credited",
  "[sound] entity.experience_orb.pickup 0.8 1.4",
  "[particle] firework 20 0.4"
]
```

A silent join:

```hocon
joined = []
```

Related: [Rewards](rewards.md), [Messages](../config/messages.md), [config.conf](../config/config-conf.md#effects)

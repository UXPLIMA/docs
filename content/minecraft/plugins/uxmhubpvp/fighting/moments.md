---
title: Moments
order: 406
description: The ten moments of a fight, the values each reads, and how a line is written.
icon: sparkles
---

A moment is a list of lines that runs when something happens in a fight: a message, a title, a sound, a particle, a
command, or nothing at all. The ten moments live in the `effects` block of
[`config.conf`](../config/config-conf.md#effects). Every line runs, in the order written.

## The ten moments

| Moment | Runs when | For | Values | Shipped |
|---|---|---|---|---|
| `warm-up-entering` | The warm-up in starts | The player | `<seconds>` | `@pvp.warm-up-enter` |
| `warm-up-leaving` | The warm-up out starts | The player | `<seconds>` | `@pvp.warm-up-leave` |
| `warm-up-cancelled` | A warm-up stopped by a second press | The player | none | `@pvp.warm-up-cancelled` |
| `warm-up-moved` | A warm-up stopped because the player moved | The player | none | `@pvp.warm-up-moved` |
| `pvp-on` | The kit is given | The player | none | `@pvp.on` |
| `pvp-off` | The fight ends by the toggle, a death or a stop of the server | The player | none | `@pvp.off` |
| `combat-tagged` | A fighter is newly tagged | Each newly tagged player | `<seconds>` | `@combat.tagged` |
| `kill` | A kill is credited | The killer | `<victim>`, `<streak>` | `@reward.kill`, a level-up sound |
| `kill-paid` | Every line of the kill reward paid | The killer | `<coins>` | `@reward.kill-coins` |
| `death` | A fighter is killed by a player | The victim | `<killer>` | `@reward.death` |

A quit plays no moment. A death with no killer plays none either.

```hocon
effects {
  pvp-on = [
    "[message] @pvp.on",
    "[title] @pvp.on",
    "[sound] item.armor.equip_iron 1.0 1.0"
  ]
  death = []
}
```

An empty list, `death = []`, makes a moment silent. A moment left out of the file runs its shipped lines.

## Writing a line

A line is `[verb] payload`. Text in the payload that starts with `@` names a line of the
[messages files](../config/messages.md), so the words stay in the language each player reads. Any other text is
written as you wrote it, in MiniMessage, with the [theme](../config/theme.md) roles such as `<value>` and `<accent>`.
The values of a moment are filled into its `@` lines; to show a value in a line of your own, add the line to the
messages files and name it with `@`.

| Verb | Payload | Does |
|---|---|---|
| `[message]` | `<text>` | Sends the text to the player the moment is for |
| `[broadcast]` | `<text>` | Sends the text to everybody on the server, each in their own language |
| `[actionbar]` | `<text>` | Shows the text over the hotbar |
| `[title]` | `<text>` | Shows the text across the middle of the screen |
| `[subtitle]` | `<title> \| <subtitle> [in stay out]` | A title with a line under it, each half its own text or `@key`. The fade in, stay and fade out are in seconds, `0.5 3.5 1` when left out |
| `[bossbar]` | `<seconds> [colour] [overlay] [drain] \| <text>` | A bar across the top of the screen for that long |
| `[sound]` | `<key> [volume] [pitch]` | Plays a vanilla sound to the player |
| `[effect]` | `<effect> <seconds> [level] [hidden]` | Gives a potion effect, such as `speed 10 2`; level 1 is the plain effect, `hidden` hides the swirls |
| `[particle]` | `<name> [count] [spread] [data]` | A puff of particles where the player stands; data is a colour `#ff0000`, a block or an item |
| `[console]` | `<command>` | Runs a command as the server, without the slash |
| `[player]` | `<command>` | Runs a command as the player |
| `[close]` | | Closes whatever menu the player has open |
| `[take-money]`, `[give-money]` | `[economy] <amount>` | Takes from or pays into an economy, named as in [Rewards](rewards.md#the-money-lines) |
| `[take-item]` | `<item> [amount]` | Takes items from the player's inventory |

`%player_name%` in a `[console]` or `[player]` line is the player's name. A `[player]` command is run by the plugin,
so the [combat tag](combat-tag.md) list does not stop it.

## When and how often

A brace block right after the verb says when a line runs. Every part is optional:

```hocon
"[message] {delay=40} <accent>Still fighting"
"[sound] {repeat=3, every=5} ui.button.click 1 1"
"[console] {chance=0.1} give %player_name% golden_apple 1"
"[broadcast] {if=%uxmhubpvp_streak% >= 5} <accent>A fighter is on a streak of five"
```

| Key | Meaning |
|---|---|
| `delay` | Ticks before the line runs; 20 ticks is one second |
| `repeat` | How many times it runs, at least one |
| `every` | Ticks between repeats |
| `chance` | The probability it runs, from `0` to `1` |
| `if` | A condition that must hold, as `<left> <operator> <right>` |

The operators of `if` are `==`, `!=`, `>=`, `<=`, `>`, `<` and `?=` (contains). A side may be a PlaceholderAPI
placeholder, `%mcmmo_level_<skill>%` with mcMMO installed, a number or a word.

## The kill sound of older files

An older `config.conf` set the kill sound with `visuals.kill-sound`, `kill-sound-volume` and `kill-sound-pitch`. A file
that still carries them keeps them: the sound is added to the end of the `kill` moment.

## The cues that are not moments

The glow lasts as long as the fight and a hit happens several times a second, so neither is a moment; both are under
`visuals`. See [Turning PvP on and off](overview.md#the-glow-and-the-hit-particles).

Related: [Rewards](rewards.md), [Messages](../config/messages.md), [config.conf](../config/config-conf.md#effects)

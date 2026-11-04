---
title: Behaviours
order: 1090
description: Every behaviour a region can set, its values, and what it does to the players inside.
icon: sliders-horizontal
---

Contents: [Setting one](#setting-one) · [Messages](#messages) · [Actions on the way in and out](#actions-on-the-way-in-and-out) ·
[Player state](#player-state) · [Comfort](#comfort) · [Lists](#lists) · [Elsewhere](#elsewhere)

## Setting one

```text
/guard set <region> <behaviour> <value>
/guard set <region> <behaviour> clear
```

Each behaviour takes one kind of value, and a value that does not fit is refused when you type it, with the
reason. Where regions overlap, the first region in [precedence order](rules.md#across-regions) that sets a
behaviour decides it. Behaviours are applied when a player enters and taken back when they leave, quit, or the
server stops: guard remembers what it changed (flight, speed, game mode, god mode, effects, time, weather) and
restores exactly that.

Value words: a yes/no behaviour takes `true` or `false` (also `on`, `off`, `yes`, `no`, `evet`, `hayir`,
`acik`, `kapali`). A three-way behaviour takes `allow`, `deny` or `keep` (also `izin`, `yasak`, `koru`);
`keep` leaves the player's own setting alone. A list is comma separated.

## Messages

| Behaviour | Value | What it does |
|---|---|---|
| `message.enter` | text | Shown on the way in. MiniMessage, `{player}` for the player's name, and PlaceholderAPI placeholders when it is installed |
| `message.exit` | text | Shown on the way out, the same way |
| `message.channel` | `chat`, `title`, `actionbar`, `bossbar` | Where the two messages go. `chat` by default. For `title`, write `title|subtitle`; a `bossbar` stays up while the player is inside |
| `enter.deny-message` | text | Replaces the refusal shown when `move.enter` or `move.teleport-in` turns a player away |
| `exit.deny-message` | text | Replaces the refusal when `move.exit` holds a player in |
| `enter.push-back` | yes/no | `true` by default: a refused player is put back where they last stood outside. `false` only stops them |
| `deny-messages` | yes/no | `false` keeps every refusal in this region silent: no action bar, no chat line |

## Actions on the way in and out

| Behaviour | Value | What it does |
|---|---|---|
| `actions.enter` | actions | Run once when a player comes in |
| `actions.exit` | actions | Run once when they leave |
| `actions.interval` | actions | Run again and again while they stay |
| `actions.interval-seconds` | a whole number | How often `actions.interval` runs |

Actions are the click actions NPCs and holograms run, written `type: value` and separated by ` | `:

```text
/guard set arena actions.enter title: <red>Arena|<gray>no teleporting out | sound: ENTITY_WITHER_SPAWN:0.6:1
/guard set mine actions.interval chance: 5 | give: diamond
```

| Type | Value |
|---|---|
| `message` | A chat line |
| `actionbar` | An action bar line |
| `title` | `title|subtitle|fadeIn|stay|fadeOut`, every part after the title optional |
| `sound` | `SOUND_KEY[:volume[:pitch]]` |
| `player` | A command the player runs |
| `player_op` | A command the player runs with operator rights for that one command |
| `console` | A command the console runs |
| `connect` | A server name on your proxy to send the player to |
| `delay` | Ticks to wait before the rest |
| `random` | `n`: run one of the next `n` actions, picked at random |
| `chance` | A percent; on a miss the rest is skipped |
| `permission` | A node; without it the rest is skipped |
| `condition` | `<left> <op> <right>` with placeholders; when false the rest is skipped |
| `cost` | An amount taken from the player's balance; if they cannot pay the rest is skipped |
| `give` | `material[:amount]` |

## Player state

| Behaviour | Value | What it does |
|---|---|---|
| `fly` | allow/deny/keep | `allow` lets players fly inside, `deny` takes flight away (creative and spectator are left alone). `/fly` refuses to turn on in a `deny` region |
| `glide` | allow/deny/keep | `deny` stops elytra gliding |
| `god` | allow/deny/keep | uxmEssentials god mode inside: `deny` switches it off in an arena, `allow` switches it on. `/god` refuses to turn on in a `deny` region. On the way out the player gets back the god mode they have in uxmEssentials at that moment |
| `game-mode` | `survival`, `creative`, `adventure`, `spectator` | Sets the game mode inside. A player with `uxmessentials.guard.bypass.gamemode` keeps their own |
| `walk-speed` | 0 to 1 | Walking speed; `0.2` is normal |
| `fly-speed` | 0 to 1 | Flying speed; `0.1` is normal |
| `effects.give` | list of `effect[:level]` | Effects that last while inside, such as `speed:2, night_vision` |
| `effects.block` | list of effects | Effects removed on the way in and refused while inside, such as `speed, levitation` |
| `time` | a tick from 0 to 23999, or `day`, `noon`, `night`, `midnight` | The time of day the player sees |
| `weather` | `clear` or `rain` | The weather the player sees |
| `void-teleport` | `<y> spawn` or `<y> <world> <x> <y> <z>` | Below that height the player is sent to the world spawn or the given place, and takes no fall damage from it |

A player with [bypass](tools.md#bypass) on is not grounded by `fly = deny` and may glide where `glide = deny`.
`game-mode` has its own exemption, the `uxmessentials.guard.bypass.gamemode` node, which nobody has by default.

## Comfort

| Behaviour | Value | What it does |
|---|---|---|
| `invincible` | yes/no | `true`: players take no damage at all inside |
| `fall-damage` | yes/no | `false`: no fall damage |
| `firework-damage` | yes/no | `false`: fireworks hurt nobody here, players or mobs |
| `hunger` | yes/no | `false`: food does not go down |
| `heal` | `<amount> <seconds>` | Gives that much health (2 is one heart) every so many seconds |
| `feed` | `<amount> <seconds>` | Gives that much food every so many seconds |
| `keep-inventory` | yes/no | `true`: a player who dies here keeps their items |
| `keep-exp` | yes/no | `true`: a player who dies here keeps their levels |
| `exp-drops` | yes/no | `false`: broken blocks and killed mobs drop no experience here |

## Lists

These are checked on top of the rules, for finer control than an action gives.

| Behaviour | Value | What it does |
|---|---|---|
| `blocks.break.deny` | list of blocks | These may not be broken, even where `break` is allowed |
| `blocks.break.allow` | list of blocks | These may be broken, even where `break` is denied |
| `blocks.place.deny` | list of blocks | These may not be placed, even where `place` is allowed |
| `blocks.place.allow` | list of blocks | These may be placed, even where `place` is denied |
| `items.use.deny` | list of items | These may not be used here |
| `spawn.deny` | list of `type[:reason]` | Mobs that may not spawn: a mob type (`zombie`), `monster`, `animal` (anything that is not a monster) or `*` for all; `:reason` narrows it to a spawn reason such as `natural` or `spawner` |
| `commands.deny` | list of commands | Commands refused here. An entry catches every name of the command (`home` also stops `/h` and `/uxmessentials:home`) and any other plugin's command of that name. Write arguments with `_`: `warp_vip` stops `/warp vip` but not `/warp shop` |
| `commands.allow` | list of commands | When set, only these commands run here. An entry matches the named command of the plugin that owns that name, with any of its aliases |

`deny` entries win over `allow` entries. `/guard` itself always runs, so staff are never locked out of fixing
a region. Players with bypass on are not stopped by the command lists.

## Elsewhere

- God mode and flight from uxmEssentials commands respect `god` and `fly`, see above.
- [Player Warps](../modules/playerwarps.md), [Homes](../modules/homes.md) and every teleport obey `move.enter` and
  `move.teleport-in`, since guard checks the teleport itself.

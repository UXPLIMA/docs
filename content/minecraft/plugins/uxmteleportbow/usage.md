---
title: "Guide"
order: 10
description: "How uxmTeleportBow works, how to set it up and what each file holds."
---

uxmTeleportBow hands players a bow whose arrow carries them to where it lands. It is a lobby toy: the arrow does no damage, never runs out, and the landing is checked so a player is never put in a wall or under the world.

## Install

1. Put the jar in `plugins/`.
2. Start the server once. The plugin writes its files into `plugins/uxmTeleportBow/`.
3. Set the worlds the bow works in under `filters.worlds` in `config.conf`, then run `/uxmteleportbow reload`.

WorldGuard and PlaceholderAPI are optional. Without WorldGuard the region filter checks nothing and the console says so once.

## Files

| File | What it holds |
|---|---|
| `config.conf` | the item, the teleport rules, the trail, the effects and the filters |
| `commands.conf` | the name, aliases and on or off switch of the command |
| `messages/messages_<language>.conf` | the words players read, including the bow's name and lore lines |
| `theme.conf` | the colours of the messages |

## The bow

Any player holding the bow can shoot it; no permission is needed. The bow is recognised by a hidden key, not by its name, so rewording it in the messages file does not break it.

| `item.bow` key | Meaning |
|---|---|
| `material` | the item; keep `BOW` unless a resource pack re-models one |
| `slot` | the inventory slot: 0 to 8 hotbar, 9 to 35 inventory, 36 to 39 armour, 40 off hand |
| `lore-lines` | how many lore lines are shown; line N is `item.bow-lore-N` in the messages file |
| `custom-model-data` | the resource pack model; `0` for none |
| `glow`, `unbreakable` | the shimmer and the durability |
| `infinite-arrows` | the shot never uses up the arrow |

`item.ammo` places one arrow (`material`, `slot`, `amount`) because a bow with no ammunition cannot be drawn outside creative mode. Turn it off only when every player is in creative.

`item.give` decides when the bow is handed out: `on-join`, `on-respawn`, `on-world-change`, and `respect-world-filter`, which skips worlds the filter denies. `item.locked` keeps the bow and arrow in place: they cannot be moved, dropped or placed and never drop on death.

Give each lobby plugin that hands out an item its own slot, or one of the items is never seen.

## Teleport rules

| `teleport` key | Meaning |
|---|---|
| `cooldown-ms` | the wait between two shots; `0` turns it off |
| `fall-damage-grace-ms` | how long after landing a fall does no damage; it also ends at the first fall. `0` turns it off |
| `headroom-blocks` | the clear blocks the landing needs, counted upward; at least 1, `2` fits a standing player |
| `void-floor-y` | the lowest Y a player may be sent to |
| `refund-cooldown-on-refusal` | give the cooldown back when a shot is refused after the arrow flew |
| `arrow-damage` | the damage the arrow deals; `0.0` by default |
| `arrow-speed-multiplier` | how much faster than a normal arrow it flies |
| `arrow-allow-pickup` | whether a player may pick the arrow up |

A shot is refused before it leaves the bow when the world is not allowed or the cooldown is running. After the arrow lands, the teleport is refused when the landing is below `void-floor-y`, has less clearance than `headroom-blocks`, or the region filter blocks the place left or the place landed. The player keeps the direction they were looking.

## Filters

`filters.worlds` takes `mode` and `list`. With `allow` only the listed worlds work; with `deny` every world works except the listed ones. Names are matched without case.

`filters.regions` checks WorldGuard regions:

| Key | Meaning |
|---|---|
| `enabled` | check regions at all |
| `mode` | `allow` or `deny`, as for worlds |
| `check` | `departure` (the place left), `destination` (where the arrow landed) or `both` |
| `list` | region names, matched without case |

A place counts as listed when any region there is listed. In `allow` mode a place with no region is refused.

```hocon
filters {
  worlds  { mode = "allow", list = ["lobby"] }
  regions { enabled = true, mode = "deny", check = "both", list = ["spawn", "shop"] }
}
```

## Trail

`trail` draws a line behind the arrow: `enabled`, `particle` (with data after a space, such as `"dust #00ff00"`), `count`, `offset`, `speed`, `tick-ms` (a lower number is a smoother line and more work) and `give-up-after-ms`, which stops drawing an arrow the plugin never saw land.

## Effects

`effects` holds a list of actions for each moment: `shot`, `departure`, `arrival` and the six refusals `refused-cooldown`, `refused-world`, `refused-departure`, `refused-destination`, `refused-void` and `refused-headroom`. Each line is `[verb] payload`, run in order; an empty list is silence.

The verbs are `[message]`, `[broadcast]`, `[actionbar]`, `[title]`, `[subtitle]`, `[bossbar]`, `[particle]`, `[sound]`, `[effect]`, `[console]` and `[player]`, and each may carry `{delay=, chance=, repeat=, every=, if=}` after the verb. Text that starts with `@` names a line of the messages file. `<seconds>` is filled on the cooldown line and `<world>` on the world line. A sound is heard by that player only; write a `[broadcast]` for the room.

## Commands

| Command | Permission | What it does |
|---|---|---|
| `/uxmteleportbow give [player]` | `uxmteleportbow.admin.give` | give the bow to a player, or to yourself |
| `/uxmteleportbow reload` | `uxmteleportbow.admin.reload` | read every file again |
| `/uxmteleportbow version` | `uxmteleportbow.admin.version` | show the running version |
| `/uxmteleportbow doctor` | `uxmteleportbow.admin.doctor` | report the plugin's health |
| `/uxmteleportbow lang`, `lang set <language>`, `lang reset` | `uxmteleportbow.language` | your own language |
| `/uxmteleportbow lang set-player <player> <language>`, `lang reset-player <player>` | `uxmteleportbow.admin.language` | another player's language |

The names and aliases come from `commands.conf`. The [reference](reference.md) lists the default aliases and every permission.

## Placeholders

| Placeholder | Value |
|---|---|
| `%uxmteleportbow_holding%` | `true` when the player holds the bow |
| `%uxmteleportbow_on_cooldown%` | `true` while the cooldown runs |
| `%uxmteleportbow_cooldown_ms%` | whole milliseconds left, `0` when ready |
| `%uxmteleportbow_cooldown_seconds%` | seconds left with one decimal, `0.0` when ready |

## Troubleshooting

- **The bow cannot be drawn.** `item.ammo.enabled` is off and the player is not in creative.
- **Players carry a bow that refuses every shot.** Their world is denied; turn on `item.give.respect-world-filter`.
- **Region rules are ignored.** WorldGuard is not installed, or `filters.regions.enabled` is `false`.
- **Shots fail inside buildings.** The landing has less clearance than `headroom-blocks`.

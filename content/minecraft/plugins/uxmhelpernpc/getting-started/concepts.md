---
title: Basic Concepts
order: 14
description: The NPC, its owner, menus, actions and the settings that follow it.
icon: lightbulb
---

## One player, one NPC

An NPC belongs to a player, not to a place. `settings.maximum-npc-per-player` caps how many a
player may own; the default is 2. Ownership is what every command checks: returning, moving or
reskinning an NPC you do not own is refused.

## The NPC lives inside a region

The region hook answers one question: *is this player standing inside their own region?* Almost
every action asks it first. If you run without a region hook the checks are skipped, and NPCs can
be placed anywhere that is not a disabled world.

`settings.place-npc-on-region-creation` hands a player their NPC the moment their island or claim
is created, with no purchase and no command. `settings.first-location` decides where it lands
relative to the region centre:

```yaml
first-location:
  enabled: false
  modify-by: -5.5, 1.5, 3.5
  rotation: -88, 0.8
```

## Three ways to get an NPC

| Route | Turned on by |
|---|---|
| Automatically, when the region is created | `settings.place-npc-on-region-creation: true` |
| Bought from the shop menu | `/helpernpc buy`, priced by `settings.price` |
| From an egg item | `/uxmhelpernpc giveegg <player> <amount>`, right-clicked |

The egg is a normal item described in `items.yml`, tagged so that only eggs the plugin gave out
work. `items.yml → npc-egg.give-even-if-has-npc` decides whether a player who already owns an NPC
can be handed another egg.

## Returning an NPC

`/helpernpc return` refunds `settings.return-percent` of `settings.price` and deletes the NPC. It
asks for the same command again as confirmation and forgets the request after ten seconds.
`settings.return-price-on-delete` controls whether a deletion refunds at all.

## Actions are the extension point

Everything a click can do is an action string: `[menu] main`, `[player] spawn`,
`[sound] ENTITY_ALLAY_AMBIENT_WITHOUT_ITEM`. Menu items carry a list of them, and
`settings.default-click-actions` is what a click on the NPC body runs. See
[Actions](../menus/actions.md).

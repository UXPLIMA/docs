---
title: config.yml
order: 301
description: Settings, hooks, burn, vanilla crafting, progression and categories.
icon: file-cog
---

The main file. Six top-level sections.

```yaml
settings:          # language, debug, slots, hooks, performance
burn:              # global burn switch, bypasses, multipliers
vanillaCrafting:   # crafting-table level gates
progression:       # curves, reward policy, mastery bonuses, perk tree
categories:        # every category and the file its trades live in
```

## settings

| Key | Default | What it does |
|---|---|---|
| `lang` | `en` | `en` or `tr`; picks a block out of `language.yml` |
| `debug` | `false` | Verbose logging |
| `removePendingInvalidTrade` | `true` | Drop pending trades whose definition no longer exists |
| `disableSlotModule` | `false` | Turn off the queue entirely — trades complete on click |
| `instantCraftBatchLimit` | `64` | Cap on crafts per shift-click batch |

`removePendingInvalidTrade` is the setting to think about before deleting a trade. Left `true`, a
player's in-progress craft of a trade you removed is discarded on startup. Set it `false` and the
row stays, unresolvable, until you put the trade back.

### settings.complete

The effect on a successful trade.

| Key | Default |
|---|---|
| `sound` | `ENTITY_PLAYER_LEVELUP` |
| `particleEffect` | `VILLAGER_HAPPY` |
| `particleCount` | `10` |

Bukkit `Sound` and `Particle` enum names. Version-specific — a name that does not exist on your
server is logged and skipped.

### settings.performance

| Key | Default | What it does |
|---|---|---|
| `remainingTradeSlotCacheTime` | `10000` | Milliseconds a remaining-slot count is cached |
| `remainingTradeSlotCacheLimit` | `200` | Maximum cached entries |

### settings.Hooks

A switch per integration. Installing the plugin is not enough — the hook must be **on**.

| Key | Default | Plugin |
|---|---|---|
| `MONEY` | `true` | Vault |
| `PLACEHOLDER` | `true` | PlaceholderAPI |
| `NEXO` | `true` | Nexo |
| `SAVE` | `true` | Built in — the binary item store |
| `SKULL` | `true` | Built in — player heads |
| `XP` | `true` | Built in — vanilla experience |
| `PLAYERPOINTS` | `false` | PlayerPoints |
| `COINSENGINE` | `false` | CoinsEngine |
| `MMOITEMS` | `false` | MMOItems |
| `ECOITEM` | `false` | EcoItems |
| `EXECITEM` | `false` | ExecutableItems |
| `IAITEM` | `false` | ItemsAdder |

`IAITEM` and `NEXO` are interchangeable: a trade written for one uses whichever plugin is actually
installed.

A hook that is on but whose plugin is missing is simply not registered, with a line in the log. See
[Integrations](../integrations.md).

## burn

| Key | Default |
|---|---|
| `enabled` | `true` |
| `bypassPermission` | `uxmblacksmith.bypassall` |
| `burnSound` | `ENTITY_ENDERDRAGON_GROWL` |
| `burnParticleEffect` | `SMOKE_NORMAL` |
| `burnParticleCount` | `10` |
| `permissionBasedMultipliers` | `uxmblacksmith.burn.1: 5`, `uxmblacksmith.burn.2: 10` |

Full explanation in [Burn and risk](../features/burn.md).

## vanillaCrafting

**Enabled by default**, with ten materials gated behind blacksmith levels. See
[Vanilla crafting gates](../features/vanilla-crafting.md) — this is the setting most likely to
surprise you on an existing server.

## progression

Curves, the XP reward policy, mastery bonuses and the perk tree. Three pages cover it:
[Progression](../features/progression.md), [Masteries](../features/masteries.md),
[Perks](../features/perks.md).

## categories

Four shipped categories, each pointing at a file under `categories/`:

| Key | Name | Icon | Slot | File |
|---|---|---|---|---|
| `swords` | Blades | `DIAMOND_SWORD` | 19 | `swords.yml` |
| `armors` | Armors | `DIAMOND_CHESTPLATE` | 21 | `armors.yml` |
| `tools` | Tools | `DIAMOND_PICKAXE` | 23 | `tools.yml` |
| `artifacts` | Artifacts | `TOTEM_OF_UNDYING` | 25 | `artifacts.yml` |

Fields are documented in [Categories](../features/categories.md).

## Format notes

The file is written by the plugin from its own defaults, which has two consequences worth knowing:

**Missing keys are re-added on start.** Delete a setting and it comes back with its default. To turn
something off, set it to `false` — do not remove it.

**Comments in the file are generated.** Your own comments are lost the next time the plugin rewrites
the file. Keep notes outside it.

Keys are camelCase exactly as shown. `successChance`, not `success_chance`; `requiredOutput`, not
`required-output`.

## Reloading

`/blacksmith reload` re-reads this file, `language.yml`, `gui.yml`, `modules/boosts.yml` and every
category file, and clears the burn permission cache. The database connection is not reopened.

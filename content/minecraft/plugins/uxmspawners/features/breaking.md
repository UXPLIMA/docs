---
title: Breaking Rules
order: 40
description: "Break management: who may break a spawner, with what, and what drops."
icon: pickaxe
---

Who may break a spawner, with what, and whether they get it back, is `modules/other.yml`.

## Drop chance

```yaml
spawner-drop-chance:
  default: 100
  permissions:
    uxmspawners-drop1: 100
    uxmspawners-drop2: 100
  vanilla-permissions:
    uxmspawners-drop1: 100
    uxmspawners-drop2: 100
```

`default` is the chance for a player with no matching permission. The two permission lists are the
tiers: one for uxmSpawners spawners, one for vanilla spawners when vanilla support is on. Keep
them ascending; the highest match wins.

## Requiring a tool

```yaml
break-management:
  mode: DISABLED
  break-item:
    material: "DIAMOND_PICKAXE"
    name: "<dark_gray>• <aqua>Spawner Pickaxe <dark_gray>(<white>{uses} use(s)<dark_gray>)"
  exempt: uxmspawners.bypassbreak
  block: true
  claimless-spawners-breakable-by-everyone: false
```

| Mode | Meaning |
|---|---|
| `DISABLED` | No requirement |
| `ONLY_FOR_VANILLA` | Vanilla spawners need the break item |
| `ONLY_FOR_VANILLA_SILK_TOUCH` | Vanilla spawners need silk touch |
| `ONLY_FOR_UXMSPAWNER` | uxmSpawners spawners need the break item |
| `ONLY_FOR_UXMSPAWNER_SILK_TOUCH` | uxmSpawners spawners need silk touch |
| `FOR_BOTH` | Both types need the break item |
| `FOR_BOTH_SILK_TOUCH` | Both types need silk touch |

`block: true` refuses the break outright without the right tool. `block: false` allows the break
but the spawner is lost: a harsher rule that reads as a bug to players, so prefer `true` unless
you mean it.

The break item has a use counter: `{uses}` in its name is replaced with what is left. Hand it out
with `/uxmspawners givepickaxe`.

`claimless-spawners-breakable-by-everyone` only applies with uxmClaims: a spawner outside any
claim becomes fair game.

## Breaking other people's spawners

```yaml
break-spawners:
  permission: uxmspawners.breakallspawners
```

Staff node. Without it, ownership and your protection plugin decide.

## Vanilla spawners

```yaml
vanilla-spawner-support:
  enabled: true
  disable-vanilla-breaking: false
  enable-placing: true
  support-on-break: true
  drop-vanilla-on-break: true
```

With `enabled`, breaking a vanilla spawner tries to match its entity type against your configured
spawner types, and hands the player a uxmSpawners spawner if one matches, which is how an
existing world is migrated without touching it. The rest of the block decides whether vanilla
spawners can be broken or placed at all, and whether the plain vanilla block drops.

## Auto-pickup

```yaml
auto-pickup:
  enabled: false
  permission: uxmspawners.autopickup
```

Sends a dropped spawner straight into the inventory.

## Explosions

`config.yml → protect-spawners-from-explosions` keeps creepers and TNT from destroying spawners.
Worth turning on the day you sell one.

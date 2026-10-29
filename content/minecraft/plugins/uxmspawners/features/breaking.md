---
title: Breaking Rules
order: 40
description: Who may break a spawner, the tool it asks for, the chance of getting it back and what happens to vanilla spawners.
icon: pickaxe
---

Everything about breaking is in `modules/other.yml`, except the world list and explosions, which are
in `config.yml`.

## Who may break a spawner

| Rule | Note |
|---|---|
| Not in `disabled-worlds` | Refused there for everyone, staff included |
| The owner, or staff with `uxmspawners.breakallspawners` | Friends cannot break it. Anyone else is told they are not the owner |
| The tool `break-management` asks for | Unless the player has `uxmspawners.bypassbreak` |

With uxmClaims, a spawner in a claim that has been deleted can be broken by anyone, and so can every
spawner outside a claim while `claimless-spawners-breakable-by-everyone` is on.

## Requiring a tool

```yaml
break-management:
  mode: DISABLED
  break-item:
    material: "DIAMOND_PICKAXE"
    model-data: 0
    name: "<dark_gray>• <aqua>Spawner Pickaxe <dark_gray>(<white>{uses} use(s)<dark_gray>)"
    lore: [...]
  exempt: uxmspawners.bypassbreak
  block: true
  claimless-spawners-breakable-by-everyone: false
```

The **pickaxe** is the break item, given with `/uxmspawners givepickaxe <player> <uses>`. Each spawner
broken with it uses one use; `{uses}` in its name shows what is left, and it disappears at zero.
**Silk touch** means any tool with Silk Touch.

What each mode asks for in 4.2.0. The names do not match what they do, so pick by this table:

| `mode` | A uxmSpawners spawner needs | A vanilla spawner needs |
|---|---|---|
| `DISABLED` | Nothing | Nothing |
| `ONLY_FOR_VANILLA` | Silk touch | The pickaxe |
| `ONLY_FOR_VANILLA_SILK_TOUCH` | Silk touch | Silk touch |
| `ONLY_FOR_UXMSPAWNER` | The pickaxe | The pickaxe |
| `ONLY_FOR_UXMSPAWNER_SILK_TOUCH` | Silk touch | The pickaxe |
| `FOR_BOTH` | The pickaxe | The pickaxe |
| `FOR_BOTH_SILK_TOUCH` | Silk touch | Silk touch |

| `block` | Without the right tool |
|---|---|
| `true` | The break is refused. A missing silk touch is explained; a missing pickaxe is not |
| `false` | The spawner breaks and is lost, with nothing dropped |

<Callout type="warning" title="block: false destroys spawners">

Players read a spawner that vanished as a bug, and support has to replace it. Keep `block: true`
unless losing the spawner is the rule you want.

</Callout>

## Drop chance

```yaml
spawner-drop-chance:
  default: 100
  permissions:
    uxmspawners-drop1: 100
  vanilla-permissions:
    uxmspawners-drop1: 100
```

Every spawner item a break would give is rolled against this chance, in percent: `default`, or the
highest value from a node the player holds. `permissions` is for uxmSpawners spawners and
`vanilla-permissions` for vanilla ones being turned into uxmSpawners spawners. A failed roll loses
that item.

How many items a break gives depends on the level, see [Levels](levels.md#breaking-levels-off).
Breaking the last level removes the spawner with its storage and its friends list.

## Where the item goes

```yaml
auto-pickup:
  enabled: false
  permission: uxmspawners.autopickup
```

With `enabled` on, players holding the permission get broken spawners straight into their
inventory; what does not fit drops. Everyone else sees them drop on the ground.

## Vanilla spawners

A vanilla spawner is one the plugin does not know: generated with the world, or placed from a plain
spawner item.

```yaml
vanilla-spawner-support:
  enabled: true
  disable-vanilla-breaking: false
  enable-placing: true
  support-on-break: true
  drop-vanilla-on-break: true
```

| Setting | Meaning |
|---|---|
| `enabled: false` | Vanilla spawners cannot be broken at all |
| `disable-vanilla-breaking: true` | Only staff with `uxmspawners.admin` can break them, and get nothing |
| `support-on-break: true` | They break the vanilla way. With `drop-vanilla-on-break`, a vanilla spawner item of the same mob drops |
| `support-on-break: false` | Breaking one gives a uxmSpawners spawner of the same mob, rolled against `vanilla-permissions` |
| `enable-placing` | Plain spawner items can be placed, as vanilla spawners. `false` refuses them |

With the defaults, a vanilla spawner stays vanilla: it drops as a vanilla item and is placed again as
one. To turn the spawners already in your world into uxmSpawners spawners as players break them, set
`support-on-break: false`.

`disable-vanilla-spawners-spawning: true` in `config.yml` stops vanilla spawners from spawning
anything.

## Explosions

`protect-spawners-from-explosions` in `config.yml` is off by default. Off, a creeper or TNT destroys
a uxmSpawners spawner, with its storage, and nothing drops. On, explosions leave them alone. Vanilla
spawners are not protected either way.

Related: [Levels](levels.md), [Permissions](../permissions.md), [Integrations](../integrations.md)

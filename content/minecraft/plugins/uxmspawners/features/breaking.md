---
title: Breaking Rules
order: 140
description: Who may break a spawner, the tool it asks for, the chance of getting it back, the storage of a spawner that goes, and natural spawners.
icon: pickaxe
---

Everything about breaking is in `general-settings.yml`, except the world list, which is
`world-management` in `config.yml`.

## Who may break a spawner

| Rule | Note |
|---|---|
| Not in a disabled world | Refused there, except for `uxmspawners.bypassbreak` and bypass mode |
| The owner, or staff with `uxmspawners.breakallspawners` | Friends cannot break it. Anyone else is told they are not the owner |
| The claim plugin allows it | Its own rules for breaking blocks apply first. It is asked about the block the spawner is placed as |
| The item `break-management` asks for | Unless the player has `uxmspawners.bypassbreak` |
| The storage is empty, at the last level | Unless `break-with-storage` says otherwise, see below |

Two settings let more players break spawners inside claims:

| Setting | Effect |
|---|---|
| `claimless-spawners-breakable-by-everyone` | A spawner outside every claim, island or region, for example because its claim was deleted, can be broken by anyone. Claiming the area again protects it again |
| `claim-owners-break-all-spawners` | Claim owners, and members the claim lets break spawners, can break every spawner in the claim |

Both need a claim plugin and do nothing without one. See [Integrations](../integrations.md#claims-islands-and-regions).

## Requiring a tool

```yaml
break-management:
  mode: DISABLED
  break-item:
    material: "DIAMOND_PICKAXE"
    model-data: 0
    name: "<#7AD7C9><bold>Spawner Pickaxe</bold> <#5F6778>· <#DCE1EA><uses> <#9AA3B5>use(s)"
    lore:
      - "<#5F6778>Breaks spawners and keeps them."
  pickaxe-uses: WHEN_REQUIRED
  pickaxe-use-per-level: false
  exempt: uxmspawners.bypassbreak
  block: true
  lost-levels: ONE
```

| `mode` | A uxmSpawners spawner needs | A natural spawner needs |
|---|---|---|
| `DISABLED` | Nothing | Nothing |
| `ONLY_FOR_VANILLA` | Nothing | The pickaxe |
| `ONLY_FOR_VANILLA_SILK_TOUCH` | Nothing | Silk Touch |
| `ONLY_FOR_UXMSPAWNER` | The pickaxe | Nothing |
| `ONLY_FOR_UXMSPAWNER_SILK_TOUCH` | Silk Touch | Nothing |
| `FOR_BOTH` | The pickaxe | The pickaxe |
| `FOR_BOTH_SILK_TOUCH` | Silk Touch | Silk Touch |

**Silk Touch** means any tool with Silk Touch. Players with `exempt` skip the requirement; an empty
`exempt` exempts nobody.

| `block` | Without what the mode asks for |
|---|---|
| `true` | The break is refused and the player is told what is missing |
| `false` | The break goes through, at a cost set by `lost-levels` |

| `lost-levels` | With `block: false` |
|---|---|
| `ONE` | One level is lost; the spawner itself only at its last level, and then its storage is handled as `break-with-storage` says |
| `ALL` | The whole spawner is lost, every level |

A natural spawner broken without the right item is simply gone.

<Callout type="warning" title="block: false destroys spawners">

Players read a spawner that vanished as a bug, and support has to replace it. Keep `block: true`
unless losing levels is the rule you want.

</Callout>

## The spawner pickaxe

`/usp givepickaxe <player> [uses]` gives it, with 10 uses by default. `<uses>` in its name shows what
is left, and it disappears with its last use. It is signed like a spawner item, so a pickaxe with
edited uses does not count. See [Item Signing](item-signing.md).

| Setting | Meaning |
|---|---|
| `pickaxe-uses: WHEN_REQUIRED` | A break takes a use only when the mode asked for the pickaxe and the player is not exempt |
| `pickaxe-uses: ALWAYS` | Every break with the pickaxe in hand takes a use |
| `pickaxe-use-per-level` | Breaking several levels at once while sneaking takes a use per level, and no more levels than the pickaxe has uses |

## Drop chance

```yaml
spawner-drop-chance:
  default: 100
  vanilla-default: 100
  permissions:
    uxmspawners-drop1: 100
  vanilla-permissions:
    uxmspawners-drop1: 100
```

Every spawner item a break would give is rolled against this chance, in percent: `default`, or the
highest value from a node the player holds. `default` and `permissions` are for uxmSpawners spawners,
`vanilla-default` and `vanilla-permissions` for natural ones. A failed roll loses that item and tells
the player so.

How many items a break gives depends on the level, see [Levels](levels.md#breaking-levels-off).

## Storage when a spawner goes

When a player breaks a spawner's last level while it still stores items or XP:

| `break-with-storage` | Result |
|---|---|
| `BLOCK` | Refused until the storage is empty. The player is told how much is left. An op who breaks it while sneaking breaks it anyway, and its storage drops |
| `INVENTORY` | The items go to the player's inventory and the XP to the player; what does not fit drops |
| `DROP` | Everything drops where the spawner stood |

A spawner can also go without a player breaking it: an explosion, a wither, or its block removed by
WorldEdit or another plugin. `lost-spawner-storage` decides what happens to its storage then:

```yaml
lost-spawner-storage:
  items: SELL
  unsold-items: DROP
  xp: OWNER
  notify-owner: true
```

| Setting | Values |
|---|---|
| `items` | `SELL`: what the shop buys is sold for the owner, online or not. `DROP`: everything drops. `DELETE`: the items are lost |
| `unsold-items` | With `SELL`, the items the shop does not buy: `DROP` or `DELETE` |
| `xp` | `OWNER`: given to the owner if online, otherwise dropped as one orb. `DROP` or `DELETE` |
| `notify-owner` | An owner who is online is told the spawner is gone and what its storage sold for |

An offline owner's sale counts their sell multiplier, read from LuckPerms. Storages drop a few stacks
at a time, so the server does not stall.

### Big storages

A storage of a million items would put thousands of stacks on the ground. `dropped-storage` caps it,
for every drop above: `break-with-storage` `DROP` and `INVENTORY`, and `lost-spawner-storage` `DROP`
and `unsold-items: DROP`.

```yaml
dropped-storage:
  max-stacks: 1000
  over-limit: BLOCK
```

| Setting | Meaning |
|---|---|
| `max-stacks` | The most item stacks one spawner's storage drops. `0` is no limit |
| `over-limit: BLOCK` | A player cannot break the spawner while more would drop, and is told to sell or take some first. A spawner gone without a player drops up to the limit; the rest is deleted |
| `over-limit: DELETE` | Up to the limit drops, the rest is deleted |

What is deleted is written to the log file with its amount.

## Protecting spawner blocks

`protect-spawners-from-explosions: true`, the default, keeps creepers, TNT and withers away from
uxmSpawners spawners. Natural spawners are not protected. A spawner placed as another block, such as
a diamond block, that an explosion does take, never drops that block.

Whatever the settings, a spawner block is never changed by a hoe, shovel, axe or honeycomb, bone
meal, a sheep, a silverfish, lightning or a tree growing into it. An iron, snow or copper golem
or a wither built with spawner blocks in or around its shape is not made, and the blocks stay.

## Where the item goes

```yaml
auto-pickup:
  enabled: false
  permission: uxmspawners.autopickup
```

With `enabled` on, players holding the permission get broken spawners straight into their
inventory; what does not fit drops. Everyone else sees them drop on the ground.

## Natural spawners

A natural spawner is one the plugin does not know: generated with the world, or placed from a plain
spawner item.

```yaml
vanilla-spawner-support:
  enabled: true
  disable-vanilla-breaking: false
  disable-vanilla-spawning: false
  enable-placing: true
  on-break: UXMSPAWNERS
  place-with-mob: true
  spawn-eggs:
    enabled: true
    permission: uxmspawners.spawnegg
```

| Setting | Meaning |
|---|---|
| `enabled: false` | Natural spawners cannot be broken at all |
| `disable-vanilla-breaking: true` | Only players with `uxmspawners.breakvanillaspawners` can break them, and get nothing. `uxmspawners.admin` includes it |
| `disable-vanilla-spawning: true` | They spawn nothing |
| `on-break: UXMSPAWNERS` | Breaking one gives a uxmSpawners spawner of the same mob, from that mob's file in `spawners/` or `default.yml` |
| `on-break: VANILLA` | Breaking one gives a vanilla spawner item of the same mob, placed again as a natural spawner |
| `on-break: NONE` | Nothing drops, as in Minecraft |
| `enable-placing` | Plain spawner items can be placed, as natural spawners. `false` refuses them |
| `place-with-mob` | A vanilla spawner item dropped by `on-break: VANILLA` is placed with its mob. The game itself does that for creative operators only. Turn it off when another plugin does it |
| `spawn-eggs` | Whether players may change a natural spawner's mob with a spawn egg, and the permission for it. Eggs never work on uxmSpawners spawners |

The block always breaks the vanilla way and gives its XP; the drop is rolled against
`vanilla-default` and `vanilla-permissions`, and given only when no other plugin cancelled the break.

A natural spawner changed with an egg breaks into a spawner of the new mob, so with `on-break:
UXMSPAWNERS` a cheap egg turns a zombie spawner into a blaze spawner. Keep `uxmspawners.spawnegg` for
players you trust, or turn `spawn-eggs.enabled` off.

Related: [Levels](levels.md), [Permissions](../permissions.md), [Integrations](../integrations.md)

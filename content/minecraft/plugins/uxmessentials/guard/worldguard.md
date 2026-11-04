---
title: Importing WorldGuard
order: 1130
description: Move WorldGuard regions over with a dry run first, an undo after, and a report of everything not converted.
icon: import
---

Contents: [The three steps](#the-three-steps) · [What is converted](#what-is-converted) ·
[Membership becomes rules](#membership-becomes-rules) · [Flags to rules](#flags-to-rules) ·
[Flags to behaviours](#flags-to-behaviours) · [Afterwards](#afterwards)

## The three steps

Keep WorldGuard installed while you import; guard reads its files, not the running plugin.

```text
/guard import worldguard dry-run
/guard import worldguard run
/guard import worldguard undo
```

1. **dry-run** reads everything, converts it in memory, writes `plugins/uxmEssentials/guard/import-report.txt`
   and changes nothing. Read the report: it lists every flag, player name or region that could not be converted.
2. **run** takes a [backup](tools.md#backups), stores the converted regions and tags them with an import batch.
   A region whose name already exists in guard is skipped and counted, never overwritten.
3. **undo** removes every region of the last batch. Since a run never overwrites, this returns guard to how it
   was before the run.

By default the regions come from `plugins/WorldGuard/worlds/<world>/regions.yml`, for every world folder there.
`sql` as the last word (`/guard import worldguard run sql`) reads WorldGuard's database instead, with the
`regions.sql` settings in WorldGuard's own `config.yml`.

## What is converted

| WorldGuard | Guard |
|---|---|
| Region name | The same name in lower case; characters other than letters, digits, `_` and `-` become `-` |
| `__global__` | A `global` region named `wg-global` with priority -100000, under every other region |
| Cuboid, polygon | The same shape |
| Priority, parent | The same |
| Owners, members (by UUID) | The `owner` and `member` roles |
| Owners, members (by name) | Looked up among players the server has seen; a name never seen goes to the report |
| Owner and member groups | `perm:group.<name>` rules wherever the region's members are allowed |
| `<flag>-group` | The target of that flag's rules: `members`, `owners`, `non_members`, `non_owners`, `all` or `none` |

## Membership becomes rules

In WorldGuard a region without a `build` flag lets only its members build. A guard region without rules does
nothing, so the importer writes that default out as rules: everyone is denied the build family, and owners,
members and member groups are allowed it. The build family is `break`, `place`, `use`, `interact`,
`damage.animal`, `damage.villager`, `damage.armor-stand`, `damage.hanging`, `damage.vehicle`, `vehicle.place`,
`vehicle.destroy`, `hanging.place`, `hanging.break`, `item.use.bucket`, `item.use.lighter`, `item.use.bone-meal`,
`item.use.spawn-egg` and `trample`. A region with `passthrough: allow` or a `build` flag of its own skips this.
The global region skips it too, unless it has `passthrough: deny`, the usual WorldGuard way to protect a whole
world: then only its members build anywhere no other region decides.

A flag the region sets itself beats this default, as in WorldGuard: `use: allow` lets everyone use, and
`block-break: deny` stops members as well.

## Flags to rules

A state flag becomes one rule for each action below, `allow` or `deny` as the flag was, for the flag's group
(`all` unless the region sets `<flag>-group`; `entry` and `exit` default to non-members, as in WorldGuard).

| WorldGuard flag | Guard actions |
|---|---|
| `build` | the build family (see above) |
| `block-break` | `break` |
| `block-place` | `place` |
| `use` | `use` |
| `interact` | `use`, `interact` |
| `damage-animals` | `damage.animal` |
| `pvp` | `damage.player` |
| `sleep` | `use.bed` |
| `respawn-anchors` | `use.respawn-anchor` |
| `tnt` | `world.explosion.tnt` |
| `chest-access` | `use.container` |
| `vehicle-place` | `vehicle.place` |
| `vehicle-destroy` | `vehicle.destroy` |
| `lighter` | `item.use.lighter` |
| `ride` | `interact.vehicle` |
| `potion-splash` | `item.use.potion` |
| `item-frame-rotation` | `interact.item-frame` |
| `block-trampling` | `trample` |
| `use-anvil` | `use.workstation` |
| `use-dripleaf` | `trample.dripleaf` |
| `wind-charge-burst`, `breeze-charge-explosion` | `world.explosion.breeze` |
| `item-pickup` | `item.pickup` |
| `item-drop` | `item.drop` |
| `creeper-explosion` | `world.explosion.creeper` |
| `ghast-fireball` | `world.explosion.ghast` |
| `other-explosion` | `world.explosion.other`, `world.explosion.bed`, `world.explosion.crystal` |
| `wither-damage` | `world.explosion.wither`, `world.mob.wither` |
| `enderman-grief` | `world.mob.enderman` |
| `snowman-trails` | `world.mob.snow-golem` |
| `ravager-grief` | `world.mob.ravager` |
| `enderdragon-block-damage`, `entity-item-frame-destroy`, `entity-painting-destroy` | `world.mob.other` |
| `pistons` | `world.piston` |
| `fire-spread` | `world.fire.spread` |
| `lava-fire` | `world.fire.lava` |
| `lightning` | `world.lightning`, `world.fire.lightning` |
| `snow-fall`, `ice-form`, `frosted-ice-form`, `lava-harden` | `world.form` |
| `snow-melt`, `ice-melt`, `frosted-ice-melt` | `world.melt` |
| `mushroom-growth`, `grass-growth`, `mycelium-spread` | `world.spread` |
| `leaf-decay` | `world.decay` |
| `vine-growth` | `world.grow.vine` |
| `rock-growth` | `world.grow.amethyst` |
| `sculk-growth` | `world.grow.sculk` |
| `crop-growth` | `world.grow.crop` |
| `soil-dry`, `coral-fade`, `copper-fade`, `moisture-change` | `world.fade` |
| `water-flow` | `world.liquid.water` |
| `lava-flow` | `world.liquid.lava` |
| `entry` | `move.enter` |
| `exit` | `move.exit` |
| `enderpearl` | `item.use.ender-pearl` |
| `chorus-fruit-teleport` | `item.use.chorus-fruit` |

## Flags to behaviours

| WorldGuard flag | Guard behaviour |
|---|---|
| `greeting`, `farewell` | `message.enter`, `message.exit` on the `chat` channel |
| `greeting-title`, `farewell-title` | `message.enter`, `message.exit` on the `title` channel |
| `entry-deny-message`, `exit-deny-message` | `enter.deny-message`, `exit.deny-message` |
| `deny-message` set to empty | `deny-messages = false` |
| `game-mode` | `game-mode` |
| `invincible` | `invincible` |
| `time-lock`, `weather-lock` | `time`, `weather` |
| `blocked-cmds`, `allowed-cmds` | `commands.deny`, `commands.allow` |
| `heal-amount` with `heal-delay`, `feed-amount` with `feed-delay` | `heal`, `feed` |
| `mob-spawning: deny` | `spawn.deny = *` |
| `deny-spawn` | `spawn.deny` with those mob types |
| `fall-damage`, `firework-damage`, `natural-hunger-drain`, `exp-drops` | `fall-damage`, `firework-damage`, `hunger`, `exp-drops` |

Colour codes in WorldGuard messages (`&a`) are turned into MiniMessage.

## Afterwards

While both plugins are installed, both protect: a player must pass WorldGuard and guard. Check the imported
regions in game (`/guard info`, `/guard test`, a walk through them), then remove WorldGuard and restart. The
uxmEssentials features that read WorldGuard (menu conditions, `respect-worldguard` in poses) simply stop finding
it.

A flag the report lists as not converted has no meaning in guard, or was registered in WorldGuard by another
plugin. uxmEssentials' own `set-pwarp` flag is one of these: nothing ever checked it, so nothing changes when it
is gone.

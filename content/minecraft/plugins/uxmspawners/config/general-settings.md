---
title: general-settings.yml
order: 56
description: Every section of general-settings.yml, the page that explains it, and the few settings that live only here.
icon: sliders-horizontal
---

`general-settings.yml` holds how spawners behave: storage, breaking, friends, Auto Kill, multipliers
and the rest. In 4.2.0 these were the `modules/` files and part of `config.yml`. Changes apply with
`/usp reload`.

## Sections

| Section | Page |
|---|---|
| `virtual-storage`, `xp-storage`, `storage-limit`, `multipliers`, `title-messages` | [Storage & Selling](../features/storage.md) |
| `production`, `level-scaling`, `allow-stopping-spawners` | [Production](../features/production.md) |
| `spawner-level-limit` | [Levels](../features/levels.md) |
| `auto-kill`, `nerf-spawner-mobs`, `no-equipment`, `spawned-mobs` | [Auto Kill](../features/auto-kill.md) |
| `break-management`, `break-spawners`, `spawner-drop-chance`, `lost-spawner-storage`, `auto-pickup`, `vanilla-spawner-support` | [Breaking Rules](../features/breaking.md) |
| `friends`, `chat-quit-triggers` | [Friends](../features/friends.md) |
| `item-signing` | [Item Signing](../features/item-signing.md) |
| `rosestacker` | [Integrations](../integrations.md#stackers) |

## Spawner blocks

| Setting | Default | Meaning |
|---|---|---|
| `open-gui-on-click` | `true` | Right-clicking a spawner opens its menu. With `false` there is no way to open it, so storage cannot be emptied |
| `disable-xp-drops` | `false` | Spawners give no XP at all, stored or dropped |
| `disable-vanilla-spawners-spawning` | `false` | Natural spawners stop spawning anything |
| `protect-spawners-from-explosions` | `true` | Creepers, TNT and withers leave uxmSpawners spawners alone. Off, the spawner is destroyed and its storage handled as `lost-spawner-storage` says |

## Placing

```yaml
place-management:
  disable-placement-outside-claims: false
```

`true` lets players place spawners only inside a claim, island or region of the claim plugin
installed. Without one it does nothing. `uxmspawners.bypassplace` places anywhere.

## Log files

```yaml
log-file-retention-days: 14
```

Problems with single spawners, such as a broken block or an item with a wrong signature, are written
to `logs/<date>_uxmspawners.log`, one file a day. Older files are deleted after this many days; `0`
keeps them forever.

Related: [Configuration](index.md), [config.yml](config-yml.md)

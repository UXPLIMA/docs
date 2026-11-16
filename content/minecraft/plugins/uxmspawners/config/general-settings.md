---
title: general-settings.yml
order: 156
description: Every section of general-settings.yml, the page that explains it, and the few settings that live only here.
icon: sliders-horizontal
---

`general-settings.yml` holds how spawners behave: storage, breaking, friends, Auto Kill, multipliers
and the rest. In 4.2.0 these were the `modules/` files and part of `config.yml`. Changes apply with
`/usp reload`.

## Sections

The file opens with the single settings, then one section per feature:

| Section | Page |
|---|---|
| `open-gui-on-click`, `allow-stopping-spawners`, `withdraw-chat-timeout`, `chat-quit-triggers`, `disable-xp-drops`, `protect-spawners-from-explosions`, `log-file-retention-days` | Below |
| `auto-kill` | [Auto Kill](../features/auto-kill.md) |
| `storage-limit`, `virtual-storage`, `xp-storage`, `multipliers`, `title-messages` | [Storage & Selling](../features/storage.md) |
| `friends` | [Friends](../features/friends.md) |
| `spawner-level-limit`, `level-scaling` | [Levels](../features/levels.md), [Production](../features/production.md) |
| `break-management`, `spawner-drop-chance`, `break-spawners`, `auto-pickup`, `vanilla-spawner-support`, `lost-spawner-storage`, `dropped-storage` | [Breaking Rules](../features/breaking.md) |
| `place-management` | Below |
| `nerf-spawner-mobs`, `no-equipment`, `spawned-mobs` | [Auto Kill](../features/auto-kill.md#mobs-that-do-spawn) |
| `production` | [Production](../features/production.md) |
| `rosestacker` | [Integrations](../integrations.md#stackers) |
| `item-signing` | [Item Signing](../features/item-signing.md) |

## The single settings

| Setting | Default | Meaning |
|---|---|---|
| `open-gui-on-click` | `true` | Right-clicking a spawner opens its menu. With `false` there is no way to open it, so storage cannot be emptied |
| `allow-stopping-spawners` | `true` | Owners can stop and start their spawners from the menu. `false` hides the button and lets stopped spawners run again |
| `withdraw-chat-timeout` | `15` | Seconds the withdraw menu's **Custom Amount** waits for a number in chat. `0` waits until the player answers, cancels or leaves |
| `chat-quit-triggers` | `cancel`, `iptal` and others | Words that cancel an open chat prompt, adding a friend or a custom amount, in any case. An empty list turns cancelling off |
| `disable-xp-drops` | `false` | Spawners give no XP at all, stored or dropped |
| `protect-spawners-from-explosions` | `true` | Creepers, TNT and withers leave uxmSpawners spawners alone. Off, the spawner is destroyed and its storage handled as `lost-spawner-storage` says |
| `log-file-retention-days` | `14` | Days to keep the log files, see below |

`disable-vanilla-spawners-spawning` is now `vanilla-spawner-support.disable-vanilla-spawning`. See
[Breaking Rules](../features/breaking.md#natural-spawners).

## Placing

```yaml
place-management:
  disable-placement-outside-claims: false
```

`true` lets players place spawners only inside a claim, island or region of the claim plugin
installed. Without one it does nothing. `uxmspawners.bypassplace` places anywhere.

## Log files

Problems with single spawners, such as a broken block or an item with a wrong signature, are written
to `logs/<date>_uxmspawners.log`, one file a day. Older files are deleted after
`log-file-retention-days`; `0` keeps them forever.

Related: [Configuration](index.md), [config.yml](config-yml.md)

---
title: Troubleshooting
order: 80
description: What to check when the plugin does not start, a spawner produces nothing, storage stays empty or breaking goes wrong.
icon: life-buoy
---

Start with the console and `logs/errors.txt` in the plugin folder: the plugin writes most problems to
one or both.

## The plugin does not start

| Console says | Fix |
|---|---|
| `License key is not set` | Set `license-key` in `config.yml`. No key yet: open a ticket in the UXPLIMA Discord |
| `License validation failed` | The key is wrong or not valid for this server. Ask UXPLIMA |
| `Vault or an economy plugin could not be found` | Install Vault and an economy plugin |
| `NBT-API could not be found` | Install NBTAPI |
| An error about permissions while enabling | Vault has no permission plugin behind it. Install LuckPerms or another one |

## A spawner produces nothing

| Cause | Fix |
|---|---|
| Nobody is within `range` | A player has to be close, or a WildLoaders chunk loader has to be in the chunk |
| The type has no `range` | Add `range` to that type in `config.yml`; without it the range is 0 |
| The chunk is not loaded | Spawners only run in loaded chunks |
| A `SPAWN_TIME` boost with `amount: 0` | It stops the spawner until it ends |
| Real mobs appear instead of loot | Auto-kill is off on that spawner. Turn it on from its menu, with `uxmspawners.autokill` |

## Storage stays empty or stops growing

| Cause | Fix |
|---|---|
| The storage is full | The limit is per item type. Raise it, or sell |
| No XP is stored on a new spawner | Its owner has not opened it yet. Open it once |
| Virtual storage is off | Items fall on the ground around it. Switch it in the settings menu |
| `disable-xp-drops: true` | No XP at all, by design |
| A RoseStacker stack counts as one mob | Set `trigger-death-event-for-entire-stack-kill: true` in RoseStacker |
| The limit did not go up after a rank | Limits are worked out when the owner opens the menu. Open it again |

## The menu

| Problem | Cause and fix |
|---|---|
| Right-click does nothing | You are not the owner or a friend, or `open-gui-on-click` is `false` |
| Level Up finds nothing | The spawners in the inventory differ from the placed one, often because its name or lore was changed in the config after they were given out |
| Level Up refuses | A spawner is in the off hand or an armour slot |
| Selling pays less than expected | Items without a price count as 0, and are thrown away with the rest. See [Prices](config/prices.md) |
| The chat swallows a message | The player pressed **Add Friend** and the next message is read as a name. Type `cancel` |

## Breaking

| Problem | Cause and fix |
|---|---|
| Refused without a message | `break-management` wants the pickaxe. The mode names are misleading; see the table on [Breaking Rules](features/breaking.md#requiring-a-tool) |
| `Silk touch is required` | The mode wants a Silk Touch tool for this spawner |
| The spawner vanished | `block: false` without the right tool, or a failed drop chance roll |
| A vanilla spawner drops as vanilla | That is the default. `support-on-break: false` turns it into a uxmSpawners spawner instead |
| Spawners in a world cannot be placed or broken | The world is in `disabled-worlds` |
| A spawner and its storage disappeared after an explosion | Turn on `protect-spawners-from-explosions` |

## Holograms

| Problem | Fix |
|---|---|
| No holograms at all | Install FancyHolograms, DecentHolograms or CMI, or set `hologram.enabled: false` |
| One spawner has none | Its hologram was switched off from its menu |
| The storage bar is behind | It is redrawn from time to time, not on every change |

## Still stuck

Open a ticket in the UXPLIMA Discord with the plugin and server versions, the console from startup,
`logs/errors.txt`, and the settings you changed.

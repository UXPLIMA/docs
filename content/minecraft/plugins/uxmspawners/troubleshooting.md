---
title: Troubleshooting
order: 80
description: What to check when the plugin does not start, a spawner produces nothing, storage stays empty, selling or breaking goes wrong.
icon: life-buoy
---

Start with the console, and for a single spawner with `/usp debug` while you look at it: it says who
owns the spawner, what it produces, when it last ran and why it waits. Problems with single spawners
are also written to the files in `logs/`.

## The plugin does not start

| Console says | Fix |
|---|---|
| `License key is not set` | Set `license-key` in `config.yml`. No key yet: open a ticket in the UXPLIMA Discord |
| `License validation failed` | The key is wrong, not valid for this server, or UXPLIMA could not be reached |
| `Failed to load the runtime libraries` | The server could not download them from Maven Central. Allow it internet access for the first start |
| `Vault or an economy plugin could not be found` | Install Vault and an economy plugin |
| `... is not compatible with this version of uxmSpawners` | Update the plugin it names, or uxmSpawners |
| `Failed to load configuration` | A file has an error. The same line names the file and the problem |
| `v5.0.0 migration failed, refusing to start` | See [Upgrading to 5.0.0](getting-started/upgrading.md#the-update). Your 4.2.0 files and database were put back |

## A spawner produces nothing

| Cause | Fix |
|---|---|
| Nobody is within `range` | A player has to be close, or a WildLoaders chunk loader has to be in the chunk |
| The chunk is not loaded | Spawners only run in loaded chunks |
| It is stopped | The owner starts it again with the button in the bottom right of its menu |
| It has `max-living-mobs` alive | Kill its mobs, or turn Auto Kill on |
| Real mobs appear instead of loot | Auto Kill is off on that spawner. Turn it on from its menu, with `uxmspawners.autokill` |

## Storage stays empty or stops growing

| Cause | Fix |
|---|---|
| The storage is full | The limit is per item type. Raise it, give a rank, or sell |
| Virtual Storage is off | Items fall on the ground around it. Switch it in the Settings menu |
| `disable-xp-drops: true` | No XP at all, by design |
| A rank did not raise the limit | Limits follow the owner's permissions when they open the spawner or join. Ranks count only when given, not for being op |
| Mobs a player kills drop their normal loot | `loot-table-only-auto-kill: true` applies loot tables only to Auto Kill |

## The menu and selling

| Problem | Cause and fix |
|---|---|
| Right-click does nothing | You are not the owner or a friend, the claim plugin refuses you, or `open-gui-on-click` is `false`. Staff are reminded of `/usp bypass` |
| Level Up finds nothing | The spawners in the inventory differ from the placed one, often because its name or lore was changed after they were given out |
| Items stay after **Sell All** | They have no price. With `price-items-by: ITEM`, renamed items and the output of item spawners have none. See [Prices](config/prices.md) |
| No **Sell All** button | The spawner type has `sell-button: false` |
| Add Friend says the player was not found | They have never played on the server, or the name is misspelt |
| The chat swallows a message | The owner pressed **Add Friend** and the next line is read as a name. Type `cancel` |
| Players cannot add friends or toggle settings | They lack `uxmspawners.friends` or the toggle permissions, which are for ops until granted |

## Breaking and placing

| Problem | Cause and fix |
|---|---|
| `Empty this spawner first` | `break-with-storage: BLOCK`. Take or sell the storage, or change the setting |
| `You need a Spawner Pickaxe` or `Silk Touch` | `break-management.mode` asks for it. See [Breaking Rules](features/breaking.md#requiring-a-tool) |
| `This item was not made by the server` | The item fails its signature: made in creative mode, edited, or from before a lost database. See [Item Signing](features/item-signing.md) |
| `Spawners are disabled in` a world | The world is in `world-management` |
| `You can't place spawners here` | The claim plugin refuses it, or `disable-placement-outside-claims` is on |
| `No luck this time` | The drop chance roll failed. See [Breaking Rules](features/breaking.md#drop-chance) |
| A broken spawner gave a plain spawner, and its levels are gone | RoseStacker or WildStacker spawner stacking is on. See [Integrations](integrations.md#stackers) |
| Breaking is refused for a moment after a restart | The world's spawners are still loading |

## Holograms

| Problem | Fix |
|---|---|
| No holograms at all | Install PacketEvents and restart, or set `enabled: false` in `hologram.yml` |
| One spawner has none | Its owner switched it off in the Settings menu |
| Old hologram text above spawners after the update | It belongs to the hologram plugin 4.2.0 used. Remove those holograms there |

## Still stuck

Open a ticket in the UXPLIMA Discord with the plugin and server versions, the console from startup,
the output of `/usp debug` on the spawner, today's file from `logs/`, and the settings you changed.

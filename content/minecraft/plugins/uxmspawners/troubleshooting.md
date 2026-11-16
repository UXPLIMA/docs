---
title: Troubleshooting
order: 180
description: What to check when the plugin does not start, a spawner produces nothing, storage stays empty, selling or breaking goes wrong.
icon: life-buoy
---

Start with the console, and for a single spawner with `/usp debug` while you look at it: it says who
owns the spawner, what it produces, when it last ran and which spawn condition it waits for. Players
see the same in the clock of its menu. Problems with single spawners are also written to the files in
`logs/`.

## The plugin does not start

When uxmSpawners cannot start, it stops the server, unless `stop-server-on-failure` is `false`. The
console says why just before.

| Console says | Fix |
|---|---|
| The server lists uxmSpawners as missing a dependency | Install Vault and LuckPerms |
| `License key is not set` | Set `license-key` in `config.yml`. No key yet: open a ticket in the UXPLIMA Discord |
| `License validation failed` | The key is wrong, not valid for this server, or UXPLIMA could not be reached |
| `Failed to load the runtime libraries` | The server could not download them from Maven Central. Allow it internet access for the first start |
| `Vault or an economy plugin could not be found` | Install Vault and an economy plugin |
| `... is not compatible with this version of uxmSpawners` | Update the plugin it names, or uxmSpawners |
| `Failed to load configuration` | A file has an error. The same line names the file and the problem |
| A file is no valid YAML | Fix the file it names. Nothing was changed |
| An unknown `database.type` | Use `SQLITE` or `MYSQL` |
| `v5.0.0 migration failed, refusing to start` | See [Upgrading to 5.0.0](getting-started/upgrading.md#the-update). Your 4.2.0 files and database were put back |

## A spawner produces nothing

| Cause | Fix |
|---|---|
| A spawn condition does not hold | The clock in its menu shows which. By default a player has to be within 16 blocks, or a WildLoaders chunk loader in the chunk. See [Spawn Conditions](features/spawn-conditions.md) |
| The chunk is not loaded | Spawners only run in loaded chunks |
| It is stopped | The owner starts it again with the button in the bottom right of its menu |
| The world is on Peaceful | Spawners are off there, unless `work-in-peaceful` is on; then only Auto Kill spawners work |
| Its file is missing or has `enabled: false` | Put the file back or turn it on, then `/usp reload` |
| Its ItemsAdder or Nexo block is not known | The console names the block. Install or fix that plugin |
| The console names a `spawns` that is no mob or item | Fix `produces.spawns` in that spawner file |
| It has `max-living-mobs` or `max-mobs` alive | Kill its mobs, or turn Auto Kill on |
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
| Level Up finds nothing | The spawners in the inventory are of another kind: another spawner file, mob or mode |
| A menu does not open, `This menu is unavailable` | A menu file has an item or filler whose material is no item. The console names it |
| Nothing happens in the menu | The player walked away from the spawner, or clicked too fast |
| Items stay after **Sell All** | They have no price. With `price-items-by: ITEM`, renamed and custom items have none. See [Prices](config/prices.md) |
| No **Sell All** button, "Not for sale" | The spawner type has `sell-button: false` |
| `Your inventory is full` | A take is never more than fits. Make room |
| `This spawner has ... friends, the most you can add` | `max-friends`, or the owner's `uxmspawners.maxfriends.<amount>` |
| `This boost can't be used on this spawner` | The spawner's file is not in the boost's `useable-in` |
| Add Friend says the player was not found | They have never played on the server, or the name is misspelt |
| The chat swallows a message | A chat prompt is open: **Add Friend** or **Custom Amount**, and the next line is read as the answer. Type `cancel` |
| Players cannot add friends or toggle settings | They lack `uxmspawners.friends` or the toggle permissions, which are for ops until granted |

## Breaking and placing

| Problem | Cause and fix |
|---|---|
| `Empty this spawner first` | `break-with-storage: BLOCK`. Take or sell the storage, or change the setting |
| `This spawner stores too much to drop` | More than `dropped-storage.max-stacks` would drop. Sell or take some first |
| `Take some out of the storage first` | With `multiply-by-level` and `over-limit: DELETE`, a lower level could not hold the storage |
| `You need a Spawner Pickaxe` or `Silk Touch` | `break-management.mode` asks for it. See [Breaking Rules](features/breaking.md#requiring-a-tool) |
| `This item was not made by the server` | The item fails its signature: made in creative mode, edited, or signed by another server's key. See [Item Signing](features/item-signing.md) |
| `Spawners are disabled in` a world | The world is in `world-management`, or on Peaceful |
| Natural spawners cannot be broken | `disable-vanilla-breaking` is on: only `uxmspawners.breakvanillaspawners` can |
| A vanilla spawner item places an empty spawner | It was not dropped by uxmSpawners, or `place-with-mob` is off |
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

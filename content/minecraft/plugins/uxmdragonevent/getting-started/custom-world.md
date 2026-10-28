---
title: A custom event world
order: 13
description: Fighting the dragon in an end map you built, from loading the map to the first test event.
icon: map
---

A custom world is an end map you built or downloaded. The plugin keeps the original folder untouched
and fights every event in a fresh copy of it, so whatever players break or build is gone by the next
event.

## What you need

- The map's world folder, built as an end world. The plugin loads it as the End, whatever it was
  saved as.
- A world plugin to load the map once while you set the spawn points, such as Worlds or
  Multiverse-Core.

## Step by step

1. **Load the map.** Put the world folder in your server folder and load it with your world plugin,
   for example `/mv import mymap end` with Multiverse-Core.
2. **Set the player spawn.** Go to the map, stand where players should arrive, and run
   `/dragon setcustomspawn`.
3. **Set the dragon spawn.** Stand where the dragon should appear and run `/dragon setdragonspawn`.
4. **Save.** Run `/dragon save`. Only the position and the facing are stored, not the world's name,
   so the two points apply to every copy of the map.
5. **Unload the map.** Teleport out, save and unload the world with your world plugin, and stop the
   server.
6. **Move the folder.** Move the world folder into `plugins/uxmDragonEvent/`. The `uid.dat` file is
   skipped when the plugin copies it, so you do not need to delete it.
7. **Turn it on** in `config.yml`:

   ```yaml
   custom-world:
     enabled: true
     type: CUSTOM
     folder-name: "mymap"
   ```

   `folder-name` is the folder's exact name, capital letters included.

8. **Test.** Start the server, run `/dragon start`, join, and check that players and the dragon
   appear where you set them. Then `/dragon stop`.

## Things to know

| | |
|---|---|
| The copy | A new copy is made before every event, so changes you make to the original folder apply from the next event |
| The spawn platform | The plugin places a 5 by 5 obsidian platform at x 0, y 50, z -30 and clears the three blocks above it in every event world. Keep that spot free in your map |
| The world border | `world-border` applies to the copy. Set its centre to your arena. See [Worlds and the border](../features/worlds.md#the-world-border) |
| No dragon spawn | If `/dragon setdragonspawn` was never run, the event ends as soon as it starts and the console says why |
| No player spawn | Players arrive next to the map's own spawn point, or on the platform with Worlds and `spawn-type: PLATFORM`. Set the spawn so it does not depend on the world plugin |

You can also set or correct either point during a running event: `/dragon teleport` takes you into the
event's copy without joining, and the commands work there as well. Run `/dragon save` afterwards.

Related: [Worlds and the border](../features/worlds.md), [Troubleshooting](../troubleshooting.md)

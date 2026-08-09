---
title: Worlds
order: 1060
---

## Worlds Commands

The `worlds` module is a full multi-world manager: create, import, load, unload, configure, teleport between, pre-generate, back up, and restore worlds — all from `/worlds`, all through **Brigadier** with tab-completed world names, properties, and gamerules.

<Callout type="info" title="`/worlds` (plural) vs `/world` (singular)">

`/worlds` is the **management** command described here — it manages every world on the server and uses the `uxmessentials.world.*` permission tree.

`/world` (singular) is a completely different, player-facing command from the **Player State** module that simply tells you which world you are standing in. See [Player State](playerstate.md) for that one.

</Callout>

---

## Command Reference

| Command | Description | Permission |
|---------|-------------|------------|
| `/worlds` | Bare command: opens the world-list GUI (or prints usage) | `uxmessentials.world.use` (GUI: `uxmessentials.world.gui`) |
| `/worlds list` | List all managed worlds | `uxmessentials.world.list` |
| `/worlds info <name>` | Show a world's environment, generator, and properties | `uxmessentials.world.info` |
| `/worlds create <name> [env] [type] [gen]` | Create a new world | `uxmessentials.world.create` |
| `/worlds import <folder> <env> [gen…]` | Register an existing world folder | `uxmessentials.world.import` |
| `/worlds load <name>` | Load an unloaded world | `uxmessentials.world.load` |
| `/worlds unload <name>` | Unload a loaded world | `uxmessentials.world.unload` |
| `/worlds unregister <name>` | Drop a world from management without deleting its files | `uxmessentials.world.unregister` |
| `/worlds delete <name>` | Stage a world for permanent deletion | `uxmessentials.world.delete` |
| `/worlds confirm <name>` | Confirm a staged deletion | `uxmessentials.world.delete` |
| `/worlds set <name> <property> <value…>` | Set a world property | `uxmessentials.world.set` |
| `/worlds gamerule <name> <rule> <value>` | Set a world gamerule | `uxmessentials.world.gamerule` |
| `/worlds setspawn <name>` | Set a world's spawn to your current position | `uxmessentials.world.setspawn` |
| `/worlds spawn [name]` | Teleport to a world's spawn | `uxmessentials.world.spawn` |
| `/worlds tp <name> [player]` | Teleport yourself (or another player) to a world | `uxmessentials.world.tp` (+`uxmessentials.world.tp.others`) |
| `/worlds gui [name]` | Open the world-list / editor GUI | `uxmessentials.world.gui` |
| `/worlds pregen <name> <radius>` | Pre-generate chunks out to a radius | `uxmessentials.world.pregen` |
| `/worlds pregen cancel <name>` | Cancel a running pre-generation | `uxmessentials.world.pregen` |
| `/worlds backup <name>` | Snapshot a world | `uxmessentials.world.backup` |
| `/worlds backups <name>` | List a world's backups | `uxmessentials.world.backup` |
| `/worlds restore <name> <backup>` | Stage a restore from a backup | `uxmessentials.world.restore` |
| `/worlds restoreconfirm <name>` | Confirm a staged restore | `uxmessentials.world.restore` |

---

## Generators

When you create a world, the generator argument shapes what gets built:

- `void` — an empty world with nothing but the spawn platform you set. Ideal for lobbies, hubs, and creative plots.
- `flat` — a superflat world.

Leave the generator off for a normal world using the vanilla terrain generator for the chosen environment (`NORMAL`, `NETHER`, or `THE_END`).

```
/worlds create lobby NORMAL FLAT void
/worlds create skyblock NORMAL NORMAL void
```

---

<Callout type="warning" title="Delete and restore are staged — they need confirmation">

Destructive world operations happen in two steps so you cannot wipe or overwrite a world by accident:

- `/worlds delete <name>` stages the deletion; nothing is removed until you run `/worlds confirm <name>`.
- `/worlds restore <name> <backup>` stages the restore; the world is not overwritten until you run `/worlds restoreconfirm <name>`.

Always double-check the world name before you confirm — these actions cannot be undone.

</Callout>

---

## Examples

### Create and configure a lobby

```
/worlds create lobby NORMAL NORMAL void
/worlds gamerule lobby doDaylightCycle false
/worlds setspawn lobby
/worlds tp lobby
```

### Pre-generate a survival world

```
/worlds pregen survival 2000
/worlds pregen cancel survival
```

### Back up before a risky change

```
/worlds backup survival
/worlds backups survival
/worlds restore survival 2026-07-01
/worlds restoreconfirm survival
```

---

<Callout type="note" title="Rename or disable any of these">

Every command literal above can be renamed, re-aliased, or switched off in `commands/commands.conf` without touching permissions — see [Renaming Commands](../config/commands-conf.md).

</Callout>

## Next Steps

- [🌍 Worlds Feature Guide](../features/worlds.md) - Generators, properties, pre-gen, and backups explained
- [🧍 Player State](playerstate.md) - The single-world `/world` command and other self commands
- [🔐 Permission Reference](../permissions/reference.md) - Every `uxmessentials.world.*` node

---
title: Game Modes
order: 70
description: The twelve ways an island can play, how a server offers one or several, and where
  each one is set up.
icon: layers
---

uxmSkyblock plays twelve game modes. A server may offer one of them or any number side by side,
and every island plays the mode it was made in for the rest of its life.

| Mode | The island | Its file |
| --- | --- | --- |
| [Skyblock](skyblock.md) | The classic: a small island in the void | `modules/presets.conf` |
| [OneBlock](oneblock.md) | One block that comes back as something new each time it breaks | `modules/oneblock.conf` |
| [ChunkBlock](chunkblock.md) | One chunk of land, the next ones bought and earned | `modules/chunkblock.conf` |
| [AcidIsland](acidisland.md) | An island in a sea of acid that rises and falls | `modules/acidisland.conf` |
| [CaveBlock](caveblock.md) | A cave carved out of solid rock | `modules/caveblock.conf` |
| [SkyGrid](skygrid.md) | A grid of single blocks hanging in the void | `modules/skygrid.conf` |
| [Boxed](boxed.md) | Real land inside a box that grows with advancements | `modules/boxed.conf` |
| [Poseidon](poseidon.md) | An island on the sea floor, where water is the air | `modules/poseidon.conf` |
| [StrangerRealms](strangerrealms.md) | Real land with an Upside Down in place of the Nether | `modules/strangerrealms.conf` |
| [Parkour](parkour.md) | A course with a start plate, checkpoints and best times | `modules/parkour.conf` |
| [Brix](brix.md) | A creative building plot with contests | `modules/brix.conf` |
| [TradeWinds](tradewinds.md) | A vessel that sails between ports and trades | `modules/tradewinds.conf` |

Two more pages belong with them: [Loot Tables](loot-tables.md), the tables the modes fill their
containers from, and [What a Player Carries](player-state.md), how a server of several modes keeps
each world group's inventory apart.

## Offering modes

The kinds of island a player can start are the presets of `modules/presets.conf`. Each preset
names the mode it plays with `mode`. The list `presets.modes` at the top of that file names the
modes this server offers:

```hocon
presets {
    modes = [ skyblock, oneblock, chunkblock ]
}
```

- Only the kinds of the modes in that list are shown in the island window and made by
  `/is create`.
- `"all"` offers every mode.
- A mode taken off the list still runs for the islands that already play it, so nobody is
  stranded.
- When only one kind is left to offer, `/is` makes it at once with no window.

A mode also has its own file, and that file's `enabled` must be `true` for the mode to run. A
preset whose start list names an action of a mode that is off is not offered.

## Starting an island

A player with no island types `/is`. What happens then is `presets.when-no-island` in
`modules/presets.conf`: `menu` opens the window of kinds, `menus/island-create.conf`, `create`
makes the default preset at once, and `help` lists the commands. `create-without-type` says the
same for `/is create` with no kind written, `menu` or `default`, and `default` names the preset a
player who names none gets.

In the window each kind is a tile. A Bedrock player who taps one reads the kind's description and
whether they may start it, with the buttons **Start this island**, **Look at it first** and
**Back to the kinds**. On Java a right click on a tile looks at it first.

A look shows the kind's start island from above, as a spectator for `preview.time`, then puts the
player back where they stood, as they were. Each kind stands in a showroom of its own, laid the
first time anybody looks, at the corner `preview` names. A player hurt within `preview.calm`,
falling or on fire cannot look, so a look is no way out of a fight or a fall.

A preset may name a `permission`. Name any node you like and give it to whom you like. The tile
of a preset a player may not start says it is locked.

## Where islands are made

A preset without `world` makes its islands in the island world `config.conf` names. A mode played
on land of its own, Boxed and StrangerRealms, names a world the server makes the usual way. Each
such world is an island world: its islands are protected, kept alive and scanned there.

On land the server generated, the planned arrival height can be inside a hill, over a drop or in
the sea. The player arrives on the nearest ground in the island's centre chunk, and the island's
spawn is moved there. A chunk that is all sea sets the player at the top of the water.

## Other dimensions

`dimensions` in a preset says how the island starts in the Nether, the End, or any dimension a
server adds, each with its own start list and the height players arrive at. A preset without it
lays a platform at height 64 in the Nether and the End. `modules/dimensions.conf` names the
worlds and whether each is a private copy per island or one shared wilderness.

## Bedrock

Every window here is a native form for a Bedrock player through Floodgate: the kinds, each kind's
page, the island menu and every mode's own window. A line a Java player reads in the world, as
over a OneBlock block, comes to a Bedrock player on the action bar.

## Commands for operators

`/isadmin` has a branch for each mode the server runs, and every verb asks a permission of its
own: `uxmskyblock.admin.<mode>.<verb>`. Each mode's page lists its verbs. The words are yours to
rename in `commands.conf` under `isadmin`.

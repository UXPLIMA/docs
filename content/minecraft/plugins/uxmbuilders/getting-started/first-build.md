---
title: Your first build
order: 104
description: "A schematic, a package, a blueprint and a confirmed build: start to finish."
icon: play
---

## 1. Save the structure

Stand in front of a house you built, select it with WorldEdit, and save it into the plugin's folder:

```
//wand
/builders admin save oak_house
```

The file lands in `plugins/uxmBuilders/schematics/oak_house.schem`. Air inside the selection is kept,
which matters: [site preparation](../features/site-preparation.md) clears whatever stands where the
schematic has air. `--overwrite` replaces an existing file of the same name.

A `.schem` from anywhere else works the same way: drop it into `schematics/`.

## 2. Create the package

```
/builders admin package create oak_house oak_house.schem 1500
```

This writes a package into `packages.yml` with the price `1500`, the first category, and the seed
display name and icon from `admin.newPackage` in `config.yml`. Give it a proper name and icon:

```
/builders admin package edit oak_house display <gold>Oak Cottage</gold>
/builders admin package edit oak_house icon OAK_PLANKS
/builders admin reload
```

`/builders admin package info oak_house` prints the size, block count and material list.

## 3. Buy the blueprint

As a player:

```
/builders shop
```

Open the category, click the package, then confirm. The confirm button unlocks after
`gui.confirmDelayTicks` so a double-click cannot buy twice. The money is taken now, and the blueprint
goes into the inventory.

## 4. Place it

Hold the blueprint and right-click the ground. The preview opens: the whole house drawn in your client,
with a particle outline.

| Input | Does |
|---|---|
| Look around | The structure follows the block you look at |
| Left-click | Rotate by 90 degrees |
| Shift + left-click | Mirror |
| Shift + scroll one step | Raise or lower it by one block, up to 5 |
| `/builders offset <x> <y> <z>` | Fine adjustment, up to 5 blocks per axis |
| Right-click | Confirm |

A red outline means the spot is not allowed, and the chat says why: protected land, a forbidden world,
outside the height limits, or too close to spawn.

## 5. Watch it go up

After confirming, the chat prints the job id and block count. A boss bar near the site and an action
bar show progress, and the builder NPC appears and starts placing blocks.

```
/builders status
```

Lists your running builds with a progress bar and the time left. `/builders` opens the main menu, where
**Active Builds** lets you pause, resume, boost or cancel each one.

## 6. Check the result

When the last block lands the title reads **Construction Complete**, the NPC leaves, and a firework goes
off. The build appears in `/builders history`.

<Callout type="tip" title="Test with a fast builder first">

`/builders admin setbuilder <player> wizard 5` gives a test account the fastest shipped builder, so you
can confirm the package end to end in seconds before tuning prices and levels.

</Callout>

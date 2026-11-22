---
title: Claim Block
order: 1308
description: The physical block that marks a land, and claim items that make a land where they are used.
icon: box
---

The claim block is a block placed at the land spawn when a land is created. Nobody can break it, and a member who right-clicks it runs a command, by default the land menu. The same module also offers claim items: items that make a new land where a player right-clicks with them.

Module `claim-block` · enabled by default · `modules/claim-block/config.yml` · requires [Lands](lands.md)

## Commands

| Command | What it does | Permission |
|---|---|---|
| `/claim block` | Opens the claim block menu | `uxmclaims.command.claim-block.menu` |
| `/claim block place [type]` | Places the claim block where you stand (or the nearest free block) | `uxmclaims.command.claim-block.place` |
| `/claim block change <type>` | Changes the block type | `uxmclaims.command.claim-block.change` |
| `/claim block destroy` | Takes the claim block away | `uxmclaims.command.claim-block.destroy` |
| `/uxm claim-block cleanup` | Removes claim blocks of lands that no longer exist | `uxmclaims.admin.claim-block` |
| `/uxm claim-block cleanup all` | Takes every claim block out of the world | `uxmclaims.admin.claim-block` |
| `/claim admin bulk setblock <type>` | Sets the type of every placed claim block | `uxmclaims.admin` |
| `/claim admin bulk destroyblock` | Takes away every placed claim block | `uxmclaims.admin` |
| `/claim admin giveitem <player> <tier> [amount]` | Gives an online player claim items of a tier | `uxmclaims.admin` |

The `/claim block` commands act on the land you stand in. Their command nodes are open to everyone by default;
place, change and destroy also need the ability node of the same name and the land role.

## Permissions

| Node | Default | Grants |
|---|---|---|
| `uxmclaims.command.claim-block.<sub>` | everyone | The command of the table above (`menu` is the plain `/claim block`) |
| `uxmclaims.ability.block.place` | everyone | Place the claim block |
| `uxmclaims.ability.block.change` | everyone | Change the claim block type |
| `uxmclaims.ability.block.destroy` | everyone | Remove the claim block |
| `uxmclaims.admin.claim-block` | op | The `/uxm claim-block` commands and every land's claim block |
| `blocks.<key>.permission` | as set | Choose that block type (shipped: `claim.blockchange.obsidian` for `block2`) |

| Role permission | Default holder | Allows |
|---|---|---|
| `claim-block:manage` (3.x `MANAGE_BLOCK`) | Owner | Place, change and destroy the land's claim block |

## Settings

`config.yml`:

| Key | Default | What it does |
|---|---|---|
| `place-on-create` | `true` | Place the default block at the land spawn when a land is created; `false` leaves it to `/claim block place` |
| `right-click-command` | `claim menu` | Command a member runs by right-clicking the block, without the slash; `<land>` is the land name; empty does nothing |
| `protect-from-explosions` | `true` | Explosions leave claim blocks alone |
| `protect-from-pistons` | `true` | Pistons do not move claim blocks |

`blocks.yml`:

| Key | Default | What it does |
|---|---|---|
| `default-block` | `block1` | The type new lands get |
| `blocks.<key>.material` | `BEDROCK`, `OBSIDIAN` | The block placed in the world |
| `blocks.<key>.permission` | empty, `claim.blockchange.obsidian` | Node needed to choose the type; empty means everyone |
| `blocks.<key>.icon` | the material | Material shown in menus |

Type keys are stored in the database: never rename one. Menu names and lore are in `lang/<code>/messages.yml` under `blocks.<key>`.

`claim-items.yml`:

| Key | Default | What it does |
|---|---|---|
| `enabled` | `false` | Turns claim items on |
| `tiers.<tier>.material` | `EMERALD_BLOCK` | The item |
| `tiers.<tier>.radius` | `1` | Chunks around the used one that join the land: `0` one chunk, `1` 3x3, `2` 5x5 |
| `tiers.<tier>.free` | `true` | The land and chunks cost nothing; `false` charges the normal prices |
| `tiers.<tier>.refund-on-unclaim` | `true` | When the land is deleted, the item goes back to its owner (at the next join when offline) |
| `tiers.<tier>.all-or-nothing` | `true` | When the chunks around cannot all be claimed: the land is deleted again and the player keeps the item. `false`: the land stays with only the chunk the item was used in, and the item is used up |
| `tiers.<tier>.recipe.enabled` | `false` | Let players craft the item |
| `tiers.<tier>.recipe.shape` | `EEE`, `EDE`, `EEE` | Crafting rows; a space is an empty slot |
| `tiers.<tier>.recipe.ingredients` | `E: EMERALD`, `D: DIAMOND` | Letter to material |

Tier keys are stored on the items: never rename one. Item names and lore are under `claim-item.tiers.<tier>` in the language file.

## Placeholders

All read the player's current land.

| Placeholder | Renders |
|---|---|
| `%uxmclaims_claim-block_placed%` | `true` when the land's block is placed |
| `%uxmclaims_claim-block_key%` | Type key of the block |
| `%uxmclaims_claim-block_name%` | Display name of the type, else the key |
| `%uxmclaims_claim-block_x%`, `_y`, `_z` | Block position |

## Notes

- Nobody breaks a claim block by hand, admins included; use `/claim block destroy`.
- Before you turn the module off, run `/uxm claim-block cleanup all`, or the blocks stay in the world.
- Deleting a land takes its block out of the world; restoring the land puts it back.
- With `all-or-nothing: false`, a land that cannot take all chunks around keeps only its first chunk, since chunks around are claimed in one step.
- A claim item keeps the text it was made with, in the server language. A land restored after its item was refunded leaves the item with the player.
- On a network, claim block holograms are not redrawn when another server renames a land or moves its block.

Related: [Lands](lands.md), [Holograms](holograms.md), [Economy](economy.md)

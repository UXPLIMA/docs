---
title: Role permissions
order: 303
description: All 48 role permissions, and which the built-in roles hold.
icon: key
---

A role permission decides what one player may do inside one claim. They are granted to a role with
`/claim role setperm <role> <perm> <true|false>`, or overridden on one member with
`/claim member setperm <player> <perm> <true|false>`.

The **Member** column is whether the built-in `Member` role holds it out of the box, from
[`roles.yml`](../config/roles-yml.md). `Owner` holds everything implicitly. `Default` holds
`MOVE_INSIDE` and nothing else.

## Management

Nine permissions that let a member change the claim itself. None are granted to `Member` by default.

| Permission | Grants | Member |
|---|---|---|
| `MANAGE_CHUNKS` | Add and remove chunks | no |
| `MANAGE_TIME` | Extend the claim's lifetime | no |
| `MANAGE_BANS` | Ban and unban players | no |
| `MANAGE_INVITES` | Send and revoke invitations | no |
| `MANAGE_VAULT` | Open and change the shared vault | no |
| `MANAGE_BLOCK` | Place, change and destroy the claim block | no |
| `MANAGE_RENAME` | Rename the claim | no |
| `MANAGE_RELOCATE` | Move the claim spawn | no |
| `MANAGE_WARPS` | Create, move, rename, delete and publish warps | no |

## Build and blocks

| Permission | Grants | Member |
|---|---|---|
| `BLOCK_PLACE` | Place blocks | **yes** |
| `BLOCK_BREAK` | Break blocks | **yes** |
| `SIGN_EDIT` | Write on signs | **yes** |
| `BUCKET_FILL` | Fill a bucket from a source | **yes** |
| `BUCKET_EMPTY` | Empty a bucket | **yes** |
| `HANGING_PLACE` | Hang paintings, item frames and banners | **yes** |
| `HANGING_BREAK` | Take them down | **yes** |
| `TRAMPLE_CROPS` | Turn farmland back to dirt by jumping on it | no |
| `SPAWNER_PLACE` | Place a mob spawner | no |
| `SPAWNER_DESTROY` | Break a mob spawner | no |

## Interaction

| Permission | Grants | Member |
|---|---|---|
| `CONTAINER_OPEN` | Open chests, barrels, shulkers, furnaces and hoppers | **yes** |
| `USE_REDSTONE` | Use levers, buttons, pressure plates and repeaters | **yes** |
| `USE_MECHANISMS` | Use doors, trapdoors, gates and other openables | **yes** |
| `ARMOR_STAND_MANIPULATE` | Take from and give to armour stands | **yes** |
| `SLEEP` | Use a bed | **yes** |
| `IGNITE` | Light fires with flint and steel or a fire charge | no |
| `TAKE_LECTERN_BOOK` | Take the book off a lectern | no |

## Entities

| Permission | Grants | Member |
|---|---|---|
| `ANIMAL_DAMAGE` | Hurt passive mobs | **yes** |
| `MONSTER_DAMAGE` | Hurt hostile mobs | **yes** |
| `ANIMAL_INTERACT` | Feed, breed, leash and name passive mobs | **yes** |
| `SHEAR_ENTITY` | Shear sheep and mooshrooms | **yes** |
| `VILLAGER_TRADE` | Trade with villagers | **yes** |
| `VEHICLE_INTERACT` | Place, break and enter boats and minecarts | **yes** |
| `RIDE_ENTITY` | Mount horses, pigs, striders and camels | **yes** |
| `POTION_USE` | Throw splash and lingering potions | **yes** |

## Movement and miscellaneous

| Permission | Grants | Member |
|---|---|---|
| `MOVE_INSIDE` | Enter the claim at all | **yes** |
| `TELEPORT` | Teleport into the claim | **yes** |
| `USE_WARPS` | Use the claim's warps | no |
| `ENDERPEARL_USE` | Throw ender pearls here | **yes** |
| `CHORUS_TELEPORT` | Teleport by eating a chorus fruit | **yes** |
| `ELYTRA_USE` | Fly with an elytra | **yes** |
| `WIND_BURST` | Use the wind burst enchantment | **yes** |
| `WIND_CHARGE` | Throw wind charges | **yes** |
| `ITEM_DROP` | Drop items | **yes** |
| `ITEM_PICKUP` | Pick items up | **yes** |
| `FISHING` | Fish, and use the rod on entities | **yes** |
| `RAID_TRIGGER` | Start a raid by entering with Bad Omen | no |
| `CLAIM_CHAT` | Use `/claim chat` | no |
| `FLY` | Fly inside the claim | no |

## Notes

- **`MOVE_INSIDE` is the strongest one there is.** Take it off `Default` and the claim becomes a
  no-entry zone for everyone who is not a member. It is what `Default` ships with and, on most
  servers, all it should ship with.

- **`ITEM_PICKUP` without `ITEM_DROP` is a trap.** A visitor who can pick items up but not drop them
  can clear a floor and walk away with it. Grant them together or not at all.

- **`CONTAINER_OPEN` is one permission for every container.** There is no separate chest and furnace
  node. A role that may use a furnace may also open the chest next to it.

- **`SPAWNER_PLACE` and `SPAWNER_DESTROY` are separate from `BLOCK_PLACE` and `BLOCK_BREAK`** on
  purpose, and neither is granted to `Member`. On a server where spawners are an economy item, a
  member who may build still cannot walk off with the grinder.

- **`RAID_TRIGGER` off protects a village claim** from a member arriving with Bad Omen, deliberately
  or otherwise.

- **`FLY` is a permission, not a flag,** so it is per player. It grants flight inside the claim only.

- **A denial on the member beats the role.** `/claim member setperm Steve CONTAINER_OPEN false` holds
  even if Steve's role grants it, and even if you later promote him.

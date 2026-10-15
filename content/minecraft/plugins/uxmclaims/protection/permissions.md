---
title: Role permissions
order: 303
description: All 48 role permissions, what each one gates, and which the built-in roles hold.
icon: key
---

A role permission decides what one player may do inside one claim. The owner grants it to a role with
`/claim role setperm <role> <permission> <true|false>`, or overrides it for one member with
`/claim member setperm <player> <permission> <true|false>`. Changing either also needs the ability node
`uxmclaims.ability.role.permissions.<permission>` or `uxmclaims.ability.member.permissions.<permission>`,
in lower case.

The **Member** column is whether the built-in `Member` role holds the permission in the shipped
[`roles.yml`](../config/roles-yml.md). `Owner` holds everything implicitly. `Default`, the role of
everyone who is not a member, holds `MOVE_INSIDE` and nothing else. A custom role starts with nothing.

## Management

Nine permissions let a member run part of the claim. Changing flags, roles, member roles and overrides,
transferring and deleting stay with the owner whatever a role holds.

| Permission | Grants | Member |
|---|---|---|
| `MANAGE_CHUNKS` | Add and remove chunks | no |
| `MANAGE_TIME` | Extend the claim's time | no |
| `MANAGE_BANS` | Ban, unban and kick players | no |
| `MANAGE_INVITES` | Send and revoke invites | no |
| `MANAGE_VAULT` | Open the shared vault at all | no |
| `MANAGE_BLOCK` | Place, change and destroy the claim block | no |
| `MANAGE_RENAME` | Rename the claim | no |
| `MANAGE_RELOCATE` | Move the claim spawn with `/claim setspawn` | no |
| `MANAGE_WARPS` | Create, move, rename and delete warps, and make them public or private | no |

## Build and blocks

| Permission | Grants | Member |
|---|---|---|
| `BLOCK_PLACE` | Place blocks, armor stands and end crystals; use bone meal | **yes** |
| `BLOCK_BREAK` | Break blocks; pick sweet berries; trample turtle eggs | **yes** |
| `SIGN_EDIT` | Write on signs | **yes** |
| `BUCKET_FILL` | Fill a bucket | **yes** |
| `BUCKET_EMPTY` | Empty a bucket | **yes** |
| `HANGING_PLACE` | Hang paintings and item frames | **yes** |
| `HANGING_BREAK` | Break paintings and item frames | **yes** |
| `TRAMPLE_CROPS` | Turn farmland back to dirt by jumping on it | no |
| `SPAWNER_PLACE` | Place a spawner, or change its mob with a spawn egg | no |
| `SPAWNER_DESTROY` | Break a spawner | no |

## Interaction

| Permission | Grants | Member |
|---|---|---|
| `CONTAINER_OPEN` | Open and use any block with an inventory: chests, barrels, shulker boxes, furnaces, hoppers, dispensers, brewing stands; fill and empty cauldrons | **yes** |
| `USE_REDSTONE` | Buttons, levers, pressure plates, tripwire, repeaters, comparators, note blocks, daylight detectors, targets; also a player's arrow setting one off | **yes** |
| `USE_MECHANISMS` | Doors, trapdoors, fence gates, bells, and other right-click blocks: cakes, candles, campfires, composters, flower pots, jukeboxes, lecterns, beehives, decorated pots, chiseled bookshelves, respawn anchors, the dragon egg | **yes** |
| `ARMOR_STAND_MANIPULATE` | Take from, dress and hit armor stands | **yes** |
| `SLEEP` | Use a bed | **yes** |
| `IGNITE` | Start a fire with flint and steel or a fire charge | no |
| `TAKE_LECTERN_BOOK` | Take the book off a lectern | no |

## Entities

| Permission | Grants | Member |
|---|---|---|
| `ANIMAL_DAMAGE` | Hurt animals and every other mob that is not hostile, villagers and golems included; knock an item out of an item frame | **yes** |
| `MONSTER_DAMAGE` | Hurt hostile mobs | **yes** |
| `ANIMAL_INTERACT` | Right-click, feed, breed, tame, leash, tempt and bucket animals; put or turn an item in an item frame | **yes** |
| `SHEAR_ENTITY` | Shear sheep, mooshrooms and other shearable mobs | **yes** |
| `VILLAGER_TRADE` | Right-click villagers to trade | **yes** |
| `VEHICLE_INTERACT` | Place, enter, hit and break boats and minecarts | **yes** |
| `RIDE_ENTITY` | Mount horses, pigs, striders, camels and other rideable mobs | **yes** |
| `POTION_USE` | Throw splash and lingering potions | **yes** |

## Movement and miscellaneous

| Permission | Grants | Member |
|---|---|---|
| `MOVE_INSIDE` | Be inside the claim at all | **yes** |
| `TELEPORT` | Arrive by a teleport from another plugin or a command, such as `/tp` or a home. The claim's own spawn and warp teleports do not check it, and portals are never refused | **yes** |
| `USE_WARPS` | Teleport to the claim's private warps, by command or from the warp manage menu. Public warps need nothing | **yes** |
| `ENDERPEARL_USE` | Land an ender pearl in the claim | **yes** |
| `CHORUS_TELEPORT` | Teleport into the claim by eating a chorus fruit | **yes** |
| `ELYTRA_USE` | Start gliding with an elytra | **yes** |
| `WIND_BURST` | Let a mace's Wind Burst affect blocks here | **yes** |
| `WIND_CHARGE` | Throw wind charges, and let them affect blocks here | **yes** |
| `ITEM_DROP` | Drop items, swap hands, and move items into the armor or offhand slots | **yes** |
| `ITEM_PICKUP` | Pick items up | **yes** |
| `FISHING` | Fish, and hook entities with a rod | **yes** |
| `RAID_TRIGGER` | Start a raid by arriving with Bad Omen | no |
| `CLAIM_CHAT` | Use `/claim chat` | no |
| `FLY` | Read by other plugins, such as a fly addon, through the [API](../developer/api.md). uxmClaims itself does not act on it | no |

## Notes

- **`MOVE_INSIDE` is the strongest one there is.** A player without it is bounced at the border and
  ejected by the access guard, see [How protection works](overview.md). Take it off `Default` and the
  claim becomes a no-entry zone for everyone who is not a member.
- **`ITEM_DROP` also guards armor and the offhand.** A role without it cannot put armor on or swap hands
  inside the claim.
- **`CONTAINER_OPEN` is one permission for every container.** A role that may use a furnace may also open
  the chest next to it.
- **Item frames span three permissions.** `HANGING_BREAK` breaks the frame, `ANIMAL_DAMAGE` knocks the item
  out, `ANIMAL_INTERACT` puts an item in or turns it.
- **`SPAWNER_PLACE` and `SPAWNER_DESTROY` are separate from `BLOCK_PLACE` and `BLOCK_BREAK`**, and
  `Member` holds neither. A member who may build still cannot take the spawner out of a grinder.
- **A member override is relative to the role.** `setperm ... false` records a deny only when the
  member's role grants the permission, and `setperm ... true` records an allow only when it does not.
  Recorded overrides stay when the member changes role, and a deny beats any role.

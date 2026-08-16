---
title: All Role Permissions
order: 320
icon: key
---

## Overview

Role permissions control what **members** can do inside a specific claim. These are managed per-claim using the Roles system.

<Callout type="info" title="Role vs Ability Permissions">

- **Role Permissions:** Controls what members can do *inside a claim* (breaking blocks, opening chests)
- **Ability Permissions:** Controls who can perform plugin actions (creating claims, inviting members) → [See Ability Permissions](../integrations/ability-permissions.md)

</Callout>

---

## Quick Reference Table

| Permission | Default | What It Controls |
|------------|---------|------------------|
| MANAGE_CHUNKS | None | Buying new chunks, removing chunks |
| MANAGE_TIME | None | Extending claim duration |
| MANAGE_BANS | None | Banning and unbanning players |
| MANAGE_INVITES | None | Sending and revoking invitations |
| MANAGE_VAULT | None | Full access to claim vault |
| MANAGE_BLOCK | None | Placing and breaking the claim block |
| MANAGE_RENAME | None | Changing the claim's name |
| MANAGE_RELOCATE | None | Moving the claim spawn point |
| MANAGE_WARPS | None | Creating, editing, and deleting warps |
| BLOCK_PLACE | Members | Placing any block |
| BLOCK_BREAK | Members | Breaking any block |
| SIGN_EDIT | Members | Editing text on signs |
| BUCKET_FILL | Members | Filling buckets |
| BUCKET_EMPTY | Members | Emptying buckets |
| HANGING_PLACE | Members | Placing item frames, paintings |
| HANGING_BREAK | Members | Breaking item frames, paintings |
| TRAMPLE_CROPS | None | Trampling crops |
| SPAWNER_PLACE | None | Placing mob spawners |
| SPAWNER_DESTROY | None | Breaking mob spawners |
| USE_REDSTONE | Members | Using buttons, levers, pressure plates |
| USE_MECHANISMS | Members | Using doors, gates, trapdoors |
| CONTAINER_OPEN | Members | Opening chests and containers |
| IGNITE | None | Using flint and steel |
| TAKE_LECTERN_BOOK | None | Taking books from lecterns |
| ARMOR_STAND_MANIPULATE | Members | Changing armor stand equipment |
| SLEEP | Members | Using beds |
| ANIMAL_DAMAGE | Members | Harming passive mobs |
| MONSTER_DAMAGE | Members | Harming hostile mobs |
| ANIMAL_INTERACT | Members | Feeding/breeding animals |
| SHEAR_ENTITY | Members | Shearing sheep/golems |
| VILLAGER_TRADE | Members | Trading with villagers |
| VEHICLE_INTERACT | Members | Entering boats, minecarts |
| RIDE_ENTITY | Members | Riding horses, pigs, striders |
| POTION_USE | Members | Using potions on entities |
| MOVE_INSIDE | Everyone | Entering the claim |
| TELEPORT | Members | Using claim teleport (spawn point) |
| USE_WARPS | Members | Using claim warp points |
| ENDERPEARL_USE | Members | Throwing ender pearls |
| ELYTRA_USE | Members | Flying with elytra |
| CHORUS_TELEPORT | Members | Chorus fruit random teleportation |
| ITEM_DROP | Members | Dropping items on the ground |
| ITEM_PICKUP | Members | Picking up items from the ground |
| FISHING | Members | Fishing |
| RAID_TRIGGER | None | Triggering village raids |
| CLAIM_CHAT | Members | Sending messages in claim chat |
| FLY | None | Flying inside the claim |

---

## 🔧 Management Permissions (9)

These allow administrative actions on the claim.

### MANAGE_CHUNKS

**Allows:** Buying new chunks, removing chunks

**Who needs it:** Trusted members who can expand the claim

---

### MANAGE_TIME

**Allows:** Extending claim duration

**Who needs it:** Members who pay for upkeep

---

### MANAGE_BANS

**Allows:** Banning and unbanning players

**Who needs it:** Moderators

---

### MANAGE_INVITES

**Allows:** Sending and revoking invitations

**Who needs it:** Anyone who recruits new members

---

### MANAGE_VAULT

**Allows:** Full access to claim vault

**Who needs it:** Trusted members only (vault contains valuables!)

<Callout type="note">

The player also needs `uxmclaims.ability.vault.access` Bukkit permission.

</Callout>

---

### MANAGE_BLOCK

**Allows:** Placing and breaking the claim block

**Who needs it:** Very trusted members (this affects the claim center)

---

### MANAGE_RENAME

**Allows:** Changing the claim's name

**Who needs it:** Trusted members or the owner only

---

### MANAGE_RELOCATE

**Allows:** Moving the claim spawn point

**Who needs it:** Trusted members who organize the claim

---

### MANAGE_WARPS

**Allows:** Creating, editing, and deleting warps

**Who needs it:** Members who set up teleport points

---

## 🧱 Build Permissions (10)

These control block manipulation.

### BLOCK_PLACE

**Allows:** Placing any block

**Default for:** Members ✅

---

### BLOCK_BREAK

**Allows:** Breaking any block

**Default for:** Members ✅

---

### SIGN_EDIT

**Allows:** Editing text on signs

**Default for:** Members ✅

---

### BUCKET_FILL

**Allows:** Filling buckets with water, lava, milk, etc.

**Default for:** Members ✅

---

### BUCKET_EMPTY

**Allows:** Emptying buckets (placing water, lava)

**Default for:** Members ✅

---

### HANGING_PLACE

**Allows:** Placing item frames, paintings

**Default for:** Members ✅

---

### HANGING_BREAK

**Allows:** Breaking item frames, paintings

**Default for:** Members ✅

---

### TRAMPLE_CROPS

**Allows:** Running on farmland (destroys crops)

**Default for:** Members ❌ (you might want this OFF)

---

### SPAWNER_PLACE

**Allows:** Placing mob spawners

**Default for:** Members ❌ (rare/valuable)

---

### SPAWNER_DESTROY

**Allows:** Breaking mob spawners

**Default for:** Members ❌ (rare/valuable)

---

## 🔌 Interaction Permissions (7)

These control interactions with world objects.

### USE_REDSTONE

**Allows:** Using buttons, levers, pressure plates

**Default for:** Members ✅

---

### USE_MECHANISMS

**Allows:** Using doors, gates, trapdoors

**Default for:** Members ✅

---

### CONTAINER_OPEN

**Allows:** Opening chests, barrels, shulkers, hoppers, furnaces, etc.

**Default for:** Members ✅

---

### IGNITE

**Allows:** Using flint and steel

**Default for:** Members ❌ (fire is dangerous)

---

### TAKE_LECTERN_BOOK

**Allows:** Taking books from lecterns

**Default for:** Members ❌

---

### ARMOR_STAND_MANIPULATE

**Allows:** Changing armor stand equipment and poses

**Default for:** Members ✅

---

### SLEEP

**Allows:** Using beds

**Default for:** Members ✅

---

## 🐾 Entity Permissions (8)

These control interactions with living entities.

### ANIMAL_DAMAGE

**Allows:** Harming passive mobs (cows, pigs, chickens)

**Default for:** Members ✅

---

### MONSTER_DAMAGE

**Allows:** Harming hostile mobs

**Default for:** Members ✅ (everyone needs to defend themselves!)

---

### ANIMAL_INTERACT

**Allows:** Feeding, breeding, milking cows

**Default for:** Members ✅

---

### SHEAR_ENTITY

**Allows:** Using shears on sheep, mooshrooms, snow golems

**Default for:** Members ✅

---

### VILLAGER_TRADE

**Allows:** Trading with villagers

**Default for:** Members ✅

---

### VEHICLE_INTERACT

**Allows:** Entering boats, minecarts

**Default for:** Members ✅

---

### RIDE_ENTITY

**Allows:** Riding horses, pigs, striders

**Default for:** Members ✅

---

### POTION_USE

**Allows:** Using splash/lingering potions on entities

**Default for:** Members ✅

---

## 🚶 Movement & Misc Permissions (12)

These control movement and miscellaneous actions.

### MOVE_INSIDE

**Allows:** Entering the claim at all

**Default for:** Everyone ✅ (including visitors)

<Callout type="note">

Remove this to create private claims

</Callout>

---

### TELEPORT

**Allows:** Using claim teleport (spawn point)

**Default for:** Members ✅

---

### USE_WARPS

**Allows:** Using claim warp points

**Default for:** Members ✅

---

### ENDERPEARL_USE

**Allows:** Throwing ender pearls inside the claim

**Default for:** Members ✅

---

### ELYTRA_USE

**Allows:** Flying with elytra

**Default for:** Members ✅

---

### CHORUS_TELEPORT

**Allows:** Chorus fruit random teleportation

**Default for:** Members ✅

---

### ITEM_DROP

**Allows:** Dropping items on the ground

**Default for:** Members ✅

---

### ITEM_PICKUP

**Allows:** Picking up items from the ground

**Default for:** Members ✅

---

### FISHING

**Allows:** Fishing

**Default for:** Members ✅

---

### RAID_TRIGGER

**Allows:** Triggering village raids (Bad Omen effect)

**Default for:** Members ❌ (raids are destructive!)

---

### CLAIM_CHAT

**Allows:** Sending messages in the claim chat channel

**Default for:** Members ✅

---

### FLY

**Allows:** Flying inside the claim (creative/fly mode)

**Default for:** Members ❌ (usually reserved for trusted members)

---

## Role Examples

### Builder Role

Focus on construction:

```
BLOCK_PLACE ✅
BLOCK_BREAK ✅
SIGN_EDIT ✅
BUCKET_FILL ✅
BUCKET_EMPTY ✅
HANGING_PLACE ✅
HANGING_BREAK ✅
MOVE_INSIDE ✅
TELEPORT ✅
```

### Visitor Role (Strict)

Minimal access:

```
MOVE_INSIDE ✅
(Nothing else)
```

### Moderator Role

Management capabilities:

```
(All member permissions)
MANAGE_INVITES ✅
MANAGE_BANS ✅
```

### Farmer Role

Farm work only:

```
MOVE_INSIDE ✅
TELEPORT ✅
ANIMAL_INTERACT ✅
SHEAR_ENTITY ✅
CONTAINER_OPEN ✅
ITEM_PICKUP ✅
POTION_USE ✅
```

---

## Next Steps

- [👥 Members & Roles](../menus/members-roles.md) - Managing roles in-game
- [📖 roles.yml](../config/roles-yml.md) - Configure default roles
- [🔐 Ability Permissions](../integrations/ability-permissions.md) - Bukkit permissions

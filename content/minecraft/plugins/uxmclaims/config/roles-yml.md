---
title: roles.yml
order: 250
description: The three built-in roles, custom role defaults, and permission lists.
icon: file-cog
---

## File Structure

```yaml
defaults:
  owner:
    # Owner role config
  member:
    # Member role config
  default:
    # Visitor role config

custom:
  # Your custom roles here
```

---

## Default Roles

These three roles exist on **every claim** and **cannot be deleted**:

### Owner Role

```yaml
defaults:
  owner:
    name: "Owner"
    priority: 0
    permissions: [ ]
```

**The Owner has ALL permissions automatically** - you don't need to list them.

| Setting       | Value   | Meaning                                    |
|---------------|---------|--------------------------------------------|
| `name`        | "Owner" | Display name (can be customized)           |
| `priority`    | 0       | Highest rank (lowest number = higher rank) |
| `permissions` | Empty   | Auto-grants everything                     |

---

### Member Role

```yaml
defaults:
  member:
    name: "Member"
    priority: 1
    permissions:
      # Build permissions
      - BLOCK_PLACE
      - BLOCK_BREAK
      - BUCKET_FILL
      - BUCKET_EMPTY
      - HANGING_PLACE
      - HANGING_BREAK
      - SIGN_EDIT
      # Interaction permissions
      - USE_REDSTONE
      - USE_MECHANISMS
      - CONTAINER_OPEN
      - ARMOR_STAND_MANIPULATE
      - SLEEP
      # Entity permissions
      - ANIMAL_DAMAGE
      - MONSTER_DAMAGE
      - ANIMAL_INTERACT
      - SHEAR_ENTITY
      - VILLAGER_TRADE
      - VEHICLE_INTERACT
      - RIDE_ENTITY
      - POTION_USE
      # Movement permissions
      - MOVE_INSIDE
      - TELEPORT
      - ENDERPEARL_USE
      - CHORUS_TELEPORT
      - ELYTRA_USE
      - ITEM_DROP
      - ITEM_PICKUP
      - FISHING
```

This is the role that new members get when they accept an invitation.

**Default member can:**

- ✅ Build (place, break, use buckets)
- ✅ Interact (open chests, use redstone, doors)
- ✅ Work with entities (animals, villagers)
- ✅ Move freely

**Default member cannot:**

- ❌ Manage claim settings
- ❌ Invite or ban players
- ❌ Use the vault
- ❌ Create warps

---

### Default (Visitor) Role

```yaml
defaults:
  default:
    name: "Default"
    priority: 2
    permissions:
      - MOVE_INSIDE
```

This is what **non-members (strangers)** can do in a claim.

With only `MOVE_INSIDE`, visitors can:

- ✅ Walk through the claim

Visitors cannot:

- ❌ Build anything
- ❌ Open containers
- ❌ Interact with anything

---

## Understanding Priority

Priority determines rank order:

| Priority | Meaning              |
|----------|----------------------|
| 0        | Highest rank (Owner) |
| 1        | High rank (Member)   |
| 2+       | Lower ranks          |

**Higher rank = Lower priority number**

This matters for:

- Who can manage whom (higher ranks can manage lower)
- Vault access priority
- Role assignment limits

---

## Custom Roles

You can add custom roles that are automatically added to **all new claims**:

```yaml
custom:
  moderator:
    name: "Moderator"
    priority: 1
    permissions:
      - BLOCK_PLACE
      - BLOCK_BREAK
      - CONTAINER_OPEN
      - USE_MECHANISMS
      - TELEPORT
      - MANAGE_INVITES
```

### How to Add a Custom Role

1. Under `custom:`, add a new key (lowercase, no spaces)
2. Set the `name` (display name)
3. Set the `priority` (between member and default)
4. List the permissions

**Example: Builder Role**

```yaml
custom:
  builder:
    name: "Builder"
    priority: 1
    permissions:
      - BLOCK_PLACE
      - BLOCK_BREAK
      - BUCKET_FILL
      - BUCKET_EMPTY
      - MOVE_INSIDE
      - TELEPORT
```

**Example: VIP Role**

```yaml
custom:
  vip:
    name: "VIP"
    priority: 1
    permissions:
      - BLOCK_PLACE
      - BLOCK_BREAK
      - CONTAINER_OPEN
      - USE_REDSTONE
      - USE_MECHANISMS
      - TELEPORT
      - MANAGE_INVITES
      - MOVE_INSIDE
```

---

## All Available Permissions

### 🔧 Management Permissions

| Permission        | What It Allows           |
|-------------------|--------------------------|
| `MANAGE_CHUNKS`   | Buy and remove chunks    |
| `MANAGE_TIME`     | Extend claim time        |
| `MANAGE_BANS`     | Ban and unban players    |
| `MANAGE_INVITES`  | Send and revoke invites  |
| `MANAGE_VAULT`    | Full vault access        |
| `MANAGE_BLOCK`    | Place/break claim block  |
| `MANAGE_RENAME`   | Rename the claim         |
| `MANAGE_RELOCATE` | Move spawn point         |
| `MANAGE_WARPS`    | Create/edit/delete warps |

### 🧱 Build Permissions

| Permission        | What It Allows               |
|-------------------|------------------------------|
| `BLOCK_PLACE`     | Place blocks                 |
| `BLOCK_BREAK`     | Break blocks                 |
| `SIGN_EDIT`       | Edit signs                   |
| `BUCKET_FILL`     | Fill buckets                 |
| `BUCKET_EMPTY`    | Empty buckets                |
| `HANGING_PLACE`   | Place item frames, paintings |
| `HANGING_BREAK`   | Break item frames, paintings |
| `TRAMPLE_CROPS`   | Destroy farmland             |
| `SPAWNER_PLACE`   | Place mob spawners           |
| `SPAWNER_DESTROY` | Break mob spawners           |

### 🔌 Interaction Permissions

| Permission               | What It Allows           |
|--------------------------|--------------------------|
| `USE_REDSTONE`           | Buttons, levers, plates  |
| `USE_MECHANISMS`         | Doors, gates, trapdoors  |
| `CONTAINER_OPEN`         | Chests, barrels, hoppers |
| `IGNITE`                 | Use flint and steel      |
| `TAKE_LECTERN_BOOK`      | Take books from lecterns |
| `ARMOR_STAND_MANIPULATE` | Change armor stands      |
| `SLEEP`                  | Use beds                 |

### 🐾 Entity Permissions

| Permission         | What It Allows          |
|--------------------|--------------------------|
| `ANIMAL_DAMAGE`    | Attack passive mobs     |
| `MONSTER_DAMAGE`   | Attack hostile mobs     |
| `ANIMAL_INTERACT`  | Interact with animals   |
| `SHEAR_ENTITY`     | Use shears              |
| `VILLAGER_TRADE`   | Trade with villagers    |
| `VEHICLE_INTERACT` | Use vehicles            |
| `RIDE_ENTITY`      | Ride entities           |
| `POTION_USE`       | Use potions on entities |

### 🚶 Movement & Misc Permissions

| Permission        | What It Allows      |
|-------------------|---------------------|
| `MOVE_INSIDE`     | Enter the claim     |
| `TELEPORT`        | Use claim teleport  |
| `USE_WARPS`       | Use claim warps     |
| `ENDERPEARL_USE`  | Throw ender pearls  |
| `ELYTRA_USE`      | Fly with elytra     |
| `CHORUS_TELEPORT` | Chorus fruit teleport|
| `ITEM_DROP`       | Drop items          |
| `ITEM_PICKUP`     | Pick up items       |
| `FISHING`         | Fish                |
| `RAID_TRIGGER`    | Trigger raids       |
| `CLAIM_CHAT`      | Use claim chat      |
| `FLY`             | Fly inside claim    |

---

## Common Configurations

### Restrictive Member Role

Only allow basic movement and container access:

```yaml
member:
  name: "Member"
  priority: 1
  permissions:
    - MOVE_INSIDE
    - TELEPORT
    - CONTAINER_OPEN
```

### Trust-Heavy Member Role

Give members almost full access:

```yaml
member:
  name: "Member"
  priority: 1
  permissions:
    - BLOCK_PLACE
    - BLOCK_BREAK
    - BUCKET_FILL
    - BUCKET_EMPTY
    - HANGING_PLACE
    - HANGING_BREAK
    - SIGN_EDIT
    - USE_REDSTONE
    - USE_MECHANISMS
    - CONTAINER_OPEN
    - ARMOR_STAND_MANIPULATE
    - SLEEP
    - ANIMAL_DAMAGE
    - MONSTER_DAMAGE
    - ANIMAL_INTERACT
    - SHEAR_ENTITY
    - VILLAGER_TRADE
    - VEHICLE_INTERACT
    - RIDE_ENTITY
    - MOVE_INSIDE
    - TELEPORT
    - USE_WARPS
    - ENDERPEARL_USE
    - ELYTRA_USE
    - ITEM_DROP
    - ITEM_PICKUP
    - FISHING
    - MANAGE_INVITES
```

### Locked-Down Visitor Role

Prevent strangers from even entering:

```yaml
default:
  name: "Visitor"
  priority: 2
  permissions: [ ]
```

With an empty list, visitors cannot even enter claims!

---

## Tips

1. **Keep member reasonable** - Too many permissions can cause problems
2. **Use custom roles** - Create specialized roles like Builder, Helper, Moderator
3. **Remember priority** - Lower number = higher rank
4. **Test changes** - After editing, create a new claim to test

---

## Next Steps

- [💰 entitlements.yml](../config/entitlements-yml.md) - Set limits and costs for players
- [🛡️ All Permissions](../protection/permissions.md) - Complete permission reference

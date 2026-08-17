---
title: Domain Models
order: 460
description: Claim, Member, Role and the rest of the domain types you will read.
icon: boxes
---

## Core Models

### Claim

The main aggregate root. Contains all data about a claim.

```java
public class Claim {
    UUID id;                    // Unique identifier
    String name;                // Display name
    Instant expireDate;         // When the claim expires
    Instant creationDate;       // When it was created
    Location spawnLocation;     // Teleport destination
    ClaimBlock block;           // Block style and location
    ClaimVault vault;           // Shared storage
    
    Map<UUID, ClaimBan> bans;       // Banned players
    Map<UUID, ClaimRole> roles;     // Roles (Owner, Member, custom)
    Map<UUID, ClaimWarp> warps;     // Teleport points
    Map<UUID, ClaimChunk> chunks;   // Claimed chunks
    Map<UUID, ClaimMember> members; // Claim members
    Map<UUID, ClaimInvite> invites; // Pending invitations
    Set<ClaimFlag> flags;           // Active flags
}
```

**Key Methods:**

| Method                   | Returns                 | Description               |
|--------------------------|-------------------------|---------------------------|
| `getId()`                | `UUID`                  | Claim's unique ID         |
| `getName()`              | `String`                | Display name              |
| `getOwnerUid()`          | `UUID`                  | Owner's player UUID       |
| `getOwner()`             | `ClaimMember`           | Owner member object       |
| `isOwner(UUID)`          | `boolean`               | Check if UUID is owner    |
| `isExpired()`            | `boolean`               | Check if expired          |
| `getRemainTime()`        | `Duration`              | Time until expiration     |
| `hasFlag(ClaimFlag)`     | `boolean`               | Check if flag is enabled  |
| `hasBanByUid(UUID)`      | `boolean`               | Check if player is banned |
| `findMemberByUid(UUID)`  | `Optional<ClaimMember>` | Find member               |
| `findRoleByName(String)` | `Optional<ClaimRole>`   | Find role by name         |

---

### ClaimMember

A player who is part of a claim.

```java
public class ClaimMember {
    UUID id;                // Member entry ID
    UUID uid;               // Player's UUID
    ClaimRole role;         // Assigned role
    Instant joinDate;       // When they joined
    Set<ClaimPermission> grantedPermissions;   // Extra permissions
    Set<ClaimPermission> revokedPermissions;   // Removed permissions
}
```

**Key Methods:**

| Method                           | Returns                | Description                |
|----------------------------------|------------------------|----------------------------|
| `getUid()`                       | `UUID`                 | Player's UUID              |
| `getRole()`                      | `ClaimRole`            | Current role               |
| `hasPermission(ClaimPermission)` | `boolean`              | Check effective permission |
| `getEffectivePermissions()`      | `Set<ClaimPermission>` | All active permissions     |

---

### ClaimRole

A role with a set of permissions.

```java
public class ClaimRole {
    UUID id;                        // Role ID
    String name;                    // Display name
    ClaimRoleType type;             // OWNER, MEMBER, DEFAULT, CUSTOM
    int priority;                   // Lower = higher rank
    Set<ClaimPermission> permissions; // Granted permissions
}
```

**Role Types:**

| Type      | Description                                  |
|-----------|----------------------------------------------|
| `OWNER`   | The claim owner (cannot delete)              |
| `MEMBER`  | Default role for new members (cannot delete) |
| `DEFAULT` | Permissions for non-members (cannot delete)  |
| `CUSTOM`  | User-created roles                           |

---

### ClaimChunk

A 16x16 block area that's part of a claim.

```java
public class ClaimChunk {
    UUID id;            // Chunk entry ID
    Chunk chunk;        // Chunk coordinates
    boolean isMain;     // Is this the main chunk?
    Instant createdAt;  // When it was claimed
}
```

**The Chunk value object:**

```java
public record Chunk(String world, int x, int z) {
    public static Chunk of(String world, int x, int z);
    public static Chunk fromLocation(Location loc);
}
```

---

### ClaimWarp

A teleport point within a claim.

```java
public class ClaimWarp {
    UUID id;            // Warp ID
    String name;        // Display name
    Location location;  // Teleport destination
    boolean isPublic;   // Visible to everyone?
    Instant createdAt;  // Creation time
}
```

---

### ClaimInvite

A pending invitation to join a claim.

```java
public class ClaimInvite {
    UUID id;            // Invite ID
    UUID uid;           // Invited player's UUID
    Instant createdAt;  // When sent
}
```

---

### ClaimBan

A banned player entry.

```java
public class ClaimBan {
    UUID id;            // Ban entry ID
    UUID uid;           // Banned player's UUID
    Instant createdAt;  // When banned
}
```

---

### ClaimVault

Shared storage for a claim.

```java
public class ClaimVault {
    UUID id;            // Vault ID
    byte[] items;       // Serialized inventory contents
}
```

---

### ClaimBlock

The physical claim block in the world.

```java
public class ClaimBlock {
    UUID id;            // Block entry ID
    Location location;  // Where it's placed (null if not placed)
    String key;         // Block style key (e.g., "bedrock")
}
```

---

## Value Objects

### Location

A position in the world.

```java
public record Location(
    String world,
    double x,
    double y,
    double z
) {
    public static Location of(String world, double x, double y, double z);
}
```

### Chunk

A chunk coordinate.

```java
public record Chunk(
    String world,
    int x,
    int z
) {
    public static Chunk of(String world, int x, int z);
    public static Chunk fromLocation(Location loc);
}
```

---

## Enums

### ClaimFlag

Global rules for a claim (affects everyone):

```java
public enum ClaimFlag {
    // Security
    PVP, TNT_EXPLOSIONS, CREEPER_DAMAGE, WITHER_DAMAGE,
    FIRE_SPREAD, LIGHTNING_DAMAGE, MOB_GRIEFING,
    
    // World mechanics
    FLUID_FLOW, PISTON_PUSH, LEAF_DECAY, ICE_MELT,
    BLOCK_FADE, BLOCK_FORM, ENTITY_BLOCK_FORM,
    STRUCTURE_GROW, NATURE_SPREAD,
    
    // Mob spawning
    SPAWN_ANIMALS, SPAWN_MONSTERS, SPAWN_PHANTOMS, MOB_SPAWNING,
    
    // Other
    ITEM_PICKUP_GLOBAL, CHORUS_TELEPORT
}
```

### ClaimPermission

Role-based permissions:

```java
public enum ClaimPermission {
    // Management
    MANAGE_CHUNKS, MANAGE_TIME, MANAGE_BANS, MANAGE_INVITES,
    MANAGE_VAULT, MANAGE_BLOCK, MANAGE_RENAME, MANAGE_RELOCATE,
    MANAGE_WARPS,
    
    // Build
    BLOCK_PLACE, BLOCK_BREAK, SIGN_EDIT, BUCKET_FILL, BUCKET_EMPTY,
    HANGING_PLACE, HANGING_BREAK, TRAMPLE_CROPS,
    SPAWNER_PLACE, SPAWNER_DESTROY,
    
    // Interact
    IGNITE, USE_REDSTONE, USE_MECHANISMS, CONTAINER_OPEN,
    TAKE_LECTERN_BOOK, ARMOR_STAND_MANIPULATE, SLEEP,
    
    // Entity
    ANIMAL_DAMAGE, MONSTER_DAMAGE, ANIMAL_INTERACT,
    SHEAR_ENTITY, VILLAGER_TRADE, VEHICLE_INTERACT, RIDE_ENTITY,
    
    // Movement
    MOVE_INSIDE, TELEPORT, USE_WARPS, ENDERPEARL_USE, ELYTRA_USE,
    ITEM_DROP, ITEM_PICKUP, FISHING, RAID_TRIGGER
}
```

### ClaimRoleType

```java
public enum ClaimRoleType {
    OWNER,      // The owner role
    MEMBER,     // Default member role
    DEFAULT,    // Non-member role
    CUSTOM      // User-created
}
```

---

## Working Example

```java
// Get a claim and explore its data
Optional<Claim> optClaim = claimQuery.findByLocation(location);
if (optClaim.isEmpty()) return;

Claim claim = optClaim.get();

// Basic info
getLogger().info("Claim: " + claim.getName());
getLogger().info("Owner: " + claim.getOwnerUid());
getLogger().info("Chunks: " + claim.getChunks().size());
getLogger().info("Expires: " + claim.getExpireDate());

// Check flags
if (claim.hasFlag(ClaimFlag.PVP)) {
    getLogger().info("PvP is enabled in this claim");
}

// List members
for (ClaimMember member : claim.getMembers().values()) {
    getLogger().info("Member: " + member.getUid() + 
                     " Role: " + member.getRole().getName());
}

// Check if player can build
Optional<ClaimMember> member = claim.findMemberByUid(playerUid);
if (member.isPresent() && member.get().hasPermission(ClaimPermission.BLOCK_PLACE)) {
    getLogger().info("Player can build here");
}
```

---

## Next Steps

- [📖 Query API](../developer/queries.md) - Finding claims
- [⚡ Events](../developer/events.md) - Listening to changes

---
title: Query API
order: 430
---

## Getting the Facade

```java
import com.uxplima.claim.bukkit.api.UxmClaimBukkitAPI;
import com.uxplima.claim.app.facade.ClaimFacade;

ClaimFacade claimFacade = UxmClaimBukkitAPI.getInstance().claimFacade();
```

---

## Finding Claims

### By UUID

```java
import java.util.Optional;
import java.util.UUID;

UUID claimId = UUID.fromString("...");

// Safe - returns Optional
Optional<Claim> claim = claimFacade.findById(claimId);

claim.ifPresent(c -> {
    getLogger().info("Found claim: " + c.getName());
});

// Direct - throws if not found
Claim claim = claimFacade.getById(claimId);
```

### By Location

```java
import com.uxplima.claim.domain.model.vo.Location;
import com.uxplima.claim.bukkit.api.BukkitConverter;

// Convert Bukkit location to domain Location
Location loc = BukkitConverter.toDomainLocation(player.getLocation());

// Find claim at this location
Optional<Claim> claim = claimFacade.findByLocation(loc);

if (claim.isPresent()) {
    getLogger().info("This location is claimed by: " + claim.get().getName());
} else {
    getLogger().info("This location is wilderness");
}
```

### By Chunk

```java
import com.uxplima.claim.domain.model.vo.Chunk;

// Convert Bukkit chunk to domain Chunk
Chunk chunk = BukkitConverter.toDomainChunk(player.getLocation().getChunk());

// Find claim covering this chunk
Optional<Claim> claim = claimFacade.findByChunk(chunk);
```

---

## Existence Checks

Fast checks without loading full objects:

```java
// Check if a location is claimed
boolean isClaimed = claimFacade.existsByLocation(location);

// Check if a chunk is claimed  
boolean isChunkClaimed = claimFacade.existsByChunk(chunk);

// Check if a claim ID exists
boolean exists = claimFacade.existsById(claimId);
```

---

## Finding Claims by Player

### Claims Owned by Player

```java
UUID playerUid = player.getUniqueId();

// Get all claims where player is the owner
List<Claim> ownedClaims = claimFacade.findByOwnerUid(playerUid);

getLogger().info("Player owns " + ownedClaims.size() + " claims");
```

### Claims Where Player is Member

```java
// Get claims where player is a member (not owner)
List<Claim> memberClaims = claimFacade.findByMemberUid(playerUid);
```

### Pending Invitations

```java
// Get claims where player has a pending invitation
List<Claim> invitedClaims = claimFacade.findByInvitedUid(playerUid);
```

---

## Counting Claims

```java
UUID playerUid = player.getUniqueId();

// Count owned claims
int ownedCount = claimFacade.countByOwnerUid(playerUid);

// Count member claims (not owner)
int memberCount = claimFacade.countByMemberUid(playerUid);
```

---

## Cache-Only Methods (Unsafe)

For performance-critical code, use "unsafe" methods that only check the cache:

```java
// Only checks cache - may return null even if claim exists in database
Claim claim = claimFacade.getByLocationUnsafe(location);

if (claim != null) {
    // Claim was in cache
}
```

⚠️ **Warning:** These may return `null` even if the claim exists but isn't cached.

---

## Getting All Claims

```java
// All cached claims
List<Claim> allClaims = claimFacade.findAllClaims();

// All expired claims
List<Claim> expiredClaims = claimFacade.findExpiredClaims();
```

---

## Public Warps

```java
import com.uxplima.claim.domain.model.ClaimWarp;

// Get all public warps from all claims
List<ClaimWarp> publicWarps = claimFacade.findPublicWarps();

for (ClaimWarp warp : publicWarps) {
    getLogger().info("Warp: " + warp.getName());
}
```

---

## BukkitConverter

Use `BukkitConverter` to convert between Bukkit and domain types:

```java
import com.uxplima.claim.bukkit.api.BukkitConverter;

// Bukkit → Domain
Location domainLoc = BukkitConverter.toDomainLocation(bukkitLocation);
Chunk domainChunk = BukkitConverter.toDomainChunk(bukkitChunk);

// Domain → Bukkit
org.bukkit.Location bukkitLoc = BukkitConverter.toBukkitLocation(domainLocation);
```

---

## Working with Claim Objects

Once you have a `Claim`:

```java
Claim claim = claimFacade.findByLocation(location).orElse(null);
if (claim == null) return;

// Basic info
String name = claim.getName();
UUID id = claim.getId();
UUID ownerUid = claim.getOwnerUid();

// Check ownership
boolean isOwner = claim.isOwner(playerUid);

// Chunks
int chunkCount = claim.getChunks().size();

// Members
Optional<ClaimMember> member = claim.findMemberByUid(playerUid);

// Check flags
boolean hasPvp = claim.hasFlag(ClaimFlag.PVP);

// Check bans
boolean isBanned = claim.hasBanByUid(playerUid);

// Check permissions
if (member.isPresent()) {
    boolean canBuild = member.get().hasPermission(ClaimPermission.BLOCK_PLACE);
}
```

---

## PlayerFacade

For player-specific data:

```java
import com.uxplima.claim.app.facade.PlayerFacade;

PlayerFacade playerFacade = UxmClaimBukkitAPI.getInstance().playerFacade();

// Find player by UUID
Optional<CPlayer> player = playerFacade.findByUid(playerUid);

// Find player by name
Optional<CPlayer> player = playerFacade.findByName("Steve");

// Check if player is registered
boolean exists = playerFacade.existsByUid(playerUid);
```

---

## Next Steps

- [⚡ Events](../developer/events.md) - Reacting to claim changes
- [🔧 Use Cases](../developer/usecases.md) - Modifying claims

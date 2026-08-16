---
title: How Protection Works
order: 300
---

## The Three Layers

Protection in uxmClaims works through three layers:

```
┌─────────────────────────────────────┐
│         1. CLAIM EXISTS          │
│   Is this chunk protected at all?│
├─────────────────────────────────────┤
│         2. FLAGS (Rules)         │
│   What actions are possible here?│
├─────────────────────────────────────┤
│      3. PERMISSIONS (Who)        │
│   Who can perform this action?   │
└─────────────────────────────────────┘
```

---

## Layer 1: Claim Existence

First question: **Is this chunk claimed?**

| Location               | Protection          |
|------------------------|---------------------|
| Unclaimed (wilderness) | ❌ No protection     |
| Claimed chunk          | ✅ Protection active |

If a chunk is not claimed, anyone can do anything there.

---

## Layer 2: Flags (Global Rules)

Second question: **Is this action allowed in this claim?**

Flags control what's **possible** inside a claim. They affect **everyone**, including the owner.

**Example: TNT_EXPLOSIONS flag**

| Flag Status | Effect                                                         |
|-------------|----------------------------------------------------------------|
| OFF         | TNT cannot explode at all                                      |
| ON          | TNT can explode (but who can ignite it depends on permissions) |

Think of flags as **physics rules** for your claim.

See [All Flags](../protection/flags.md) for the complete list.

---

## Layer 3: Permissions (Role-Based Access)

Third question: **Does this player have permission?**

Permissions determine who can do what. They're assigned to **roles**.

**Example: BLOCK_BREAK permission**

| Role    | Has Permission? | Result              |
|---------|-----------------|---------------------|
| Owner   | Always ✅        | Can break blocks    |
| Member  | ✅ By default    | Can break blocks    |
| Visitor | ❌ No            | Cannot break blocks |

See [All Permissions](../protection/permissions.md) for the complete list.

---

## How They Work Together

Let's walk through an example:

**Scenario:** A player tries to place a block

```
1. Is this chunk claimed?
   ├── No  → ✅ Anyone can place blocks
   └── Yes → Continue to step 2

2. Is there a flag blocking this?
   └── (No flag blocks block placement) → Continue to step 3

3. Does the player have BLOCK_PLACE permission?
   ├── Owner → ✅ Always has permission
   ├── Member with permission → ✅ Can place
   ├── Member without permission → ❌ Blocked
   └── Visitor → ❌ Blocked (usually)
```

---

## Another Example: TNT

**Scenario:** Someone tries to use TNT

```
1. Is this chunk claimed?
   └── Yes → Continue

2. Is TNT_EXPLOSIONS flag ON?
   ├── OFF → ❌ TNT doesn't explode. Period.
   └── ON  → Continue to step 3

3. Does the player have IGNITE permission (for lighting TNT)?
   ├── Yes → ✅ TNT works normally
   └── No  → ❌ Player can't light the TNT
```

Notice: Even if you have IGNITE permission, if TNT_EXPLOSIONS flag is OFF, nothing happens.

---

## Who Falls Under Which Role?

| Person          | Default Role            |
|-----------------|-------------------------|
| Claim owner     | Owner (full access)     |
| Invited members | Member (or custom role) |
| Everyone else   | Default/Visitor         |

---

## Common Protection Scenarios

### "I want a peaceful, safe base"

**Flags to disable:**

- PVP (no player fighting)
- TNT_EXPLOSIONS (no TNT damage)
- CREEPER_DAMAGE (no creeper damage)
- FIRE_SPREAD (no fire spreading)
- SPAWN_MONSTERS (no hostile mobs)

**Result:** Safe, mob-free zone where nobody can fight or cause destruction.

---

### "I want a PvP arena"

**Flags to enable:**

- PVP (allow fighting)

**Permissions:**

- Give visitors MOVE_INSIDE to enter
- Remove BLOCK_PLACE/BLOCK_BREAK to prevent griefing

**Result:** Players can fight but can't modify the arena.

---

### "I want a public farm"

**Flags:**

- Keep defaults for world mechanics

**Permissions for visitors:**

- Add MOVE_INSIDE
- Add CONTAINER_OPEN (access chests)
- Add ANIMAL_INTERACT (milk cows, shear sheep)
- DON'T add BLOCK_BREAK (protect the farm)

**Result:** People can access and use the farm but can't destroy it.

---

### "I want full lockdown"

**Default/Visitor role:**

- Remove ALL permissions including MOVE_INSIDE

**Result:** Nobody except members can even enter the claim.

---

## What's Protected by Default?

When you create a new claim with default settings:

**Protected from strangers:**

- ✅ Block placing/breaking
- ✅ Chest access
- ✅ Interactions (buttons, doors)
- ✅ Entity damage
- ✅ Most actions

**NOT protected by default:**

- ❌ Entry (visitors can walk through)

---

## Quick Reference

| Want to control...     | Use...                                 |
|------------------------|----------------------------------------|
| Explosions, fire, mobs | Flags                                  |
| Who can build          | Permissions (BLOCK_PLACE, BLOCK_BREAK) |
| Who can access chests  | Permissions (CONTAINER_OPEN)           |
| Who can enter          | Permissions (MOVE_INSIDE)              |
| PvP                    | Flags (PVP)                            |

---

## Next Steps

- [🚩 All Flags](../protection/flags.md) - Complete flag reference
- [🔐 All Permissions](../protection/permissions.md) - Complete permission reference

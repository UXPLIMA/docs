---
title: Admin Commands
order: 140
icon: shield-user
---

## Overview

Admin commands require the `uxmclaims.admin` permission. These commands allow server administrators to manage claims globally.

---

## Available Commands

| Command | Description |
|---------|-------------|
| `/claim admin view <player>` | View and manage a player's claims |
| `/claim admin bulk setflag <flag> <value>` | Set a flag on all claims |
| `/claim admin bulk setrolepermission <role> <perm> <value>` | Set a permission for a role on all claims |
| `/claim admin bulk rename <basename>` | Rename all claims with sequential numbering |
| `/claim admin bulk setblock <type>` | Set all claim blocks to a type |
| `/claim admin bulk destroyblock` | Destroy all claim blocks |

---

## `/claim admin view <player>`

Opens a menu showing all claims where the specified player is owner or member.

**Usage:**
```
/claim admin view Steve
```

**Features:**

- View all claims for a player
- Access claim management menus
- Manage claims on behalf of players

---

## Bulk Commands

Bulk commands apply changes to **all claims** on the server. Use with caution!

<Callout type="warning" title="Performance">

Bulk operations run asynchronously but may take time on large servers.

</Callout>

### `/claim admin bulk setflag <flag> <value>`

Sets a flag on every claim.

**Example:**
```
/claim admin bulk setflag PVP false
```

This disables PvP on all claims at once.

---

### `/claim admin bulk setrolepermission <role> <permission> <value>`

Updates a permission for a specific role across all claims.

**Roles:** `Owner`, `Moderator`, `Member`, `Default`

**Example:**
```
/claim admin bulk setrolepermission Member BLOCK_BREAK true
```

This grants `BLOCK_BREAK` permission to the Member role on all claims.

---

### `/claim admin bulk rename <basename>`

Renames all claims using a base name with sequential numbers.

**Example:**
```
/claim admin bulk rename Base
```

Result: `Base-1`, `Base-2`, `Base-3`, etc.

---

### `/claim admin bulk setblock <type>`

Places a claim block of the specified type on all claims.

**Example:**
```
/claim admin bulk setblock DIAMOND_BLOCK
```

---

### `/claim admin bulk destroyblock`

Removes the claim block from all claims.

**Example:**
```
/claim admin bulk destroyblock
```

---

## Next Steps

- [⚙️ config.yml](../config/config-yml.md) - Server configuration
- [🔐 Ability Permissions](../integrations/ability-permissions.md) - Permission nodes

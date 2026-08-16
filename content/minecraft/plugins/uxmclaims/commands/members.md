---
title: Member Commands
order: 80
icon: users
---

## Available Commands

These commands are strictly for advanced member management. For quick actions, see the [Basic Commands](basic.md) shortcuts.

| Command | Description |
|---------|-------------|
| `/claim member leave` | Leaves the current claim |
| `/claim member kick <player>` | Kicks a member |
| `/claim member promote <player>` | Promotes to next role |
| `/claim member demote <player>` | Demotes to previous role |
| `/claim member setrole <player> <role>` | Sets exact role |
| `/claim member setperm <player> <perm> <val>` | Sets personal permission |
| `/claim member transfer <player>` | Transfers claim ownership to a member |

---

## Member Management

### `/claim member kick <player>`

Alternative to `/claim kick`. Removes a member.

## Role & Permission Management

### `/claim member promote / demote <player>`

Moves a member up or down one role level based on priority.

### `/claim member setrole <player> <role>`

Directly assigns a specific role to a member.

**Example:**
```
/claim member setrole Steve Builder
```

### `/claim member setperm <player> <permission> <val>`

Grants or revokes a specific permission for one player only.

**Example:**
```
/claim member setperm Steve BLOCK_BREAK true
```

---

## Next Steps

- [Basic Commands](basic.md) - Shortcuts for trust, kick, ban
- [🎭 Role Commands](roles.md) - Create custom roles
- [🔐 All Permissions](../protection/permissions.md) - Permission list

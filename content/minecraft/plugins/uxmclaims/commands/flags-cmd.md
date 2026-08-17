---
title: Flag Commands
order: 130
description: Setting, toggling and listing claim flags from chat.
icon: flag
---

## Available Commands

| Command | Description |
|---------|-------------|
| `/claim setflag <flag> <true/false>` | Sets a flag to a specific value |
| `/claim toggleflag <flag>` | Toggles a flag on or off |

---

## Command Details

### `/claim setflag <flag> <true/false>`

Sets a claim flag to a specific value.

**Requirements:**

- Must be standing inside your claim OR have a claim selected
- Must have permission to manage the claim

**Example:**
```
/claim setflag PVP true
/claim setflag FIRE_SPREAD false
```

---

### `/claim toggleflag <flag>`

Toggles a flag between enabled and disabled.

**Example:**
```
/claim toggleflag TNT_EXPLOSIONS
```

---

## Common Flags

| Flag | Description |
|------|-------------|
| `PVP` | Player vs player combat |
| `FIRE_SPREAD` | Fire spreading to blocks |
| `TNT_EXPLOSIONS` | TNT destroying blocks |
| `CREEPER_DAMAGE` | Creeper explosion damage |
| `MOB_GRIEFING` | Mobs modifying blocks |
| `SPAWN_MONSTERS` | Hostile mob spawning |

For the complete list, see [All Claim Flags](../protection/flags.md).

---

## Examples

### Creating a Safe Base

```
/claim setflag PVP false
/claim setflag FIRE_SPREAD false
/claim setflag CREEPER_DAMAGE false
/claim setflag MOB_GRIEFING false
```

### Enabling a PvP Arena

```
/claim setflag PVP true
```

---

## Next Steps

- [🚩 All Claim Flags](../protection/flags.md) - Complete flag reference
- [Basic Commands](basic.md) - Other claim commands

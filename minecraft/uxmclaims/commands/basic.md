---
title: Basic Commands
order: 70
---

## Available Commands

| Command | Description |
|---------|-------------|
| `/claim` | Smart context: Opens menu OR prompts create |
| `/claim list` | Opens your claims list menu |
| `/claim warps` | Opens the public warps list |
| `/claim invites` | Opens your invite inbox |
| `/claim showborders` | Toggles border visualization on/off |

---

## Claim Management (Shortcuts & Basic)

| Command | Description |
|---------|-------------|
| `/claim create [name]` | Creates a new claim |
| `/claim delete` | Deletes the current claim |
| `/claim rename <new-name>` | Renames the current claim |
| `/claim extend <time>` | Extends claim duration (e.g. `1d`) |
| `/claim spawn [publicWarp]` | Teleports to claim spawn OR a specific public warp |
| `/claim setspawn` | Sets claim's spawn point |
| `/claim vault` | Opens claim vault |
| `/claim leave` | Leaves the current claim (for members) |
| `/claim transfer <player>` | Transfers ownership to a member |
| `/claim trust <player>` | Adds a member (Invite) |
| `/claim kick <player>` | Kicks a member |
| `/claim ban <player>` | Bans a player |
| `/claim unban <player>` | Unbans a player |
| `/claim setflag <flag> <value>` | Sets a claim flag |
| `/claim toggleflag <flag>` | Toggles a claim flag |
| `/claim chat [message]` | Sends a message to claim members / toggles claim chat mode |
| `/claim confirm` | Confirms action |
| `/claim cancel` | Cancels action |
| `/claim reload` | Reloads plugin (Admin) |

<Callout type="warning" title="Confirmation Required">

Dangerous actions like `delete`, `kick`, `ban` require confirmation using `/claim confirm`.

</Callout>

---

## Managing Flags

Control the rules of your claim (like PvP, explosions) using flags.

### Flags vs Permissions

| Concept | Applies To | Example |
|---------|------------|---------|
| **Flags** | Everyone (including owner) | "Fire can't spread in this claim" |
| **Permissions** | Specific roles | "Members can place blocks" |

### Commands

**`/claim setflag <flag> <true/false>`**
Sets a flag to a specific value.

```
/claim setflag PVP true
```

**`/claim toggleflag <flag>`**
Toggles a flag on or off.

```
/claim toggleflag FIRE_SPREAD
```

### Common Flags

| Flag | OFF | ON |
|------|-----|-----|
| `PVP` | Players can't hurt each other | PvP enabled |
| `FIRE_SPREAD` | Fire doesn't spread | Fire spreads normally |
| `TNT_EXPLOSIONS` | TNT doesn't destroy blocks | TNT works normally |
| `SPAWN_MONSTERS` | No hostile mobs spawn | Monsters spawn normally |

For the complete list, see [All Claim Flags](../protection/flags.md).

---

## Claim Block (Nexus)

Manage the physical block representing your claim's center.

| Command | Description |
|---------|-------------|
| `/claim block place <type>` | Places a new claim block |
| `/claim block change <type>` | Changes the block type |
| `/claim block destroy` | Removes the claim block |

---

## Examples

### Creating & Managing

```
/claim create MyBase
/claim extend 7d
```

### Adding & Removing Members (Shortcuts)

```
/claim trust Steve
/claim kick Steve
```

### Making a Safe Base

```
/claim setflag PVP false
/claim setflag FIRE_SPREAD false
```

### Teleporting

```
/claim spawn
/claim setspawn
```

---

## Next Steps

- [👥 Member Management](members.md) - Advanced role & permission management
- [📍 Warp Commands](warps.md) - Create teleport points
- [🗺️ Chunk Commands](chunks.md) - Expand your claim
- [🚩 All Claim Flags](../protection/flags.md) - Complete flag reference

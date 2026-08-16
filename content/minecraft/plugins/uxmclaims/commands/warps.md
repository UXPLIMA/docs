---
title: Warp Commands
order: 100
icon: map-pin
---

## Available Commands

| Command | Description |
|---------|-------------|
| `/claim warp create <name>` | Creates a warp at your location |
| `/claim warp delete <name>` | Deletes a warp |
| `/claim warp teleport <name>` | Teleports to a warp |
| `/claim warp rename <name> <new-name>` | Renames a warp |
| `/claim warp move <name>` | Updates warp location (Relocate) |
| `/claim warp setpublic <name> <true/false>` | Sets warp visibility |

---

## Public vs Private Warps

| Type | Who Can Use |
|------|-------------|
| **Private** | Only claim members |
| **Public** | Anyone on the server |

Public warps appear in the main menu's "Public Warps" list (`/claim warps`).

---

## Command Details

### `/claim warp create <name>`

Creates a warp at your current position. You'll spawn facing the direction you were looking.

**Requirements:**

- Must be standing inside your claim
- Must have available warp slots
- May cost money (server configurable)

**Naming Rules:**

- At least 3 characters
- Cannot contain spaces (use underscores or hyphens instead)

---

### `/claim warp delete <name>`

Permanently deletes a warp from your claim.

<Callout type="info" title="Confirmation Required">

This command requires confirmation. Click `[CONFIRM]` in chat or use `/claim confirm`.

</Callout>

---

### `/claim warp teleport <name>`

Teleports to the specified warp. May have a delay based on server settings.

---

### `/claim warp move <name>`

Updates the warp's location to where you're currently standing.

---

### `/claim warp setpublic <name> <true/false>`

Makes a warp:

- **Public (`true`):** Visible to all players in `/claim warps`
- **Private (`false`):** Only accessible by claim members

---

## Examples

### Creating a Shop Warp

```
/claim warp create Shop
/claim warp setpublic Shop true
```

### Moving a Warp

```
/claim warp move Shop
```

---

## Tips

1. **Name clearly** - "Shop" is better than "warp1"
2. **Face the right direction** - You'll spawn facing that way
3. **Stand on solid ground** - Don't create warps in mid-air

---

## Next Steps

- [📍 Warps Menu](../menus/warps.md) - GUI warp management
- [Basic Commands](basic.md) - Core claim commands

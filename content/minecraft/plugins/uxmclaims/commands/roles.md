---
title: Role Commands
order: 90
icon: user-cog
---

## Available Commands

| Command | Description |
|---------|-------------|
| `/claim role create <name>` | Creates a new role |
| `/claim role delete <name>` | Deletes a role |
| `/claim role rename <name> <new-name>` | Renames a role |
| `/claim role setpriority <name> <number>` | Sets role priority |
| `/claim role setperm <name> <perm> <true/false>` | Sets a role permission |

---

## Understanding Priority

Role priority determines hierarchy:

- **Lower number = Higher authority**
- Priority `0` is the highest (Owner)
- Higher priority members can manage lower priority members

| Priority | Example Role |
|----------|--------------|
| 0 | Owner |
| 1 | Moderator |
| 2 | Member |
| 3 | Default (Visitor) |

---

## Command Details

### `/claim role create <name>`

Creates a new custom role with default permissions (none).

**Naming Rules:**

- At least 2 characters
- Cannot contain spaces (use underscores or hyphens instead)

**Example:**
```
/claim role create Builder
```

---

### `/claim role delete <name>`

Deletes a custom role. Members with this role will be moved to the default role.

<Callout type="warning">

You cannot delete the default roles: Owner, Member, Default.

</Callout>

<Callout type="info" title="Confirmation Required">

This command requires confirmation. Click `[CONFIRM]` in chat or use `/claim confirm`.

</Callout>

---

### `/claim role setpriority <name> <number>`

Sets the role's priority level.

**Example:**
```
/claim role setpriority Builder 2
```

---

### `/claim role setperm <name> <permission> <true/false>`

Adds or removes a permission from a role.

**Example:**
```
/claim role setperm Builder BLOCK_PLACE true
/claim role setperm Builder BLOCK_BREAK true
```

---

## Examples

### Creating a Builder Role

```
/claim role create Builder
/claim role setpriority Builder 2
/claim role setperm Builder BLOCK_PLACE true
/claim role setperm Builder BLOCK_BREAK true
```

### Creating a Trusted Role

```
/claim role create Trusted
/claim role setperm Trusted CONTAINER_OPEN true
/claim role setperm Trusted DOOR_OPEN true
```

---

## Next Steps

- [👥 Member Commands](members.md) - Assign roles to members
- [🔐 All Permissions](../protection/permissions.md) - Available permissions
- [📖 roles.yml](../config/roles-yml.md) - Default role configuration

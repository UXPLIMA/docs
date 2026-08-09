---
title: Members & Roles
order: 170
---

## Member List Menu

**Menu File:** `menu/claim_member_list.yml`

### How to Open

1. Open claim management
2. Click **Members** (`memberList`)

### What Is This Menu For?

Shows all players who are members of your claim. You can manage their roles and access.

---

### Menu Items

#### `memberHead` - Member Entry

**Icon:** Player Head (shows the player's skin)

**What it shows:**

- Member's name
- Their role
- When they joined

**Actions:**

| Click               | Action                        |
|---------------------|-------------------------------|
| Left-Click          | Open member management menu   |
| Shift + Left-Click  | Promote (move to higher role) |
| Shift + Right-Click | Demote (move to lower role)   |

**Promotion/Demotion rules:**

- Cannot promote to Owner
- Cannot demote below the lowest role
- Cannot change your own role

---

#### `previousPage` & `nextPage` - Pagination

**Icon:** Arrows

Navigate through pages if you have many members.

---

#### `back` - Back

**Icon:** Barrier

Returns to claim management menu.

---

## Role List Menu

**Menu File:** `menu/claim_role_list.yml`

### How to Open

1. Open claim management
2. Click **Roles** (`roleList`)

### What Is This Menu For?

Shows all roles in this claim. You can create, edit, and manage role permissions.

---

### Menu Items

#### `roleItem` - Role Entry

**Icon:** Paper

**What it shows:**

- Role name
- Role type (Default, Custom)
- Priority number
- Number of permissions

**Actions:**

| Click               | Action                          |
|---------------------|---------------------------------|
| Left-Click          | Open role permission editor     |
| Shift + Left-Click  | Increase priority (higher rank) |
| Shift + Right-Click | Decrease priority (lower rank)  |

---

#### `addRole` - Create New Role

**Icon:** Lime Dye (glowing) ➕

**What it does:** Opens a text input to create a new custom role.

**After creation:**

- The role is added with default permissions
- You can edit its permissions immediately
- Priority is set automatically

---

#### `previousPage` & `nextPage` - Pagination

Navigate through pages if you have many roles.

---

#### `back` - Back

Returns to claim management menu.

---

## Understanding Roles

### Default Roles (Cannot Delete)

| Role        | Priority    | Description                       |
|-------------|-------------|-----------------------------------|
| **Owner**   | 0 (highest) | Full access to everything         |
| **Member**  | 1           | Standard member permissions       |
| **Default** | 2 (lowest)  | Visitor permissions (non-members) |

### Custom Roles

You can create unlimited custom roles like:

- Moderator
- Builder
- VIP
- Trusted

### Priority System

- Lower number = Higher rank
- Priority 0 is the highest (Owner)
- Higher priority members can manage lower priority members

---

## What Are Permissions?

Permissions are individual actions a role can or cannot do. There are 37 permissions in 5 categories:

| Category        | Examples                                       |
|-----------------|------------------------------------------------|
| **Management**  | Managing chunks, invites, bans, warps          |
| **Build**       | Placing blocks, breaking blocks, using buckets |
| **Interaction** | Opening containers, using doors, redstone      |
| **Entity**      | Interacting with animals, villagers, vehicles  |
| **Movement**    | Entering, teleporting, using warps             |

See [All Permissions](../protection/permissions.md) for the complete list.

---

## Tips

1. **Start with roles** - Set up roles before inviting members
2. **Use custom roles** - Create roles for specific purposes (Builder, Helper)
3. **Check priority** - Make sure moderators have higher priority than regular members
4. **Be careful with management permissions** - These are powerful!

---

## Next Steps

- [🔐 All Permissions](../protection/permissions.md) - Complete permission list
- [📖 roles.yml](../config/roles-yml.md) - Default role configuration

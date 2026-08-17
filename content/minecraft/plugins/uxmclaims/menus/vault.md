---
title: Vault (Storage)
order: 210
description: The shared storage every member with the permission can open.
icon: landmark
---

## How to Open

1. Open claim management
2. Click **Claim Vault** (`claimVault`)

---

## What Is This Menu For?

The vault acts like a shared chest that:

- Is tied to your claim (not a physical block)
- Can be accessed from anywhere via the menu
- Persists even when you're offline
- Has access controlled by permissions

---

## How It Works

When you open the vault:

1. A chest-like inventory appears
2. You can deposit items (move from your inventory)
3. You can withdraw items (take from vault)
4. Close with Escape or by clicking outside

---

## Access Control

Access depends on the `MANAGE_VAULT` permission:

| Role         | Default Access |
|--------------|----------------|
| Owner        | ✅ Full access  |
| Member       | ❌ No access    |
| Custom Roles | Configurable   |

To give vault access to a role:

1. Go to Roles menu
2. Click on the role
3. Enable `MANAGE_VAULT` permission

---

## Important Rules

### One User at a Time

Only one player can view the vault at a time.

If someone else is using it:
> "Someone is looking at claim vault right now. Please try again later."

### Rank Priority

If a higher-ranked member opens the vault while you're viewing:

- You're kicked out
- They get access

---

## Common Uses

- **Shared resources** - Building materials for members
- **Team storage** - Equipment for group activities
- **Safe keeping** - Valuable items protected by claim
- **Trading** - Leave items for members to pick up

---

## Tips

1. **Be careful who has access** - Only give `MANAGE_VAULT` to trusted members
2. **Don't use as personal storage** - It's shared, not private
3. **Keep organized** - Others will use it too

---

## Next Steps

- [🚩 Flags](../menus/flags.md) - Set claim rules
- [👥 Members & Roles](../menus/members-roles.md) - Manage access

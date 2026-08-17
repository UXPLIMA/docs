---
title: Main Menu (/claim)
order: 150
description: "The /claim hub: claiming the chunk you stand on, your claims, and public warps."
icon: layout-grid
---

## What Is This Menu For?

This is the **central hub** for the claim system. From here you can:

- View all your existing claims
- Create a new claim where you're standing
- Toggle border visualization
- Check your invitation inbox
- Browse public warps from other players

---

## Menu Items

### `myClaims` - My Claims

**Icon:** Paper 📄

**What it does:** Opens a list showing all claims you own. From this list, you can click on any claim to manage it.

**When to use:**

- When you want to manage an existing claim
- When you want to see how many claims you have
- When you want to teleport to one of your claims

---

### `buyClaim` - Claim This Chunk

**Icon:** Grass Block 🌿 (glowing)

**What it does:** Creates a new claim on the chunk (16x16 area) where you're currently standing.

**What happens when you click:**

1. The system checks if the chunk is claimable
2. If economy is enabled, you see the cost
3. You confirm the purchase
4. You enter a name for your new claim
5. Done - the chunk is now yours!

**The lore shows:** The cost in-game currency (`%cost%`)

**Requirements:**

- You must be standing on unclaimed land
- You must not be in a disabled world
- You must have remaining claim slots
- You must be far enough from other players' claims

---

### `showBorders` - Visualize Borders

**Icon:** Sea Lantern 🔵

**What it does:** Toggles display entities that show claim boundaries around you.

**When turned on, you see:**

| Color | Meaning |
|-------|---------|
| 🟢 Green | Your claims |
| 🔴 Red | Other players' claims |
| 🔵 Blue | Unclaimed land (wilderness) |

**When to use:**

- When you want to see exactly where your claim boundaries are
- When you want to check if nearby land is claimed
- When planning where to expand your claim

---

### `inviteInbox` - Invite Inbox

**Icon:** Globe Banner Pattern 📜

**What it does:** Opens your personal inbox of claim invitations. When other players invite you to join their claims,
the invitations appear here.

**From this menu you can:**

- See all pending invitations
- See who invited you and which claim
- Accept invitations (become a member)
- Decline invitations (reject and remove)

---

### `publicWarps` - Public Warps

**Icon:** Ender Pearl 📍 (glowing)

**What it does:** Opens a list of all warps that claim owners have made public. Anyone on the server can use these
warps.

**Useful for:**

- Finding player shops
- Visiting community farms
- Exploring other players' builds
- Quick travel across the server

**Note:** Using public warps may cost money depending on server settings.

---

### `closeMenu` - Close

**Icon:** Barrier ❌

**What it does:** Simply closes the menu. Same as pressing Escape.

---

## Decoration Items

### `filler`

The gray glass panes that fill empty slots. These are just for visual appearance and do nothing when clicked.

---

## Customizing This Menu

Server admins can customize this menu by editing `menu/common_main.yml`:

- Change item icons (`type`)
- Change item names (`name`)
- Change descriptions (`lore`)
- Move items to different positions (`slot`)
- Add glow effect (`glow: true`)

---

## Next Steps

- [🏠 Claim Management](../menus/claim-management.md) - Managing your claims
- [📖 Basic Concepts](../getting-started/concepts.md) - Understanding claims, chunks, and more

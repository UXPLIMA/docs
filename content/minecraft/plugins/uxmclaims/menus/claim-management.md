---
title: Claim Management
order: 160
description: The settings panel for a single claim, and everything reachable from it.
icon: square-pen
---

## How to Open This Menu

1. Type `/claim`
2. Click **My Claims** (`myClaims`)
3. Click on the claim you want to manage

---

## What Is This Menu For?

This menu is like the "settings panel" for your claim. From here you can:

- Teleport to your claim
- Change claim settings and flags
- Manage members, roles, and permissions
- Expand or shrink your territory
- Access the shared vault
- Extend claim duration
- Delete the claim

---

## Information Display

### `claimInformation` - Claim Information

**Icon:** Nether Star ⭐ (glowing)

**What it shows:**

- **Name:** The claim's display name
- **Owner:** Who owns this claim (you)
- **Size:** Number of chunks claimed
- **Members:** How many members are added
- **Vault:** Number of items in the vault
- **Created:** When the claim was created
- **Expires:** When the claim will expire

This is display-only - clicking does nothing.

---

## Menu Items

### `teleport` - Spawn Point

**Icon:** Compass 🧭

**What it does:** Manages the claim's spawn/teleport location.

**Actions:**
| Click | Action |
|-------|--------|
| Left-Click | Teleport to the claim spawn |
| Shift + Left-Click | Set spawn to your current location |

**Use this to:**

- Quickly travel to your claim from anywhere
- Move the spawn point to a better location (like your house entrance)

---

### `changeFlag` - Claim Flags

**Icon:** Oak Sign 🚩

**What it does:** Opens the flags menu where you control environmental rules.

**Examples of flags:**

- Allow or block PvP
- Allow or block explosions
- Allow or block mob spawning
- Control fire spread, fluid flow, etc.

See [Flags Menu](../menus/flags.md) for details.

---

### `warpList` - Warps

**Icon:** Ender Pearl 📍

**What it does:** Opens the warp management menu.

**From here you can:**

- View all warps in this claim
- Create new teleport points
- Make warps public or private
- Rename or delete warps

---

### `changeName` - Rename Claim

**Icon:** Name Tag 📝

**What it does:** Opens a text input to change the claim's name.

**Rules:**

- At least 3 characters
- Most characters allowed including spaces and some special characters

---

### `claimMap` - Claim Map

**Icon:** Filled Map 🗺️

**What it does:** Opens an overhead view of your claim and surrounding chunks.

**From this map you can:**

- See which chunks you own (green)
- See other players' chunks (red)
- See unclaimed wilderness (gray)
- Buy new adjacent chunks (click on gray squares)
- Remove chunks you no longer want

See [Claim Map](../menus/claim-map.md) for details.

---

### `memberList` - Members

**Icon:** Player Head 👥

**What it does:** Opens a list of all players who are members of this claim.

**From here you can:**

- See all current members
- See each member's role
- Change member roles (promote/demote)
- Kick members from the claim

---

### `roleList` - Roles

**Icon:** Book 📖

**What it does:** Opens the role management menu.

**From here you can:**

- View all roles (Owner, Member, Default, custom)
- Create new custom roles
- Edit role permissions
- Adjust role priorities
- Delete custom roles

---

### `inviteList` - Invitations

**Icon:** Writable Book 📜

**What it does:** Shows all pending invitations you've sent.

**From here you can:**

- See who you've invited
- Revoke (cancel) pending invitations
- Send new invitations

---

### `banList` - Banned Players

**Icon:** Iron Bars 🚫

**What it does:** Opens the ban list for this claim.

**From here you can:**

- See all banned players
- Unban players
- Ban new players

Banned players cannot enter or interact with your claim.

---

### `claimBlock` - Claim Blocks

**Icon:** Beacon 🧱 (glowing)

**What it does:** Opens the claim block style menu.

**From here you can:**

- See available block styles (Bedrock, Obsidian, etc.)
- Change the visual appearance of your claim boundary blocks
- Some styles may require special permissions

---

### `addTime` - Reschedule Claim

**Icon:** Clock ⏰

**What it does:** Opens the time extension menu.

**From here you can:**

- See current expiration date
- Add more time to your claim
- Pay the time extension cost (if economy is enabled)

**Important:** If your claim expires, it gets deleted automatically!

---

### `claimVault` - Claim Vault

**Icon:** Chest 📦

**What it does:** Opens the shared storage vault.

**Features:**

- Store items for members to access
- Access controlled by `MANAGE_VAULT` permission
- Items persist even when you're offline

---

### `back` - Close

**Icon:** Barrier ❌

**What it does:** Closes the menu and returns to the claim list.

---

### `delete` - Delete Claim

**Icon:** Lava Bucket 🗑️

**What it does:** Permanently deletes this claim.

⚠️ **WARNING:** This cannot be undone!

When deleted:

- All protection is removed
- All members are kicked
- All warps are deleted
- All vault items are... (depends on server config)

You'll be asked to confirm before deletion happens.

---

## Decoration Items

### `borderFiller`

Black glass panes that form the menu border. Visual only.

---

## Customizing This Menu

Server admins can edit `menu/claim_main.yml` to:

- Change icons
- Rearrange item positions
- Modify names and descriptions
- Enable/disable glow effects

---

## Next Steps

- [👥 Members & Roles](../menus/members-roles.md) - Managing who can access your claim
- [🚩 Flags](../menus/flags.md) - Setting claim rules

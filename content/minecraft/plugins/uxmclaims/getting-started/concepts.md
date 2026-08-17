---
title: Basic Concepts
order: 50
description: Chunks, claims, roles and flags — the four ideas everything else builds on.
icon: lightbulb
---

## 🧱 Chunk

A **chunk** is a 16x16 block area in Minecraft. It goes from bedrock (y=0) all the way up to the sky (y=320+).

Think of your Minecraft world as a grid of squares. Each square is a chunk.

| Chunk | Chunk | Chunk |
| ----- | -------------- | ----- |
| Chunk | **YOUR CLAIM** | Chunk |
| Chunk | Chunk          | Chunk |

When you claim land, you claim whole chunks. You can't claim half a chunk.

<Callout type="tip" title="How Big is a Chunk?">

A chunk is 16×16 blocks = **256 blocks of area**. For a small house, one chunk is usually enough. For a large base, you
might need 4-9 chunks.

</Callout>

---

## 🏠 Claim

A **claim** is your protected territory. It consists of:

- **One or more chunks** (the land itself)
- **An owner** (you)
- **Members** (people you invite)
- **Flags** (rules for the claim)
- **A name** (what you call it)

You can have multiple claims in different locations.

---

## 👥 Members

**Members** are players you've added to your claim. They can:

- Enter your claim
- Do things based on their role/permissions
- Access the vault (if allowed)

There are different levels of access:

| Who          | Access Level                        |
|--------------|-------------------------------------|
| **Owner**    | Can do everything, manage the claim |
| **Members**  | Can do things based on their role   |
| **Visitors** | Non-members, very limited access    |

---

## 🎭 Roles

A **role** is a set of permissions. Instead of giving permissions one by one, you assign a role.

**Default roles:**

| Role        | What They Can Do                 |
|-------------|----------------------------------|
| **Owner**   | Everything (full control)        |
| **Member**  | Build, open containers, interact |
| **Default** | Only walk inside (visitor role)  |

You can create **custom roles** like "Moderator" or "Builder" with specific permissions.

---

## 🚩 Flags

**Flags** control what happens inside your claim. They affect **everyone**, including you.

Examples:

| Flag               | OFF                           | ON                      |
|--------------------|-------------------------------|-------------------------|
| **PVP**            | Players can't hurt each other | PvP is enabled          |
| **TNT_EXPLOSIONS** | TNT doesn't explode           | TNT works normally      |
| **FIRE_SPREAD**    | Fire doesn't spread           | Fire spreads to blocks  |
| **SPAWN_MONSTERS** | Monsters don't spawn          | Monsters spawn normally |

Think of flags like global rules for your claim.

---

## 🔐 Permissions

**Permissions** control what specific actions a member can do. They are given to roles.

Examples:

| Permission         | What It Allows             |
|--------------------|----------------------------|
| **BLOCK_PLACE**    | Place blocks               |
| **BLOCK_BREAK**    | Break blocks               |
| **CONTAINER_OPEN** | Open chests, barrels, etc. |
| **TELEPORT**       | Use claim teleport         |
| **MANAGE_INVITES** | Send invitations           |

There are **37 different permissions** you can mix and match!

---

## 🔀 Flags vs Permissions: What's the Difference?

This confuses many people, so let's be clear:

| Concept         | Applies To                 | Example                           |
|-----------------|----------------------------|-----------------------------------|
| **Flags**       | Everyone (including owner) | "Fire can't spread in this claim" |
| **Permissions** | Specific roles             | "Members can place blocks"        |

**Flags** are like **laws of physics** in your claim.
**Permissions** are like **access cards** for different people.

---

## 📍 Warps

A **warp** is a teleport point inside your claim.

- You can create multiple warps
- Warps can be **public** (anyone can use) or **private** (members only)
- Great for shops, farms, or special locations

---

## 📦 Vault

The **vault** is a shared storage chest for your claim.

- Store items that members can access
- Access is based on permissions
- Items persist even if you go offline

---

## ⏱️ Claim Expiration

Claims can **expire** after a certain time.

- Default is usually 7-30 days
- You can extend time (may cost money)
- Expired claims are deleted automatically

This prevents abandoned claims from cluttering the server.

<Callout type="note" title="Server Setting">

Whether claims expire depends on server configuration. Ask your server admin!

</Callout>

---

## Summary Table

| Term       | Simple Explanation                        |
|------------|-------------------------------------------|
| Chunk      | A 16×16 block area                        |
| Claim      | Your protected land (one or more chunks)  |
| Member     | Player added to your claim                |
| Role       | A set of permissions                      |
| Flag       | A rule that affects everyone in the claim |
| Permission | Something a role can or can't do          |
| Warp       | A teleport point                          |
| Vault      | Shared storage                            |

---

## Ready for More?

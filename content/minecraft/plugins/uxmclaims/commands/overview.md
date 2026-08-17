---
title: Overview
order: 60
description: How the command tree is organised, and when to use it instead of the menus.
icon: book-open
---

## Command Overview

uxmClaims offers both **GUI (menus)** and **CLI (commands)** for managing claims.

<Callout type="tip" title="For New Players">

Just type `/claim` and use the menu! Commands are optional for power users.

</Callout>

---

## Selection System

Most commands operate on the claim you are currently standing in.

- **Automatic:** If you're inside a claim, commands will affect that claim.
- **Manual (Menu):** Use `/claim list` to see and manage your claims if you are not standing in one.

---

## Confirmation System

Dangerous actions require confirmation before execution. When you perform a destructive action (like deleting a claim), you'll see a prompt with clickable buttons.

| Command | Description |
|---------|-------------|
| `/claim confirm` | Confirms a pending action |
| `/claim cancel` | Cancels a pending action |

**Actions requiring confirmation:**

- `/claim delete` - Deleting a claim
- `/claim role delete <name>` - Deleting a role
- `/claim warp delete <name>` - Deleting a warp
- `/claim kick <player>` - Kicking a member
- `/claim ban <player>` - Banning a player

<Callout type="info" title="Confirmation Timeout">

Confirmations expire after **30 seconds**. You can also click the `[CONFIRM]` or `[CANCEL]` buttons in chat.

</Callout>

---

## Quick Reference

| Category | Commands | Description |
|----------|----------|-------------|
| [Basic](basic.md) | `/claim`, `/claim list`, `/claim create` | Core claim operations |
| [Members](members.md) | `/claim kick`, `/claim ban`, `/claim setrole` | Member management |
| [Roles](roles.md) | `/claim role create`, `/claim role setperm` | Role management |
| [Warps](warps.md) | `/claim warp create`, `/claim warp teleport` | Teleport points |
| [Chunks](chunks.md) | `/claim chunk add`, `/claim chunk remove` | Claim area management |
| [Invites](invites.md) | `/claim trust`, `/claim invites` | Invitations |
| [Flags](basic.md#managing-flags) | `/claim setflag` | Claim settings |
| [Storage](../menus/vault.md) | `/claim vault` | Claim shared storage |
| [Utility](basic.md) | `/claim setspawn`, `/claim spawn` | Moving spawn & fast travel |
| [Admin](admin.md) | `/claim admin view`, `/claim admin bulk` | Server-wide management |

---

## Tab Completion

Press **TAB** while typing to see suggestions for claim names, players, roles, and more.

---

## Next Steps

- [🎮 Main Menu](../menus/main-menu.md) - GUI-based management
- [🚩 All Flags](../protection/flags.md) - Available claim flags
- [🔐 All Permissions](../protection/permissions.md) - Plugin & Role permissions

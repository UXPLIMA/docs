---
title: Invite Commands
order: 120
---

## Available Commands

| Command | Description |
|---------|-------------|
| `/claim invite send <player>` | Sends an invitation |
| `/claim invite inbox` | Opens your invite inbox |
| `/claim invite accept <claim>` | Accepts an invitation |
| `/claim invite reject <claim>` | Declines an invitation |
| `/claim invite revoke <player>` | Revokes a sent invitation |

---

## How Invitations Work

1. **Owner sends invite** → `/claim invite send PlayerName` (or `/claim trust`)
2. **Player receives notification** with clickable buttons
3. **Player accepts:**
    - By clicking `[ACCEPT]` in chat
    - By typing `/claim invite accept <claimName>`
    - By opening the inbox `/claim invites`

---

## Command Details

### `/claim invite send <player>`

Sends an invitation to join your claim.

**Requirements:**

- You must have permission to invite (check your role)
- Player must not already be a member
- Player must not be banned

---

### `/claim invite inbox`

Opens your personal inbox showing all pending invitations. Same as `/claim invites`.

From this menu you can:

- See which claims invited you
- See who sent the invitation
- Accept (become a member)
- Decline (remove invitation)

---

### `/claim invite accept <claim>`

Accepts a pending invitation from a specific claim.

**Example:**
```
/claim invite accept MyBase
```

---

### `/claim invite reject <claim>`

Declines (removes) a pending invitation from a specific claim.

**Example:**
```
/claim invite reject MyBase
```

---

### `/claim invite revoke <player>`

Cancels a pending invitation you sent, before the player accepts it.

---

## Examples

### Inviting a Friend

```
/claim invite send FriendPlayer
```

### Checking Your Invitations

```
/claim invites
```

---

## Next Steps
- [👥 Member Commands](members.md) - Managing members after they join
- [📬 Invitations Menu](../menus/invitations.md) - GUI invite management

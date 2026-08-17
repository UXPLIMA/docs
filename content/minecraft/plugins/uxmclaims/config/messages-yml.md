---
title: messages.yml
order: 270
description: Every message, its type — chat, title or action bar — and its placeholders.
icon: message-square
---

## Message Structure

Every message in uxmClaims follows this structure:

```yaml
messageName:
  type: 'CHAT'          # Message type: CHAT, TITLE, or ACTIONBAR
  text: 'Your message'  # The message content (string or list)
  sound: 'SOUND_NAME'   # Optional: Minecraft sound effect
  volume: 1.0           # Optional: Sound volume (0.0 - 1.0)
  pitch: 1.0            # Optional: Sound pitch (0.5 - 2.0)
```

### Multi-Line Messages

The `text` field can be either a single string or a list for multi-line messages:

```yaml
# Single line
text: 'This is a single line message.'

# Multi-line using a list
text:
  - '<gray>───────────────────────</gray>'
  - '<gold><bold>⚠ Warning!</bold></gold>'
  - ''
  - '<gray>This action is irreversible.</gray>'
  - '<gray>───────────────────────</gray>'
```

When using a list, each item becomes a separate line in the message.

### Message Types

| Type        | Where It Appears              |
|-------------|-------------------------------|
| `CHAT`      | Chat window                   |
| `TITLE`     | Big text in center of screen  |
| `ACTIONBAR` | Text above hotbar             |

### Sound Settings

| Setting  | Description                 | Range     |
|----------|-----------------------------|-----------|
| `sound`  | Minecraft sound name        | See below |
| `volume` | How loud the sound plays    | 0.0 - 1.0 |
| `pitch`  | How high/low the sound is   | 0.5 - 2.0 |

**Common sounds:**

- `ENTITY_PLAYER_LEVELUP` - Level up sound
- `ENTITY_EXPERIENCE_ORB_PICKUP` - XP pickup
- `ENTITY_VILLAGER_NO` - Error/denial sound
- `ENTITY_ENDERMAN_TELEPORT` - Teleport sound
- `UI_BUTTON_CLICK` - Click sound
- `BLOCK_NOTE_BLOCK_PLING` - Pling sound

**To disable sound:** Set `sound: ''`

---

## Format Settings

```yaml
timeFormat: 'dd days HH hours mm minutes ss seconds'
dateFormat: 'dd-MM-yyyy HH:mm:ss'
statusActive: 'Active'
statusPassive: 'Passive'
```

| Setting         | What It Does                | Example Output              |
|-----------------|-----------------------------|-----------------------------
| `timeFormat`    | How durations are displayed | "5 days 3 hours 20 minutes" |
| `dateFormat`    | How dates are shown         | "25-12-2024 14:30:00"       |
| `statusActive`  | Text for "on" status        | "Active"                    |
| `statusPassive` | Text for "off" status       | "Passive"                   |

---

## Message Categories

### ✅ Success Messages

Shown when actions complete successfully:

```yaml
successClaimCreated:
  type: 'CHAT'
  text: 'Claim "%claim.name%" has been successfully created!'
  sound: 'ENTITY_PLAYER_LEVELUP'
  volume: 1.0
  pitch: 1.0

successClaimDeleted:
  type: 'CHAT'
  text: 'Claim "%claim.name%" has been successfully deleted!'
  sound: 'ENTITY_ITEM_BREAK'
  volume: 1.0
  pitch: 1.0

successClaimRenamed:
  type: 'CHAT'
  text: 'Claim renamed to "%claim.name%!"'
  sound: 'ENTITY_EXPERIENCE_ORB_PICKUP'
  volume: 1.0
  pitch: 1.0
```

**Available placeholders:**

- `%claim.name%` - Claim's name
- `%claim.id%` - Claim's ID
- `%claim.owner.name%` - Owner's name

---

### ℹ️ Info Messages

Informational messages:

```yaml
infoTeleportPending:
  type: 'CHAT'
  text: 'Please wait %seconds% seconds, you are teleporting to claim.'
  sound: 'BLOCK_NOTE_BLOCK_PLING'
  volume: 0.5
  pitch: 1.0

infoClaimFlagUpdated:
  type: 'CHAT'
  text: 'Flag %flag% updated: %status%.'
  sound: ''
  volume: 1.0
  pitch: 1.0
```

---

### 🎯 Claim Enter/Leave Messages

These messages use `TITLE` type by default and support special formatting:

```yaml
infoClaimMoveInside:
  type: 'TITLE'
  text: '%claim.name%\nYou entered claim %claim.name%'
  sound: 'BLOCK_NOTE_BLOCK_CHIME'
  volume: 0.5
  pitch: 1.2

infoClaimMoveOutside:
  type: 'TITLE'
  text: '%claim.name%\nYou left claim %claim.name%'
  sound: 'BLOCK_NOTE_BLOCK_BASS'
  volume: 0.5
  pitch: 0.8
```

**Note:** For `TITLE` type, use a list format to separate title and subtitle:

```yaml
infoClaimMoveInside:
  type: 'TITLE'
  text:
    - '%claim.name%'                      # Title (first line)
    - 'You entered claim %claim.name%'    # Subtitle (second line)
  sound: 'BLOCK_NOTE_BLOCK_CHIME'
```

| Format                         | Result                                   |
|--------------------------------|------------------------------------------|
| `text: ['Title', 'Subtitle']`  | Title on first line, subtitle on second  |
| `text: 'Only Title'`           | Just the title, no subtitle              |

---

### ❌ Error Messages

Shown when something goes wrong:

```yaml
errorNoPermission:
  type: 'CHAT'
  text: 'You don''t have permission to do this!'
  sound: 'ENTITY_VILLAGER_NO'
  volume: 1.0
  pitch: 1.0

errorClaimLimitExceed:
  type: 'CHAT'
  text: 'You reached the maximum number of main claims.'
  sound: 'ENTITY_VILLAGER_NO'
  volume: 1.0
  pitch: 1.0

errorInsufficientFunds:
  type: 'CHAT'
  text: 'You don''t have enough money to do this!'
  sound: 'ENTITY_VILLAGER_NO'
  volume: 1.0
  pitch: 1.0
```

---

### 📢 Broadcast Messages

Server-wide announcements:

```yaml
broadcastClaimExpired:
  type: 'CHAT'
  text: 'Claim "%claim.name%" has expired.'
  sound: ''
  volume: 1.0
  pitch: 1.0
```

---

## Using Colors

You can use MiniMessage format for colors:

```yaml
successClaimCreated:
  type: 'CHAT'
  text: '<green>Claim created successfully!</green>'
  sound: 'ENTITY_PLAYER_LEVELUP'
  volume: 1.0
  pitch: 1.0
```

**Common color tags:**

- `<red>`, `<green>`, `<blue>`, `<yellow>`, `<gold>`, `<gray>`
- `<bold>`, `<italic>`, `<underlined>`
- `<gradient:red:blue>Rainbow text</gradient>`

---

## Placeholder Defaults

```yaml
placeholderDefaults:
  uxmclaims_claim_name: 'No claim'
  uxmclaims_claim_owner_name: 'No claim'
```

These values show when a placeholder has no data (e.g., player isn't in a claim).

---

## All Message Keys

### Success Messages

| Key                           | Description                    |
|-------------------------------|--------------------------------|
| `successClaimCreated`         | Claim created                  |
| `successClaimDeleted`         | Claim deleted                  |
| `successClaimRenamed`         | Claim renamed                  |
| `successClaimRelocated`       | Spawn location updated         |
| `successClaimTeleported`      | Teleported to claim            |
| `successClaimBlockPlaced`     | Claim block placed             |
| `successClaimBlockDestroyed`  | Claim block removed            |
| `successClaimTimeAdjusted`    | Claim time updated             |
| `successMemberRemoved`        | Member removed                 |
| `successMemberBanned`         | Member banned                  |
| `successMemberUnbanned`       | Member unbanned                |
| `successInviteSent`           | Invite sent                    |
| `successInviteAccepted`       | Invite accepted                |
| `successInviteDeclined`       | Invite declined                |
| `successRoleCreated`          | Role created                   |
| `successRoleDeleted`          | Role deleted                   |
| `successRoleRenamed`          | Role renamed                   |
| `successWarpCreated`          | Warp created                   |
| `successWarpDeleted`          | Warp deleted                   |
| `successWarpRenamed`          | Warp renamed                   |
| `successWarpTeleported`       | Teleported to warp             |

### Error Messages

| Key                           | Description                    |
|-------------------------------|--------------------------------|
| `errorNoPermission`           | No permission                  |
| `errorMaxMembers`             | Member limit reached           |
| `errorClaimLimitExceed`       | Claim limit reached            |
| `errorChunkLimitExceed`       | Chunk limit reached            |
| `errorRoleLimitExceeded`      | Role limit reached             |
| `errorBanLimitExceeded`       | Ban limit reached              |
| `errorWarpLimitExceeded`      | Warp limit reached             |
| `errorInviteLimitExceeded`    | Invite limit reached           |
| `errorInsufficientFunds`      | Not enough money               |
| `errorClaimTooClose`          | Too close to another claim     |
| `errorNotClaimableArea`       | Cannot claim here              |
| `errorWorldDisabled`          | World is disabled for claims   |
| `errorClaimNotFound`          | Claim not found                |
| `errorMemberNotFound`         | Member not found               |
| `errorRoleNotFound`           | Role not found                 |
| `errorWarpNotFound`           | Warp not found                 |
| `errorTeleportCancelled`      | Teleport cancelled (moved)     |

---

## Tips

1. **Keep it consistent** - Use similar formatting for similar messages
2. **Use colors wisely** - Green for success, red for errors
3. **Match sounds to actions** - Level up for good, villager no for bad
4. **Test after changes** - Use `/claim reload` and test in-game
5. **Escape quotes** - Use `''` for apostrophes in YAML

---

## Next Steps

- [🔔 webhooks.yml](../config/webhooks-yml.md) - Set up Discord notifications
- [🎨 Menu Customization](../config/menus.md) - Customize menu appearance

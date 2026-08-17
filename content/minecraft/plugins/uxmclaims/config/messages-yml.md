---
title: messages.yml
order: 504
description: Every message the plugin sends, its delivery type, and the formats.
icon: message-square
---

Every string a player sees is here. Messages are MiniMessage, and placeholders are parsed when
`generalSettings.placeholderSupport` is on.

## The shape of an entry

```yaml
successClaimCreated:
  type: 'CHAT'
  text: 'Claim "%claim.name%" has been successfully created!'
```

| Key | Meaning |
|---|---|
| `type` | `CHAT`, `TITLE` or `ACTIONBAR` |
| `text` | A single string, or a list of lines |

A `TITLE` message takes a two-line list — the title and the subtitle:

```yaml
infoClaimMoveInside:
  type: 'TITLE'
  text:
    - '%claim.name%'
    - 'You entered claim %claim.name%'
```

A `CHAT` message given a list sends one line per entry.

## The formats at the top

| Key | Default | Used by |
|---|---|---|
| `timeFormat` | `dd days HH hours mm minutes ss seconds` | The `time:` modifier |
| `dateFormat` | `dd-MM-yyyy HH:mm:ss` | The `date:` modifier |
| `statusActive` | `Active` | The `status:` modifier, for true |
| `statusPassive` | `Passive` | The `status:` modifier, for false |

Changing `dateFormat` changes every rendered date at once — holograms, menus, map tooltips and chat.

## The groups

| Prefix | Count | What they cover |
|---|---|---|
| `success…` | ~35 | Something worked |
| `error…` | ~50 | Something was refused, and why |
| `info…` | ~15 | Neutral state changes — entering a claim, a pending teleport |
| `notification…` | 1 | The invitation a player receives |
| `warning…` | 1 | The PvP warp confirmation |
| `broadcast…` | 1 | A claim expired |

## Messages worth rewriting first

| Key | Why |
|---|---|
| `notificationInviteReceived` | Carries the clickable Accept and Reject buttons |
| `infoClaimMoveInside` / `infoClaimMoveOutside` | Fire on every border crossing, so they are the most-seen strings on the server |
| `claimChatFormat` | The prefix for `/claim chat` |
| `errorNoPermission` | The catch-all refusal, shown for both ability and role failures |
| `infoClaimExpiringReminder` | Repeats every 5 minutes for a day before expiry |

The invitation message is a list, and its second line is the button row:

```yaml
notificationInviteReceived:
  type: 'CHAT'
  text:
    - 'You have been invited to join claim %claim.name%!'
    - "<click:run_command:/claim invite accept %claim.name%><hover:show_text:'Click to accept'>[Accept]</hover></click> <click:run_command:/claim invite reject %claim.name%><hover:show_text:'Click to reject'>[Reject]</hover></click>"
```

The click targets are ordinary commands. Keep them intact when you translate the visible text.

## Placeholder defaults

```yaml
placeholderDefaults:
  uxmclaims_claim_name: 'No claim'
  uxmclaims_claim_owner_name: 'No claim'
```

What `%uxmclaims_claim_name%` and `%uxmclaims_claim_owner_name%` render when the player is not in a
claim. Add entries here for any other placeholder you put on a scoreboard — without one, an
unresolvable path renders empty.

## Notes

- **`errorNoPermission` is shown for two different failures.** A missing ability node and a missing
  role permission produce the same string. If players are confused about which they hit, split the
  wording — mention both causes — rather than trying to find a second key.

- **`infoTeleportPending` carries `%seconds%`,** the resolved warmup from
  `uxmclaims.delay.teleport`. A rank with a 0-second delay never sees it.

- **`errorVaultBusy`, `errorVaultInUse` and `errorVaultAlreadyInUse` are three different states:**
  someone is looking at it, someone of higher rank took it, and it is simply locked. Wording them
  identically loses information players can act on.

- **MiniMessage only.** Legacy `&` codes are not translated. Use `<red>` and `<gradient:…>`.

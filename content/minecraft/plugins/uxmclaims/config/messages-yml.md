---
title: messages.yml
order: 504
description: Every message the plugin sends, its delivery type, sound and the shared formats.
icon: message-square
---

Every chat line, title and action bar a player sees comes from this file. Texts are MiniMessage, and
the plugin's own placeholders (`%claim.name%`, `%player.name%` and the rest, see
[Placeholders](../placeholders/reference.md)) are filled in before sending. The file ships in eight
languages, see [Languages](languages.md).

## The shape of an entry

```yaml
successClaimCreated:
  type: 'CHAT'
  text: '<white>Claim</white> <green>%claim.name%</green> <white>created successfully!</white>'
  sound: 'ENTITY_PLAYER_LEVELUP'
  volume: 0.8
  pitch: 1.2
```

| Key | Default | Meaning |
|---|---|---|
| `type` | `CHAT` | `CHAT`, `TITLE` or `ACTIONBAR`; anything else is sent as `CHAT` |
| `text` | | A single string, or a list of lines |
| `sound` | none | A Bukkit sound name or a namespaced key (`minecraft:block.note_block.pling`); empty plays nothing |
| `volume` | `1.0` | Sound volume |
| `pitch` | `1.0` | Sound pitch |

A `CHAT` message given a list sends one line per entry. A `TITLE` message takes a two-line list, the
title and the subtitle, and shows for 0.5 seconds fade-in, 3.5 seconds and 1 second fade-out:

```yaml
infoClaimMoveInside:
  type: 'TITLE'
  text:
    - '<green>%claim.name%</green>'
    - '<gray>You entered this claim</gray>'
```

An `ACTIONBAR` message takes a single string.

## The formats at the top

| Key | Default | Used by |
|---|---|---|
| `timeFormat` | `dd days HH hours mm minutes ss seconds` | The `time:` modifier |
| `dateFormat` | `dd-MM-yyyy HH:mm:ss` | The `date:` modifier |
| `statusActive` | `<green>Enabled</green>` | The `status:` modifier, for true |
| `statusPassive` | `<red>Disabled</red>` | The `status:` modifier, for false |
| `chatInputCancelWord` | `cancel` | Typed in chat to cancel a chat input; `cancel` always works as well |

Changing `dateFormat` changes every rendered date at once: holograms, menus, map tooltips, webhooks
and chat. Each translation ships its own cancel word (`iptal`, `abbrechen`, `annuler` and so on), so
mention it in `infoChatInputInstruction` if you change it.

## The groups

| Prefix | Count | What they cover |
|---|---|---|
| `success…` | 38 | Something worked |
| `error…` | 88 | Something was refused, and why |
| `info…` | 20 | Neutral state changes: entering a claim, a pending teleport, chat input |
| `confirmation…` | 5 | The confirmation prompt and its outcomes |
| `notification…` | 1 | The invitation a player receives |
| `warning…` | 1 | The PvP warp confirmation |
| `broadcast…` | 1 | A claim expired |
| `claimChatFormat` | 1 | The line format of `/claim chat` |

## Messages worth rewriting first

| Key | Why |
|---|---|
| `notificationInviteReceived` | Carries the clickable Accept and Reject buttons |
| `infoClaimMoveInside` / `infoClaimMoveOutside` | Fire on every border crossing, so they are the most-seen strings on the server |
| `confirmationRequired` | Carries the clickable Confirm and Cancel buttons |
| `errorNoPermission` | The refusal for a missing claim role permission |
| `errorAbilityPermissionDenied` | The refusal for a missing `uxmclaims.ability.*` node |
| `infoClaimExpiringReminder` | Repeats every 5 minutes for a day before expiry |

The invitation message is a list, and its second line is the button row:

```yaml
notificationInviteReceived:
  type: 'CHAT'
  text:
    - '<white>You have been invited to join claim</white> <gold>%claim.name%</gold><white>!</white>'
    - "<click:run_command:/claim invite accept %claim.name%><hover:show_text:'Click to accept'>[Accept]</hover></click> <click:run_command:/claim invite reject %claim.name%><hover:show_text:'Click to reject'>[Reject]</hover></click>"
```

The click targets are ordinary commands. Keep them intact when you reword the visible text.

## Placeholder defaults

```yaml
placeholderDefaults:
  uxmclaims_claim_name: 'Wilderness'
  uxmclaims_claim_owner_name: 'None'
```

What a `%uxmclaims_...%` PlaceholderAPI placeholder renders when no claim applies to the player (no
selected claim, not standing in one, not owning exactly one) or the value cannot be resolved. The key
is the placeholder without its percent signs. Add an entry for every placeholder you put on a scoreboard:
without one, PlaceholderAPI shows the placeholder text as it is.

## Notes

- **Two refusals, two keys.** `errorNoPermission` answers a missing role permission in the claim;
  `errorAbilityPermissionDenied` answers a missing ability node. Word them differently so players
  and staff can tell which one they hit.
- **`infoTeleportPending` carries `%seconds%`,** the resolved warmup from `uxmclaims.delay.teleport`.
  A rank with a 0-second delay never sees it.
- **Only `errorVaultAlreadyInUse` is sent for the vault:** another player has it open. `errorVaultBusy`
  and `errorVaultInUse` are kept for compatibility and are never sent in this release.
- **`errorClaimBanned` and `errorClaimNoEntry` are action bars** sent by the access guard when it
  ejects a player.
- **MiniMessage only.** Legacy `&` codes are not translated. Use `<red>` and `<gradient:…>`.

Related: [Placeholders](../placeholders/reference.md), [Languages](languages.md)

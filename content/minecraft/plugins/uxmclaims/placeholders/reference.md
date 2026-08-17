---
title: Reference
order: 701
description: The uxmclaims expansion, the path syntax, and the modifiers.
icon: braces
---

uxmClaims registers a PlaceholderAPI expansion with the identifier `uxmclaims`, and uses the same
engine for the tokens inside `messages.yml`, `webhooks.yml`, holograms and menus.

Set `generalSettings.placeholderSupport: true` (the default) and install PlaceholderAPI.

## How a placeholder resolves

Everything after `uxmclaims_` is a **path** into the claim object:

```
%uxmclaims_claim_name%          →  the claim's name
%uxmclaims_claim_owner_name%    →  the owner's name
%uxmclaims_claim_expireDate%    →  the raw expiry timestamp
```

Which claim? The one selected with `/claim select`, if any; otherwise the one the player is standing
in. Outside a claim, `%uxmclaims_claim_name%` and `%uxmclaims_claim_owner_name%` fall back to the
values in `messageFile.placeholderDefaults` (`No claim` for both) and every other path returns
empty.

Separators are interchangeable: `claim_name` and `claim.name` are the same path. Inside `messages.yml`
the dotted form is conventional; PlaceholderAPI is happier with underscores.

## The special one

| Placeholder | Renders |
|---|---|
| `%uxmclaims_is_claimable%` | `true` if the chunk the player stands in is unclaimed |

This is the only placeholder that works outside a claim, because it is answered before the claim is
resolved.

## Common paths

| Placeholder | Renders |
|---|---|
| `%uxmclaims_claim_name%` | The claim's name |
| `%uxmclaims_claim_id%` | Its UUID |
| `%uxmclaims_claim_owner_name%` | The owner's name |
| `%uxmclaims_count:claim_chunks%` | How many chunks it holds |
| `%uxmclaims_count:claim_members%` | How many members |
| `%uxmclaims_count:claim_warps%` | How many warps |
| `%uxmclaims_count:claim_roles%` | How many roles |
| `%uxmclaims_count:claim_bans%` | How many bans |
| `%uxmclaims_count:claim_invites%` | How many pending invitations |
| `%uxmclaims_count:claim_vault_items%` | How many items are in the vault |
| `%uxmclaims_time:claim_remainTime%` | Time left before expiry, formatted |
| `%uxmclaims_date:claim_expireDate%` | The expiry date, formatted |
| `%uxmclaims_date:claim_creationDate%` | When it was created |
| `%uxmclaims_claim_location_world_name%` | The world it sits in |
| `%uxmclaims_claim_location_x%` | Spawn x |
| `%uxmclaims_claim_location_y%` | Spawn y |
| `%uxmclaims_claim_location_z%` | Spawn z |

## Modifiers

A modifier goes before a colon and transforms the value.

| Modifier | Turns | Into |
|---|---|---|
| `count:` | A list or map | Its size |
| `time:` | A duration or millisecond count | `timeFormat` from `messages.yml` |
| `date:` | An instant or millisecond timestamp | `dateFormat` from `messages.yml` |
| `status:` | A boolean, or an integer | `statusActive` / `statusPassive` |
| `float:` | A number | Two decimal places |
| `upper:` | Text | Uppercase |
| `lower:` | Text | Lowercase |

```
%uxmclaims_count:claim_members%     →  7
%uxmclaims_time:claim_remainTime%   →  12 days 06 hours 41 minutes 02 seconds
%uxmclaims_status:claim_flags%      →  Active
```

Without `count:`, `%uxmclaims_claim_members%` renders the raw map, which is not what you want in a
scoreboard.

The formats themselves are `timeFormat` and `dateFormat` at the top of `messages.yml`, defaulting to
`dd days HH hours mm minutes ss seconds` and `dd-MM-yyyy HH:mm:ss`.

## Tokens inside plugin files

`messages.yml`, `webhooks.yml`, the hologram lines and the menus use the same engine without the
`uxmclaims_` prefix, and get extra objects in context depending on where they are used.

| Token | Available in |
|---|---|
| `%claim.name%`, `%claim.owner.name%`, `%claim.id%` | Anywhere a claim is in context |
| `%player.name%` | Messages sent to a player |
| `%member.name%`, `%member.joinDate%`, `%member.texture%` | Member menus and messages |
| `%role.name%`, `%role.priority%`, `%role.type%` | Role menus and messages |
| `%warp.name%`, `%warp.location%`, `%warp.isPublic%`, `%warp.createdBy%` | Warp menus and messages |
| `%invite.name%`, `%invite.invitedAt%`, `%invite.texture%` | Invitation menus |
| `%ban.name%`, `%ban.reason%`, `%ban.bannedAt%` | Ban menus |
| `%chunk.world%`, `%chunk.x%`, `%chunk.z%` | The chunk map |
| `%flag%`, `%permission%`, `%status%`, `%priority%`, `%cost%`, `%seconds%` | The message that carries them |

`%member.texture%`, `%invite.texture%` and `%ban.texture%` resolve to the player's skin, which is how
menu heads are rendered.

<Callout type="info" title="Any getter on the claim is reachable">

The path is resolved reflectively against the domain model, so a path that maps to a getter works even
if it is not in the table above. `%uxmclaims_claim_block_keyId%` reads the claim block's style key.
Paths that do not resolve render as `null` rather than failing, which is the quickest way to tell a
typo from a missing value.

</Callout>

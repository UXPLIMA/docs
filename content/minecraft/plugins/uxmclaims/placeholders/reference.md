---
title: Reference
order: 701
description: The uxmclaims expansion, the path syntax, the modifiers and the tokens in plugin files.
icon: braces
---

uxmClaims has one placeholder engine. PlaceholderAPI reaches it through the `uxmclaims` expansion;
`messages.yml`, `webhooks.yml`, the menus, the dialogs, the holograms and the map tooltips use it
directly, without the `uxmclaims_` prefix.

The expansion registers when `generalSettings.placeholderSupport` is `true` (the default) and
PlaceholderAPI is installed. The tokens in uxmClaims' own files work either way.

## How a PlaceholderAPI placeholder resolves

Everything after `uxmclaims_` is a **path** that starts at the claim:

```
%uxmclaims_claim_name%          →  Iron Hollow
%uxmclaims_claim_owner_name%    →  Steve
%uxmclaims_claim_expireDate%    →  2026-11-14 18:02:11
```

The claim is the first of these that exists:

1. The claim the player selected with `/claim select`
2. The claim the player stands in
3. The player's only claim, when they own exactly one

With none of them, the expansion answers from `placeholderDefaults` at the end of `messages.yml`, keyed
by the full name without the percent signs:

```yaml
placeholderDefaults:
  uxmclaims_claim_name: 'Wilderness'
  uxmclaims_claim_owner_name: 'None'
```

A placeholder with no entry there stays in the text as you typed it. Add an entry for anything you
put on a permanent scoreboard. A request without an online player returns an empty string.

## The path rules

| Rule | Example |
|---|---|
| `.` and `_` are the same separator | `claim_owner_name` and `claim.owner.name` |
| Each step calls `getStep()`, then `step()`, then reads the field `step` | `claim_spawnLocation_x` calls `getSpawnLocation().getX()` |
| A member, invitation or ban falls through to that player | `claim_owner_name` reads the owner's player name |
| A step that resolves to nothing renders `null` | `claim_nmae` renders `null` |

Because the path is resolved against the [domain model](../developer/models.md), any getter is
reachable, not only the ones below.

## Placeholders

| Placeholder | Renders |
|---|---|
| `%uxmclaims_is_claimable%` | `true` when no claim covers the player's position, else `false` |
| `%uxmclaims_claim_name%` | The claim's name |
| `%uxmclaims_claim_id%` | Its UUID |
| `%uxmclaims_claim_ownerUid%` | The owner's UUID |
| `%uxmclaims_claim_owner_name%` | The owner's name |
| `%uxmclaims_count:claim_chunks%` | How many chunks it holds |
| `%uxmclaims_count:claim_members%` | How many members, the owner included |
| `%uxmclaims_count:claim_warps%` | How many warps |
| `%uxmclaims_count:claim_roles%` | How many roles |
| `%uxmclaims_count:claim_bans%` | How many bans |
| `%uxmclaims_count:claim_invites%` | How many pending invitations |
| `%uxmclaims_count:claim_flags%` | How many flags are on |
| `%uxmclaims_count:claim_vault_items%` | How many items are in the vault |
| `%uxmclaims_time:claim_remainTime%` | Time left before expiry, in `timeFormat` |
| `%uxmclaims_date:claim_expireDate%` | The expiry date, in `dateFormat` |
| `%uxmclaims_date:claim_creationDate%` | When it was created, in `dateFormat` |
| `%uxmclaims_claim_spawnLocation_world%` | The world its spawn is in |
| `%uxmclaims_claim_spawnLocation_x%` | Spawn x, likewise `_y` and `_z` |
| `%uxmclaims_claim_block_keyId%` | The claim block's style key, such as `block1` |
| `%uxmclaims_status:claim_block_placed%` | Whether the claim block is placed |
| `%uxmclaims_status:claim_isExpired%` | Whether the claim has expired |

`%uxmclaims_is_claimable%` is answered before the claim is resolved, so it works anywhere.

## Values without a modifier

| Value | Renders as |
|---|---|
| Text, whole number, UUID, enum | As is: `Iron Hollow`, `7`, `PVP` |
| Decimal | Two places: `128.50` |
| Boolean | `Yes` or `No` |
| Instant (`expireDate`, `creationDate`) | `yyyy-MM-dd HH:mm:ss` |
| Duration (`remainTime`) | Java's own form, `PT295H41M2S`: use `time:` |
| Location | `world:x:y:z`, with two decimals |
| List, set or map | Its raw contents: use `count:` |

## Modifiers

A modifier goes before a colon and transforms the value.

| Modifier | Takes | Gives |
|---|---|---|
| `count:` | A list, set or map | Its size |
| `time:` | A duration, or a number of milliseconds | `timeFormat` from `messages.yml` |
| `date:` | An instant, a date, or a millisecond timestamp | `dateFormat` from `messages.yml` |
| `status:` | A boolean, or an integer (`0` is off) | `statusActive` or `statusPassive` |
| `float:` | A number | Two decimal places |
| `upper:` | Anything | Upper case |
| `lower:` | Anything | Lower case |

A modifier that does not fit the value, or an unknown one, renders the value as plain text.

```
%uxmclaims_count:claim_members%           →  7
%uxmclaims_time:claim_remainTime%         →  12 days 06 hours 41 minutes 02 seconds
%uxmclaims_status:claim_block_placed%     →  Enabled
```

The formats sit at the top of `messages.yml`:

| Key | Default | Notes |
|---|---|---|
| `timeFormat` | `dd days HH hours mm minutes ss seconds` | `dd`, `HH`, `mm` and `ss` are replaced; days are not capped |
| `dateFormat` | `dd-MM-yyyy HH:mm:ss` | A Java `DateTimeFormatter` pattern, in the server's time zone |
| `statusActive` | `<green>Enabled</green>` | What `status:` renders for on |
| `statusPassive` | `<red>Disabled</red>` | What `status:` renders for off |

## Tokens inside plugin files

Inside uxmClaims' own files the path starts at an object the plugin puts in context. Which objects
exist depends on where the text is shown.

| Token root | Is | Available in |
|---|---|---|
| `claim` | The claim: every path above, without `uxmclaims_` | Messages about a claim, claim menus, holograms, map tooltips, webhooks |
| `player` | The viewer: `%player.name%` | Every menu, claim chat, `claimSettings.defaultName`, messages that name a player |
| `member` | A member: `name`, `texture`, `uid`, `joinDate`, `roleId` | Member menus and messages |
| `role` | A role: `name`, `priority`, `type`, `permissions` | Role and member menus, role messages |
| `warp` | A warp: `name`, `location`, `isPublic`, `createdAt`, `createdBy` (a UUID) | Warp menus and messages |
| `creator` | The warp creator's name, or their UUID when the player is unknown: `%creator%` | The public warp list |
| `invite` | An invitation: `name`, `texture`, `invitedAt` | Invitation menus |
| `ban` | A ban: `name`, `texture`, `reason`, `bannedAt` | The ban list |
| `chunk` | A chunk: `world`, `x`, `z`, `center.x`, `center.z`, and `centerX`, `centerZ` (the middle block, whole numbers) | The chunk map |
| `target` | The inspected player: `%target.name%` | `/claim admin view` |
| `event` | The event that fired | [webhooks.yml](../config/webhooks-yml.md) |

Some texts carry plain values instead of objects:

| Token | Is | Available in |
|---|---|---|
| `%flag%`, `%permission%`, `%status%` | The flag or permission, and its new state | Flag and permission messages; `%status%` also in the flag and permission menus |
| `%cost%` | A price; `%float:cost%` for two decimals | Main menu, chunk map, time menu |
| `%costPerDay%`, `%totalMillis%` | One day's price, the chosen extension | The time menu |
| `%ownedClaimAmount%`, `%memberedClaimAmount%` | Claims you own, claims you belong to | The general main menu (`common_main.yml`) |
| `%maximum%` | The viewer's member limit | The invitation list |
| `%permissionCount%` | How many permissions the role has | The role list |
| `%seconds%` | The teleport warmup | The teleport countdown message |
| `%message%` | The chat line | `claimChatFormat` |
| `%time%` | Seconds left to type | Chat text input titles |

`%member.texture%`, `%invite.texture%` and `%ban.texture%` resolve to the player's skin, which is how
menu heads are drawn.

<Callout type="warning" title="Other plugins' placeholders do not work in uxmClaims' files">

uxmClaims does not pass its own messages, menus or holograms through PlaceholderAPI. A token such as
`%vault_eco_balance%` in `messages.yml` renders `null`.

</Callout>

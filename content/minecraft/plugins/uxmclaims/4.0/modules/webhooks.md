---
title: Webhooks
order: 1342
description: Discord embeds for land, claim block, expiry, town and nation events, sent through channel webhooks.
icon: webhook
---

The webhooks module posts a Discord embed when something happens to a land, a town or a nation: a land is
created or deleted, a chunk is claimed, a member joins, a town gets a new mayor. It reports a change only after
it is saved, and sends the requests off the main thread. The 3.x `embeds.yml` and `senders.yml` keep working.

Module `webhooks` · enabled by default · `modules/webhooks/config.yml`

## Settings

`config.yml`:

| Key | Default | What it does |
|---|---|---|
| `enabled` | `true` | Send nothing while `false`. |
| `timeout-seconds` | `5` | Connect and read timeout of one request. |
| `date-format` | `dd-MM-yyyy HH:mm:ss` | Date pattern for `%date:...%` in embeds (3.x `dateFormat`). |
| `events.<key>` | `true` | Per-event switch by embed key. An event that is off sends nothing even with an embed; an event missing here is on. |

`senders.yml` holds the endpoints, by name (`playerWebhook` and `adminWebhook` ship):

| Key | Default | What it does |
|---|---|---|
| `senders.<name>.enabled` | `true` | `false` keeps the sender but sends nothing. |
| `senders.<name>.username` | `uxmClaims` | Name shown in Discord. Empty uses the webhook's own. |
| `senders.<name>.avatar-url` | `""` | Avatar image URL. Empty uses the webhook's own. |
| `senders.<name>.url` | `""` | The webhook URL, from Discord: Edit Channel, Integrations, Webhooks, New Webhook. uxmClaims never logs it. |

`embeds.yml` holds one embed per event key:

| Key | What it does |
|---|---|
| `embeds.<key>.senders` | Sender names from `senders.yml`. The embed goes to each of them. |
| `embeds.<key>.title` | Bold text at the top. |
| `embeds.<key>.description` | Lines of the body. |
| `embeds.<key>.color` | Hex colour of the side bar, such as `'#85e048'`. |
| `embeds.<key>.thumbnail`, `embeds.<key>.image` | Small image on the right, large image at the bottom. |
| `embeds.<key>.footer.text`, `embeds.<key>.footer.icon` | Footer text and icon. The icon shows only next to text. |
| `embeds.<key>.author.name`, `embeds.<key>.author.avatar-url` | Name and avatar above the title. |

Fields that are empty, and image fields that are not an `http(s)` URL, are left out of the message.

## Events

Keys are the 3.x event class names; the 4.x name in brackets works too and wins when both have an embed.

| Key | When | Embed shipped |
|---|---|---|
| `ClaimCreateEvent` (`LandCreateEvent`) | A land was created. | yes |
| `ClaimDeleteEvent` (`LandDeleteEvent`) | A land was deleted. | yes |
| `ClaimExpireEvent` (`LandExpireEvent`) | A land expired and was deleted. | yes |
| `ClaimRenameEvent` (`LandRenameEvent`) | A land was renamed. | yes |
| `ClaimChunkExtendEvent` (`LandChunkClaimEvent`) | A chunk was claimed. | yes |
| `ClaimChunkShrinkEvent` (`LandChunkUnclaimEvent`) | A chunk was unclaimed. | yes |
| `ClaimInviteAcceptEvent` (`LandMemberAddEvent`) | A member joined. | yes |
| `ClaimMemberLeaveEvent` (`LandMemberLeaveEvent`) | A member left. | yes |
| `ClaimMemberKickEvent` (`LandMemberKickEvent`) | A member was removed. | yes |
| `ClaimMemberRoleAssignEvent` (`LandMemberRoleChangeEvent`) | A member got another role. | no |
| `ClaimOwnerTransferEvent` (`LandOwnerTransferEvent`) | The land changed owner. | yes |
| `ClaimFlagsUpdateEvent` (`LandFlagChangeEvent`) | A flag changed. | no |
| `ClaimRolePermissionUpdateEvent` (`LandRolePermissionChangeEvent`) | A role permission changed. | no |
| `ClaimRelocateEvent` (`LandSpawnChangeEvent`) | The land spawn moved. | no |
| `ClaimRescheduleEvent` (`LandRescheduleEvent`) | A land's expiry date changed. | yes |
| `ClaimBlockPlaceEvent` | A claim block was placed. | yes |
| `ClaimBlockDestroyEvent` | A claim block was removed. | yes |
| `TownCreateEvent`, `TownDeleteEvent`, `TownRenameEvent` | A town was founded, deleted or renamed. | yes |
| `TownJoinEvent`, `TownLeaveEvent`, `TownKickEvent` | A resident joined, left or was kicked. | yes |
| `TownMayorChangeEvent` | A town got a new mayor. | yes |
| `NationCreateEvent`, `NationDeleteEvent`, `NationRenameEvent` | A nation was founded, deleted or renamed. | yes |
| `NationJoinEvent`, `NationLeaveEvent` | A member joined or left a nation. | yes |
| `NationKickEvent` | A member was removed from a nation. Without this embed, `NationLeaveEvent` is sent. | no |
| `NationCapitalChangeEvent` | A nation got a new capital. | yes |

An event without an embed sends nothing, so add one to `embeds.yml` for the rows marked "no".

## Embed placeholders

Case and `.` versus `_` do not matter; an unknown placeholder stays as written. Modifiers: `%date:...%`,
`%upper:...%`, `%lower:...%`.

| Placeholder | Renders |
|---|---|
| `%claim.name%`, `%claim.id%`, `%claim.ownerUid%`, `%claim.owner.name%` | The land's name, id, owner id and owner name. |
| `%claim.world%`, `%claim.chunks%`, `%claim.members%` | World, chunk count, member count. |
| `%claim.spawnLocation.world%`, `.x`, `.y`, `.z` | The land spawn. |
| `%date:claim.creationDate%` | The creation date. |
| `%land.*%` | The same under `land`, plus `%land.owner.type%` and `%land.kind%`. |
| `%event.actor.name%` | Who did it: a player, `Console`, or the reason of an automatic change. |
| `%event.oldName%`, `%event.newName%` | Renames of lands, towns and nations. |
| `%event.chunk.chunk.world%`, `.x`, `.z` | The claimed or unclaimed chunk. Also `%event.claimChunk.chunk.*%` and `%event.chunk.x%`. |
| `%event.member.uid%`, `%event.member.name%`, `%event.role.name%` | Member events. |
| `%event.newOwner.name%`, `%event.newOwner.uid%` | Transfer. |
| `%event.flag%`, `%event.value%` | Flag changes. |
| `%event.role.name%`, `%event.permission%`, `%event.value%` | Role permission changes. |
| `%event.reason%` | Delete reason: `owner`, `admin`, `expired`, `empty` or `plugin`. |
| `%date:event.expireDate%`, `%date:event.oldExpireDate%` | Reschedule. |
| `%event.claimBlock.location.world%`, `.x`, `.y`, `.z`, `%event.block.type%` | Claim block events. |
| `%town.name%`, `%town.id%`, `%town.tag%`, `%town.mayor.name%`, `%town.residents%` | Town events. |
| `%event.player.name%` | Town joins, leaves and kicks. |
| `%event.oldMayor.name%`, `%event.newMayor.name%` | Mayor change. |
| `%nation.name%`, `%nation.id%`, `%nation.tag%`, `%nation.capital.name%`, `%nation.members%` | Nation events. |
| `%event.member.name%`, `%event.member.type%`, `%event.kicked%` | Nation joins and leaves. |
| `%event.oldCapital.name%`, `%event.newCapital.name%` | Capital change. |

## Notes

- Both shipped senders have an empty `url`, so nothing is sent until you paste a webhook URL.
- An embed that names a sender `senders.yml` does not define is skipped, with one warning in the log.
- A request Discord refuses is logged with its HTTP status and not sent again.
- Switching off `NationLeaveEvent` in `events` also silences kicks that fall back to it.

Related: [Discord](discord.md), [Expiry](expiry.md), [Claim Block](claim-block.md), [Towns](towns.md), [Nations](nations.md)

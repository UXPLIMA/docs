---
title: inactivity.conf
order: 626
description: Archiving and deleting islands whose team stopped playing.
icon: hourglass
---

The file is `plugins/uxmSkyblock/modules/inactivity.conf`. Every default below is the shipped value.

## `inactivity`

Islands whose people stopped playing. Once each interval the plugin looks at every island: one whose owner has been away too long passes to the next member, and one whose members have all been away too long is archived, or deleted with its space returned to the grid. /is admin inactivity scan looks at once.

| Key | Default | What it does |
|---|---|---|
| `enabled` | `true` | Whether islands are looked at at all. |
| `check-interval-seconds` | `86400` | How often, in seconds: 86400 is once a day. |
| `owner-inactivity-duration` | `30d` | How long the owner may be away before the island passes to another member. |
| `all-members-inactivity-duration` | `60d` | How long every member may be away before abandonment-action below happens. |
| `succession-hierarchy` | `CO_OWNER`, `MODERATOR`, `MEMBER` | Which roles the island may pass to, the first first: of the members still playing, the longest serving one of the first role that has any becomes the owner. With none, the island stays as it is. |
| `former-owner-action` | `DEMOTE_TO_CO_OWNER` | What becomes of the owner it passed from: DEMOTE_TO_CO_OWNER, DEMOTE_TO_MEMBER or KICK_FROM_ISLAND. |
| `abandonment-action` | `ARCHIVE` | What happens to an island nobody plays: ARCHIVE keeps it, DELETE_AND_RECYCLE deletes it and gives its space back to the grid. |

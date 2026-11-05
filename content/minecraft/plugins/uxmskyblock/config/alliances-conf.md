---
title: alliances.conf
order: 611
description: "Allied islands: visits, chat and how many allies an island keeps."
icon: handshake
---

Alliances: islands that ally visit each other freely and share a chat.

The file is `plugins/uxmSkyblock/modules/alliances.conf`. Every default below is the shipped value.

## `alliances`

| Key | Default | What it does |
|---|---|---|
| `enabled` | `true` | false turns alliances off. |
| `max-allies` | `2` | How many allies an island may have. |
| `invite-timeout` | `5m` | How long an offer of alliance waits for an answer. |
| `expired-invite-sweep-interval` | `5m` | How often the invitations nobody answered are deleted. An expired invite cannot be accepted, so this only keeps the table from holding every invite the server has ever seen. |
| `friendly-fire-shielding` | `true` | true: members of allied islands cannot hurt each other. |
| `privileged-visit-access` | `true` | true: allies may visit an island that is locked to visitors. |
| `alliance-chat-enabled` | `true` | true opens /is allychat (or /is ac) to the allies. |

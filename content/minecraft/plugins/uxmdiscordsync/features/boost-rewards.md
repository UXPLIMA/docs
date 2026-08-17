---
title: Boost rewards
order: 35
description: Rewarding a Discord boost with a command in game.
icon: rocket
---

A player who boosts the Discord guild claims a reward in Minecraft with `/boostprize`.

```yaml
enabled: true
cooldown: 86400
cooldown-mode: "per-server"

rewards:
  - "give %player% diamond 10"
  - "give %player% emerald 5"
  - "eco give %player% 1000"
  - "tellraw %player% {\"text\":\"Thank you for boosting!\",\"color\":\"gold\"}"

send-discord-message: true
notification-channel-id: ""
```

| Key | Meaning |
|---|---|
| `cooldown` | Seconds between claims — 86400 is a daily reward |
| `cooldown-mode` | `per-server` gives each server its own cooldown; `global` shares one |
| `rewards` | Console commands, with `%player%`, `%player_uuid%`, `%discord_id%`, `%discord_user%`, `%server_name%` |
| `send-discord-message` | Announce the claim in Discord |
| `notification-channel-id` | Where to announce it; empty uses the audit webhook only |

The claim requires a linked account and an active boost. A player who stops boosting stops being
able to claim, but keeps what they already took.

## Recurring or one-off

The `cooldown` decides which kind of reward this is:

- **86400** — a daily perk. The player comes back every day, which is the point.
- **A very large number** — effectively one-off. Better done with a permission or rank grant than
  with items.

On a network, `cooldown-mode: per-server` means a daily reward is claimable once per day on each
server. If the reward is money and your servers share an economy, that is a multiplier you probably
did not intend — use `global` there.

## Audit trail

`logging.yml → audit.boost-claim` posts an embed each time a reward is claimed, with the player, the
Discord user and the server. Leave it on: boost rewards are the part of this plugin most likely to
be disputed, and the log is the answer.

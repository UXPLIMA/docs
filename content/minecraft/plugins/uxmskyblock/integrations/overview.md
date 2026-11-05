---
title: Overview
order: 801
description: Every plugin uxmSkyblock hooks into and what each turns on.
icon: plug
---

Every integration is optional. A plugin that is not installed turns its part off, and the console says so once.

| Plugin | What it turns on | Set in |
|---|---|---|
| Vault, VaultUnlocked | The island's money: the bank, upgrades, the shop, upkeep, the visit fee. A second balance as a bank currency | [bank.conf](../config/bank-conf.md) |
| Treasury | A bank currency | [bank.conf](../config/bank-conf.md) |
| PlayerPoints | A bank currency, `points` as shipped | [bank.conf](../config/bank-conf.md) |
| EcoBits | A bank currency | [bank.conf](../config/bank-conf.md) |
| PlaceholderAPI | The [placeholders](../placeholders/reference.md) | |
| floodgate, Geyser | Forms for Bedrock players where a form reads better than a chest window | |
| dynmap, BlueMap, Pl3xMap | Islands drawn on the web map | [webmap.conf](../config/webmap-conf.md) |
| ItemsAdder, Nexo, CraftEngine | Their blocks in OneBlock phases, written `itemsadder:<namespace>:<id>`, `nexo:<id>`, `craftengine:<namespace>:<id>` | [OneBlock phases](../config/oneblock-phases.md) |
| MythicMobs | Its creatures in OneBlock phases, written `mythicmobs:<id>` | [OneBlock phases](../config/oneblock-phases.md) |
| mcMMO | Skill levels a requirement may ask for | |

## Outside services

| Service | What it does | Set in |
|---|---|---|
| Discord webhooks | Island milestones, leaderboard changes, alliances and staff actions | [discord.conf](../config/discord-conf.md) |
| Redis | Servers sharing islands hear from each other | [config.conf](../config/config-conf.md) |
| S3 | Island backups kept in a bucket | [config.conf](../config/config-conf.md) |

---
title: Overview
order: 801
description: Every plugin and service uxmAuction reaches, and troubleshooting.
icon: plug
---

| With | Adds |
|---|---|
| Vault, VaultUnlocked | `vault` and `vaultunlocked` currencies |
| PlayerPoints, EcoBits, Treasury | Their currencies |
| Any economy plugin | Through the `placeholder` or `bridge` type |
| PlaceholderAPI | The placeholders, and the `placeholder` currency |
| Discord | The webhook |
| Redis | The `redis` network transport |
| Floodgate, Geyser | Windows as Bedrock forms |
| mcMMO | `%mcmmo_level_<skill>%` in effect lines |
| uxmLang | One language choice for every UXM plugin |
| Other plugins | The Bukkit events of `events.enabled` |

## Troubleshooting

| Symptom | Cause |
|---|---|
| A listing is refused because the currency is not ready | Its plugin is not installed or not loaded; the console names it at start |
| A price with a fraction is refused | The currency counts whole numbers (`item`, or a whole plugin) |
| Bids go up by one coin | Set `house.bidding.step.<currency>` |
| Nothing moves at all | The house is locked; `/uxmauction lock off` |
| A window on one server shows a listing already bought on another | `network.transport` is `none` on a shared database |

Related: [Installation](../getting-started/installation.md)

---
title: Overview
order: 801
description: PlaceholderAPI, WorldEdit and FastAsyncWorldEdit, the economies, Floodgate and Geyser.
icon: plug
---

Nothing is required. Each is found while the plugin starts, so each loads before it.

| Plugin | Gives |
|---|---|
| PlaceholderAPI | The [placeholders](../placeholders/reference.md), and other plugins' placeholders in conditions, such as `%mcmmo_level_mining%` |
| WorldEdit | Edits held inside the plot a player may build on (`guard-edits`) |
| FastAsyncWorldEdit | The same, through its own region restriction, `fawe.uxmplots` |
| Vault or VaultUnlocked | The `vault` currency type |
| Treasury | The `treasury` currency type |
| Floodgate | Windows as native Bedrock forms |
| Geyser | Knowing a player is on Bedrock |
| lettuce-core | The `redis` network transport; without it the plugin runs on its own |

Without WorldEdit, a server has no editor to guard and nothing is lost. A Vault currency with no economy answers that it
is not ready, and nothing is priced in it.

`/uxmplots doctor` names each one it found.

Related: [Installation](../getting-started/installation.md)

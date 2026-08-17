---
title: config.yml
order: 51
description: Hooks, worlds, cooldowns, sounds, the log cleaner and debug.
icon: file-cog
---

## Top level

```yaml
config-version: "..."   # written by the plugin, do not edit
language: en            # en or tr
license-key: ""
```

`language` picks the folder under `languages/`. Adding a third language means copying `en/` to a new
folder, translating it, and naming it here.

## Hooks

```yaml
hooks:
  region-hook: "uxmClaims"
  money-hook: "Vault"
  price-source-hook: default
  interaction-hook: "FancyNpcs"
```

Each is covered in detail on [Integrations](../integrations.md). The short version:

| Hook | Purpose | Empty value means |
|---|---|---|
| `region-hook` | Which plugin owns the land a farmer sits on | Farmers have nowhere to live |
| `money-hook` | Where money comes from and goes | No economy |
| `price-source-hook` | Where product prices come from | `collected-materials.yml` prices |
| `interaction-hook` | What draws the NPC and receives clicks | Farmer is command-only |

## Worlds

```yaml
world-management:
  world-list:
    - "world"
  list-type: BLACKLIST
```

`BLACKLIST` forbids farmers in the listed worlds; `WHITELIST` allows them only there. Every player
command checks this before doing anything, so a farmer that somehow ends up in a forbidden world
becomes inert rather than broken.

## Cooldowns

All in seconds.

| Key | Default | Applies to |
|---|---|---|
| `move-cooldown` | 180 | Moving the farmer |
| `take-all-cooldown` | 120 | Taking everything out of storage |
| `sell-all-cooldown` | 120 | Selling everything |
| `sell-xp-cooldown` | 120 | Selling XP |
| `change-glowing-cooldown` | 120 | Changing the glow |
| `change-entity-cooldown` | 120 | Changing the entity type |
| `change-profession-cooldown` | 120 | Changing the villager profession |
| `return-cooldown` | 120 | `/farmer return` |

The appearance cooldowns exist because each change re-spawns the NPC for everyone nearby. Lowering
them below about 30 seconds lets a player flicker their farmer at anyone standing next to it.

## Sounds

```yaml
sounds:
  on-cooldown: BLOCK_ANVIL_LAND
  try-again: BLOCK_NOTE_BLOCK_PLING
  already-using-that-glow-color: UI_BUTTON_CLICK
  blacklisted-world: BLOCK_ANVIL_BREAK
```

Any Bukkit sound name. These are the feedback sounds for refusals — leave them distinct from each
other so a player can tell what went wrong without reading.

## Log cleaner

```yaml
log-cleaner:
  enabled: true
  clean-old-logs-after: 50
  logs-to-clean: 25
```

When a farmer reaches 50 log entries, the oldest 25 are deleted.

<Callout type="warning" title="Enabling the cleaner on an old database">

On a server that has been running without it, the first cleanup pass has a large backlog to work
through and can cause a stall. Disable the cleaner, trim the log table directly, then enable it.

</Callout>

## Debug

```yaml
debug: false
debug-categories:
  - NONE
ignore: []
farmer-ids: []
```

Categories are `NONE`, `ALL`, `AUTO_KILL`, `AUTO_SELL`, `AUTO_HARVEST`, `XP_COLLECTOR`,
`HANDLE_ITEM`, `GENERAL`. `farmer-ids` narrows debug output to specific farmers and `ignore`
excludes categories from `ALL`.

Leave `debug: false` unless support asks for it. `ALL` on a busy server writes more to the console
than the console can keep up with.

---
title: config.yml
order: 401
description: The license, the language, performance, preview, economy, protection and every other switch.
icon: file-cog
---

The main file. A key missing from your copy keeps its default, and keys added in a later version are
written into your copy when the plugin updates it. Numbers outside a safe range are clamped rather than
refused.

```yaml
general:          # license key, language, metrics, update check, debug
performance:      # the shared budget, adaptive slowdown, chunk tickets
schematic:        # size cap, cache, build order, unknown blocks, fluids, blacklist
npc:              # builder NPCs
builders:         # the default builder
preview:          # the in-client preview
economy:          # prices, refunds, number format
protection:       # providers and land checks
materials:        # the warehouse
restrictions:     # worlds, heights, spawn distance
sitePreparation:  # clearing and foundations
trust:            # trusted players
boost:            # speed boosts
history:          # build history
start:            # blueprint item, placement, cooldown, cancel confirmation
progress:         # boss bar, action bar, milestones
effects:          # sounds, particles, completion firework
admin:            # seeds for new packages
gui:              # menu timing
database:         # SQLite, MySQL or PostgreSQL, checkpoints, recovery, server id
api:              # developer events
logging:          # the audit trail
```

The Discord webhook is no longer here: it has its own file, [webhooks.yml](webhooks-yml.md).

## general

| Key | Default | Meaning |
|---|---|---|
| `licenseKey` | `LICENSE_KEY_HERE` | Your key; the plugin does not start without a valid one |
| `language` | `en` | `en`, `tr`, `es`, `pt-BR`, `de`, `fr`, `ru` or `pl`; see [Languages](../features/languages.md) |
| `metrics` | `true` | Anonymous bStats statistics; no player data is sent |
| `updateCheck` | `true` | Ask uxplima.com for a newer release; the console and staff with `uxmbuilders.admin` are told when one is out |
| `debug` | `false` | Write every build event and refused command to the console; `/builders admin debug` switches it until the next restart |

The update notice reaches staff as they join, with the version and a download link, and says so when a
release includes a security fix. Its texts are the `update*` keys in `messages.yml`.

## performance

| Key | Default | Meaning |
|---|---|---|
| `blocksPerTick` | `20` | Server-wide ceiling on blocks placed per tick |
| `maxNanosPerTick` | `2000000` | Time placing may take per tick |
| `maxConcurrentJobs` | `5` | Jobs building at once |
| `queueLimit` | `20` | Jobs allowed to wait |
| `adaptive.*` | on, 19.0, 0.5, 17.0, 100 | See [Building and performance](../features/building.md) |
| `chunk.keepLoaded` | `true` | Keep the chunks being built in loaded |
| `chunk.maxTickets` | `64` | Chunk tickets per job |

## schematic

| Key | Default | Meaning |
|---|---|---|
| `maxBlocks` | `200000` | Largest schematic volume accepted |
| `cacheSize` | `20` | Parsed schematics kept in memory |
| `buildOrder` | `GREEDY` | `GREEDY`, `LAYERED`, `SPIRAL`, `NEAREST` or `RANDOM` |
| `twoPass` | `true` | Place blocks that need support last |
| `unknownBlock` | `SKIP` | `SKIP`, `REPLACE` or `ABORT` |
| `unknownReplacement` | `minecraft:stone` | Used by `REPLACE` |
| `keepSignText` | `true` | Copy sign text from the schematic |
| `keepSkullTextures` | `true` | Copy the skin of player heads from the schematic |
| `allowFluids` | `false` | Place water and lava |
| `blacklist` | spawners, command blocks, bedrock... | Blocks never placed |

## npc and builders

| Key | Default | Meaning |
|---|---|---|
| `npc.enabled` | `true` | Show builder NPCs |
| `npc.nameFormat` | yellow bold `%builder.name%` | The nameplate |
| `npc.hoverOffset` | `1.2` | Height above the block being worked on |
| `npc.viewDistance` | `48` | Who receives the NPC |
| `npc.trackerInterval` | `20` | Ticks between viewer refreshes |
| `npc.swingInterval` | `6` | Ticks between arm swings |
| `npc.arrivalEpsilon` | `0.6` | How close counts as arrived |
| `npc.teleportThreshold` | `24` | Farther than this, teleport instead of walking |
| `npc.hideFromTablist` | `true` | Keep NPCs out of the tab list |
| `builders.defaultBuilder` | `apprentice` | The builder new players start with; it must exist in `builders.yml` |
| `builders.allowSwitching` | `true` | Allow selecting another owned builder |

## economy

| Key | Default | Meaning |
|---|---|---|
| `enabled` | `true` | Off makes everything free |
| `currencyFormat` | `#,##0` | Number pattern |
| `currencySymbol` | ` $` | Appended to every amount |
| `locale` | `en-US` | Thousands and decimal marks, for example `tr-TR` or `de-DE`; an unknown tag falls back to `en-US` |
| `pricingModel` | `FLAT` | `FLAT`, `PER_BLOCK` or `HYBRID` |
| `pricePerBlock` | `2.5` | Used by `PER_BLOCK` and `HYBRID` |
| `refund.onCancel` | `PARTIAL` | `FULL`, `PARTIAL` or `NONE`; see [Restarts and refunds](../features/recovery.md) |
| `refund.maxPercent` | `80` | Cap on a partial refund |

`PER_BLOCK` charges `pricePerBlock` for every non-air block; `HYBRID` adds that to the package price.

## The feature sections

| Section | Page |
|---|---|
| `preview` | [Preview](../features/preview.md) |
| `protection` | [Land protection](../features/protection.md) |
| `materials` | [Warehouse](../features/warehouse.md) |
| `restrictions` | [Placement rules](../features/placement-rules.md) |
| `sitePreparation` | [Site preparation](../features/site-preparation.md) |
| `trust` | [Trusted players](../features/trusted-players.md) |
| `boost` | [Speed boost](../features/boost.md) |
| `history` | [Build history](../features/history.md) |
| `database` | [The database](database.md) |

## start

| Key | Default | Meaning |
|---|---|---|
| `item.material` | `PAPER` | The blueprint item |
| `item.customModelData` | `1001` | For a resource pack model |
| `item.glow` | `true` | Enchantment shimmer |
| `origin.yMode` | `ABOVE` | Build `ABOVE`, `AT` or `BELOW` the block looked at |
| `cooldownSeconds` | `30` | Between any two starts of a player; a package's own `cooldown` applies on top, between two starts of that package |
| `confirmCancel` | `true` | `/builders cancel` asks for `/builders confirm` |
| `confirmTimeout` | `10` | Seconds to confirm |

The blueprint's name and lore are messages, `blueprintName` and `blueprintLore`, so they follow the
language.

## progress, effects, admin, gui, api, logging

| Key | Default | Meaning |
|---|---|---|
| `progress.bossbar.enabled` | `true` | The boss bar; a reload applies to running builds |
| `progress.bossbar.colorByState` | `true` | Green building, blue queued, yellow paused by hand, red paused for a reason |
| `progress.bossbar.onlyNearby`, `radius` | `true`, `64` | Who sees it: players within the radius, measured across the ground, or the owner from anywhere when off |
| `progress.actionbar.enabled` | `true` | The action bar summary, for players within `radius` of a site |
| `progress.chatMilestones` | `[25, 50, 75, 100]` | Percentages announced in chat |
| `progress.eta` | `true` | Show the time left |
| `effects.placeSound.*` | on, every 5 blocks, 0.4 | The placing sound |
| `effects.placeParticle.*` | on, 4 | The placing particles |
| `effects.radius` | `32` | Who hears and sees them |
| `effects.completion.firework` | `true` | A firework on completion; it cannot hurt anyone |
| `admin.newPackage.display`, `icon` | `<white>%id%</white>`, `PAPER` | Seeds for `/builders admin package create` |
| `gui.confirmDelayTicks` | `40` | Before the purchase button unlocks |
| `api.fireBlockEvents` | `false` | Fire `BuildBlockPlaceEvent` for every block |
| `logging.audit` | `true` | Write purchases, starts, cancels, refunds, boosts, trust and admin changes to the audit table |

The menus' filler pane is no longer a setting here: it is the entry without a slot under `otherItems`
in each [menu file](messages-and-menus.md).

Related: [packages.yml](packages-yml.md), [builders.yml](builders-yml.md), [webhooks.yml](webhooks-yml.md)

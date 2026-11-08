---
title: market.yml
order: 407
description: The player market's money, listing rules, capture, purchases, reports, moderation and categories.
icon: shopping-bag
---

Everything about the [player market](../features/market/index.md) except its texts and menus. A key
missing from your copy keeps its default. Numbers out of range are clamped; a misspelt mode falls back
to its default and a negative amount to 0, with a warning in the console, so a typo never stops the
market.
`/builders admin reload` applies changes, except `cache.syncSeconds`.

```yaml
enabled: true
economy:      # commission, listing fee, price range, payouts
listing:      # titles, live listings, copies, duplicates
capture:      # size limits, land, what a capture leaves out
purchase:     # builds per purchase, copies per buyer, blueprints
reports:      # auto-hide and who may report
moderation:   # seller bans and revokes
cooldowns:    # waits between a player's actions
text:         # the word filter
cache:        # several servers on one database
webhook:      # not read; see below
categories:   # the market's categories
```

Block ids may leave out `minecraft:`, and a `*` in an id stands for any text: `minecraft:*shulker_box`
is every shulker box. Amounts are in the server's money.

## enabled and economy

| Key | Default | Meaning |
|---|---|---|
| `enabled` | `true` | Off: nothing can be captured, listed or bought; bought designs stay buildable |
| `economy.commissionPercent` | `10.0` | The market's share of each sale; `uxmbuilders.market.commission.<n>` sets it per player |
| `economy.commissionAccount` | empty | A player name or UUID credited with commissions and listing fees; empty destroys them. An unknown name is warned about and treated as empty |
| `economy.listingFee` | `500.0` | What listing a design costs; `uxmbuilders.market.fee.exempt` pays nothing |
| `economy.priceMin`, `priceMax` | `100.0`, `10000000.0` | The price range of a listing |
| `economy.payout` | `INSTANT` | `INSTANT`: sellers are paid right after a sale. `ON_JOIN`: when they join or claim |

## listing

| Key | Default | Meaning |
|---|---|---|
| `titleMin`, `titleMax` | `3`, `32` | Title length in characters, at most 64 |
| `descriptionMax` | `160` | Description length, at most 512; `0` leaves the description out |
| `maxLiveDefault` | `3` | Live listings for a player without `uxmbuilders.market.listings.<n>` |
| `limitedMaxCopies` | `1000` | The most copies a limited listing may sell |
| `allowUnlimited` | `true` | Allow listings with unlimited copies |
| `priceChangeCooldownMinutes` | `10` | Minutes between two price changes of one listing |
| `dedupe` | `GLOBAL` | `GLOBAL`: a design is on sale once on the whole network. `SELLER`: once per seller, so co-owners may each list it |
| `blockResaleOfPurchased` | `true` | A player cannot list a design they bought, in any rotation |
| `lockLimitedEditions` | `true` | A design that sold copies as a limited edition cannot be listed again by its seller |

## capture

| Key | Default | Meaning |
|---|---|---|
| `requireBuildPermission` | `true` | Ask the protection plugins whether the seller may build on the whole selection |
| `wilderness` | `DENY` | A selection on nobody's land: `DENY` refuses it, `ALLOW` accepts it. Applies only while a protection plugin is active |
| `maxWidth`, `maxLength`, `maxHeight` | `64` each | The largest selection |
| `maxVolume` | `131072` | The largest selection volume; `uxmbuilders.market.size.<n>` can lower it per player |
| `minBlocks` | `20` | The fewest blocks a design may have |
| `trimAir` | `true` | Shrink the selection to the blocks it holds |
| `blacklistPolicy` | `AIR` | Blocks never sold become air (`AIR`), or refuse the capture (`REJECT`) |
| `extraBlacklist` | empty | More blocks never sold, on top of the market's own list and `schematic.blacklist` |
| `stripValuables` | `true` | While `materials.enabled` is off, turn `valuableBlocks` into air |
| `valuableBlocks` | diamond, emerald, netherite, gold, iron blocks, ancient debris, beacon, conduit, every shulker box | Blocks too valuable to hand out for free |
| `allowFluids` | `false` | Keep water, lava and waterlogged blocks |
| `normalize` | empty | Blocks reset before they are sold, on top of the market's own table; see below |
| `worlds.mode`, `worlds.list` | `NONE`, empty | `NONE`: every world. `WHITELIST`: only the listed worlds. `BLACKLIST`: all but them |
| `maxSnapshotKilobytes` | `2048` | The largest stored design, 16 to 65536 |
| `timeoutSeconds` | `30` | How long reading a selection may take, 5 to 600 |

`normalize` maps a block to a state or to another block. An empty value turns one of the market's own
entries off:

```yaml
capture:
  normalize:
    minecraft:campfire: 'lit=false'
    minecraft:lava_cauldron: 'minecraft:cauldron'
    minecraft:composter: ''
```

What the market's own lists hold is on [Selling a build](../features/market/selling.md#what-the-capture-leaves-out).

## purchase

| Key | Default | Meaning |
|---|---|---|
| `buildsPerPurchase` | `1` | Builds one purchase allows; `-1` for any number |
| `maxPerBuyerLimited` | `1` | Copies of one limited listing a player may buy; `0` is no limit |
| `maxPerBuyerUnlimited` | `0` | The same for an unlimited listing |
| `creditReturnMaxProgress` | `10` | Cancelling a market build at or below this percent gives the build back |
| `blueprintBound` | `true` | Only the buyer may build from their blueprint |

## reports and moderation

| Key | Default | Meaning |
|---|---|---|
| `reports.autoHideThreshold` | `3` | Open reports that hide a listing until staff decide; `0` never hides |
| `reports.minPlaytimeMinutes` | `60` | Playtime a player needs before reporting |
| `reports.dailyLimit` | `5` | Reports one player may make in 24 hours; `0` is no limit |
| `reports.noteMax` | `100` | Longest report note kept, at most 255 |
| `moderation.onSellerBan` | `HIDE` | What a market ban does to the seller's listings: `HIDE` or `REMOVE` |
| `moderation.holdEarningsOfBanned` | `true` | Hold what the market owes a banned seller until the ban is lifted |
| `moderation.revokeRefund` | `FULL` | `FULL`: buyers get their money back when staff revoke a design. `NONE`: they do not |

## cooldowns

Seconds between two actions of one player; `0` for none. `uxmbuilders.market.bypass.cooldown` skips
them.

| Key | Default | Between two |
|---|---|---|
| `createSeconds` | `300` | Listings |
| `purchaseSeconds` | `3` | Purchases |
| `reportSeconds` | `60` | Reports |
| `reissueSeconds` | `60` | Blueprints issued again for the same design |

## text, cache and webhook

| Key | Default | Meaning |
|---|---|---|
| `text.blockedWords` | empty | Words refused in titles, descriptions and sign text. Case, accents and digits standing for letters (`b4d`) are ignored, and letters written apart (`b a d`) still match; a word only matches a whole word, so `class` is never refused for `ass` |
| `text.blockedPatterns` | empty | Regular expressions refused the same way; one that does not compile is skipped with a warning |
| `cache.syncSeconds` | `15` | How often other servers' market changes are read from a shared database, at least 5; read at startup |
| `webhook.events`, `webhook.moderationUrl` | all four, empty | Not read in this version: the market's posts are switched and routed by `events` and `moderationUrl` in [webhooks.yml](webhooks-yml.md) |

## categories

```yaml
categories:
  houses:
    display: '<green>Houses</green>'
    icon: OAK_DOOR
    order: 1
    permission: ''
```

| Key | Meaning |
|---|---|
| The id (`houses`) | What listings store: 1 to 32 letters, digits, `-` or `_` |
| `display` | The name menus show, in MiniMessage |
| `icon` | The material of its tab |
| `order` | Position in menus |
| `permission` | Needed to list in it, to see it and to buy from it; empty for everyone |

The shipped categories are `houses`, `castles`, `farms` and `decorations`. Their names follow
`general.language`; the [language switch](../features/languages.md) rewrites them like any other text.

Related: [Player market](../features/market/index.md), [webhooks.yml](webhooks-yml.md), [config.yml](config-yml.md)

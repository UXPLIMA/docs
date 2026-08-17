---
title: Integrations
order: 60
icon: plug
---

Four hooks in `config.yml`, and a handful of plugins uxmFarmer notices without being told.

## Region hook

Which plugin owns the land. A farmer belongs to a region, so this hook is not optional — without it
there is nowhere to put one.

```yaml
hooks:
  region-hook: "uxmClaims"
```

| Value | Notes |
|---|---|
| `uxmClaims` | The recommended option |
| `SuperiorSkyblock2` | |
| `IridiumSkyblock` | |
| `BentoBox:<world name>` | Written as `BentoBox:BSkyBlock` or `BentoBox:OneBlock` — the friendly world name from your addon's config |
| `FabledSkyBlock` | |
| `DeluxeSkyblock` | |
| `Lands` | |
| `RealmCore` | |
| `GriefPrevention` | |
| `GriefDefender` | Does not support `place-farmer-on-region-creation` |
| `ProtectionStones:<world>` | Supports automatic placement, but leaving it off is recommended |
| `RClaim` | |
| `Towny` | For TownyAdvanced |
| `UltimateClaims` | |
| `NClaim` | |
| `hClaims` | Treated as `uxmClaims` |

Island hooks are the only ones where `where-players-can-reach-farmer: EVERYWHERE` has any effect.
Every other hook behaves as `NEAR_FARMER`.

The region hook also drives the member rules in `general-settings.yml` — who is added to a farmer
when they join the region, who is removed when they leave, and where ownership goes if the owner
leaves.

## Economy hook

```yaml
hooks:
  money-hook: "Vault"
```

| Value | What it uses |
|---|---|
| `Vault` | Whatever economy plugin Vault is bridging |
| `PlayerPoints` | The points balance |
| `SuperiorSkyblock2` | The island bank |

The SuperiorSkyblock2 option is worth considering on an island server: money from the farm goes to
the island rather than to whoever happened to click, which matches how a shared farmer is actually
used.

## Price source hook

Where a product's sale price comes from.

```yaml
hooks:
  price-source-hook: default
```

| Value | Prices from |
|---|---|
| `default` or `""` | `unit-price` in `collected-materials.yml` |
| `ShopGUIPlus` | Live shop prices |
| `EconomyShopGUI` or `EconomyShopGUI-Premium` | Live shop prices |
| `ExcellentShop` | Live shop prices |

Using a shop plugin keeps one price list for the whole server — a player cannot make money by
selling through the farmer instead of the shop, or the other way round. The cost is that a product
missing from your shop has no price, so it never auto-sells and its storage fills quietly. If that
matters more than the single price list, use `default` and price everything here.

## Interaction hook

What draws the NPC and receives clicks.

```yaml
hooks:
  interaction-hook: "FancyNpcs"
```

| Value | What it is |
|---|---|
| `Citizens` | |
| `FancyNpcs` | |
| `ZNPCsPlus` | |
| `NpcApi` | Eisi05's NpcApi |
| `Holograms` | Internal — a hologram with no entity |
| `ModelSystem` | Internal — ModelEngine, BetterModel or ItemsAdder, configured under `model-settings` |
| `default` or `""` | No body at all; commands only |

Holograms above the farmer are drawn by the plugin itself in every case — the interaction hook is
only about the entity.

## Plugins it notices without configuration

| Plugin | What it adds |
|---|---|
| PlaceholderAPI | The [placeholder expansion](placeholders.md), and placeholder requirements on levels and conversions |
| LuckPerms | Permission lookups for offline owners, which is what makes owner-gated modules work while the owner is away |
| packetevents | Packet-level NPC and hologram handling |
| RoseStacker, WildStacker, SpawnerMeta | Stacked mobs and spawners are understood by Auto Kill |
| ModelEngine, BetterModel, ItemsAdder | Custom models for the farmer |

<Callout type="note" title="LuckPerms and offline owners">

Module permissions are checked against the farmer's owner, who is often offline while members use
the farm. LuckPerms is what lets that check resolve properly. Without it, an owner-gated module can
behave differently depending on whether the owner happens to be online.

</Callout>

## Folia

`folia-supported: true`. uxmFarmer runs on Folia without extra configuration.

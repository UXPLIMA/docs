---
title: Overview
order: 1480
description: Every plugin uxmEssentials detects, by family, and what each one turns on.
icon: book-open
---

uxmEssentials detects 51 other plugins. Every one is optional and none is switched on by hand.

Land claim plugins are the one family where several can run at once. All of them are consulted
and their answers combined; see [Land Claims](claims.md) for the combine modes.

## Permissions

| Plugin | What it turns on |
|---|---|
| LuckPerms | Group and meta lookups behind the quota and tier permission nodes. |

See [LuckPerms](luckperms.md) for the node reference.

## Economy

| Plugin | What it turns on |
|---|---|
| Vault | Runs the economy through an existing Vault provider, and answers permission queries. |
| Treasury | Runs the economy through an existing Treasury provider. |
| PlayerPoints | Points as a currency menus and requirements can charge. |
| CoinsEngine | CoinsEngine currencies menus and requirements can charge. |
| zEssentials | zEssentials currencies menus and requirements can charge. |
| EconomyShopGUI | Prices unpriced items for `/worth` and `/sell` from the shop's own sell prices. |
| EconomyShopGUI-Premium | The premium edition of the same shop, under its own plugin name. |

With none of these installed the built-in economy runs on its own database. See
[Vault & Treasury](vault-treasury.md), and [Economy](../modules/economy.md) for the
EconomyShopGUI price fallback.

## Placeholders

| Plugin | What it turns on |
|---|---|
| PlaceholderAPI | Expands third-party placeholders inside our text, and publishes the `uxmessentials` expansion for other plugins to read. |
| MiniPlaceholders | MiniMessage-native global tags inside our message catalog lines. |

See [PlaceholderAPI](placeholderapi.md) for the full placeholder list we publish.

## Custom items

| Plugin | What it turns on |
|---|---|
| HeadDatabase | Head-database heads as menu icons. |
| ItemsAdder | ItemsAdder custom items as menu icons. |
| Oraxen | Oraxen custom items as menu icons. |
| Nexo | Nexo custom items as menu icons. |
| CraftEngine | CraftEngine custom items as menu icons. |
| MMOItems | MMOItems items as menu icons. |
| ExecutableItems | ExecutableItems items as menu icons. |

See [Custom Items & Heads](custom-items.md) for the icon syntax.

## Land claims

Every installed one is consulted, not just the first.

| Plugin | What it turns on |
|---|---|
| Lands | Lands claims gate homes and warps. |
| GriefPrevention | GriefPrevention claims gate homes and warps. |
| GriefDefender | GriefDefender claims gate homes and warps. |
| ExcellentClaims | ExcellentClaims claims gate homes and warps. |
| SimpleClaimSystem | SimpleClaimSystem claims gate homes and warps. |
| RClaim | RClaim claims gate homes and warps. |
| XClaim | XClaim claims gate homes and warps. |
| Homestead | Homestead claims gate homes and warps. |
| Towny | Town plots gate homes and warps. |
| Kingdoms | KingdomsX land gates homes and warps. |
| HuskClaims | HuskClaims claims gate homes and warps. |
| HuskTowns | HuskTowns town claims gate homes and warps. |
| Factions | FactionsUUID or SaberFactions territory gates homes and warps. |
| BentoBox | BentoBox islands gate homes and warps. |
| Residence | Residences gate homes and warps. |
| PlotSquared | Plots gate homes and warps. |
| SuperiorSkyblock2 | SuperiorSkyblock islands gate homes and warps. |

uxmClaims, our own claim plugin, is detected too and needs no declaration.

## Regions

| Plugin | What it turns on |
|---|---|
| WorldGuard | Region membership and flags gate teleports, poses and menu requirements. |
| WorldEdit | Defines a region from your current WorldEdit selection. |

## Web maps

| Plugin | What it turns on |
|---|---|
| dynmap | Publishes spawn and warp markers to the dynmap web map. |
| squaremap | Publishes spawn and warp markers to the squaremap web map. |
| BlueMap | Publishes spawn and warp markers to the BlueMap web map. |

## Voting

| Plugin | What it turns on |
|---|---|
| Votifier | Feeds votes from vote sites into the vote module. NuVotifier works too. |

## Bedrock

| Plugin | What it turns on |
|---|---|
| Floodgate | Shows Bedrock players native forms instead of chest menus. |
| Geyser-Spigot | Names Bedrock players on networks running Geyser without Floodgate. |

See [Bedrock (Floodgate)](floodgate.md).

## Login

| Plugin | What it turns on |
|---|---|
| AuthMe | Holds our security prompts until AuthMe has authenticated the player. |
| nLogin | Holds our security prompts until nLogin has authenticated the player. |

Without one of these installed nothing waits: our own prompts open as soon as the player
joins. With one installed the player logs in first and only then sees ours, so two prompts
never fight over the same screen. See [Security](../modules/security.md).

## Vanish

| Plugin | What it turns on |
|---|---|
| SuperVanish | Players SuperVanish has hidden are vanished for our tab list, nametags and `/msg` too. |
| PremiumVanish | Players PremiumVanish has hidden are vanished for our tab list, nametags and `/msg` too. |

See [Vanish](../modules/vanish.md).

## Combat

| Plugin | What it turns on |
|---|---|
| CombatLogX | A CombatLogX combat tag blocks self-teleports out of a fight. |
| PvPManager | A PvPManager combat tag blocks self-teleports out of a fight. |

We keep no combat timer of our own, so with neither installed nothing is ever blocked. See
[Teleport & RTP](../modules/teleport.md).

## Client protocol

| Plugin | What it turns on |
|---|---|
| ViaVersion | Reports what protocol version a translated client actually speaks. |

## Menu conditions

| Plugin | What it turns on |
|---|---|
| Jobs | Job and level conditions in menu requirements. JobsReborn is the plugin. |
| mcMMO | Skill-level and power-level conditions in menu requirements. |

## Checking what bound

`/uxmess doctor` reports the integrations it found on its `soft-dependencies` line, grouped the
same way as this page:

```
[OK] soft-dependencies
     3/51 integrations present (economy: Vault; placeholders: PlaceholderAPI; land claims: Lands)
```

If a plugin you installed is missing from that line, it either failed its own startup (check
your console for its errors) or it is not one uxmEssentials integrates with. Present means
installed **and** enabled, since a plugin that crashed on startup is not one we can call into.

## Plugins that fight us instead

The same `/uxmess doctor` names any installed plugin that owns commands we also own, on its
`command-conflicts` line:

```
[WARN] command-conflicts
       commands may be shadowed by Essentials (homes, warps, economy and most player
       commands); whichever plugin registers a name first wins, so disable the
       overlapping module on one side
```

It watches for **Essentials**, **CMI** and **Multiverse-Core**. Two plugins both registering
`/home` is legal and the server resolves it by load order, which is exactly why it is worth
saying out loud: the usual symptom is "my `/sethome` silently does nothing", and the usual
cause is that the other plugin won the name.

This is always a warning, never an error. Running both is a legitimate setup: keep Essentials
for one feature and turn our overlapping module off, or run CMI's moderation alongside our
homes. The check states the overlap and leaves the decision to you.

These are not integrations. We call into none of them, and they are deliberately absent from
the tables above.

<Callout type="note" title="Importing from another plugin is a different thing">

Moving data across from EssentialsX, LiteBans, DecentHolograms, FancyHolograms,
Multiverse-Core or one of the player-warp plugins does not need that plugin installed at
all, and is not on this page. That is `/uxmess import`, covered under Getting Started.

</Callout>

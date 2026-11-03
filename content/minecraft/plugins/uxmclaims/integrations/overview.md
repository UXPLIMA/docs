---
title: Overview
order: 801
description: Every optional plugin, what it adds, and the switch that controls it.
icon: plug-zap
---

Every integration is optional. uxmClaims looks for each plugin once, at startup, and uses a no-op
stand-in for any it does not find, so it runs with none of them installed.

## Settings

| Plugin | Adds | Switch in `config.yml` |
|---|---|---|
| [Vault](vault.md) and an economy plugin | Charging for claims, chunks, time, warps, roles, invites, renames | `generalSettings.economySupport` |
| LuckPerms | Reading the `uxmclaims.limit.*`, `uxmclaims.cost.*` and `uxmclaims.ability.*` nodes | `generalSettings.permissionSupport` |
| [WorldGuard](worldguard.md) | Refusing claims on regions | `generalSettings.worldGuardSupport` |
| [PlaceholderAPI](placeholderapi.md) | The `%uxmclaims_…%` placeholders for other plugins | `generalSettings.placeholderSupport` |
| [Dynmap, BlueMap, Pl3xMap, squaremap](maps.md) | Claim areas on the web map | `visualizer.enabled` |
| Geyser or Floodgate | Bedrock forms instead of chest, anvil and chat menus | `bedrock.enabled`, `bedrock.menus` |
| Discord (no plugin) | Notifications on claim events | [webhooks.yml](../config/webhooks-yml.md) |

All switches default to `true`. Every switch except the two `bedrock` keys is read at startup only:
change it, then restart. A switch left on for a plugin that is not installed logs a warning such as
`WorldGuard support is enabled but WorldGuard is not installed!` and the integration stays off.

## What happens when one is missing

| Missing | Result |
|---|---|
| Vault | Nothing is charged |
| An economy plugin behind Vault | The plugin fails to start while `economySupport` is `true` |
| LuckPerms | Every limit and cost is the `defaultValue` from `entitlements.yml`, and every ability node counts as granted |
| WorldGuard | Regions are not checked. `generalSettings.disabledWorlds` still is |
| PlaceholderAPI | Other plugins cannot read `%uxmclaims_…%`. uxmClaims' own placeholders in its files still work |
| A map plugin | Nothing is drawn |
| Geyser and Floodgate | Every player gets the Java menus |

Two configurations stop the plugin at startup: `economySupport: true` with Vault installed but no
economy plugin registered with it, and an `economyProvider` that matches no registered economy. See
[Vault](vault.md).

## Turning one off deliberately

Setting a switch to `false` disables the integration even when the plugin is installed.

| Switch | Worth doing when |
|---|---|
| `worldGuardSupport: false` | Regions and claims are meant to overlap, such as a plot world inside one large region. `disabledWorlds` keeps working |
| `economySupport: false` | Claims should be free, or Vault is installed without an economy plugin |
| `permissionSupport: false` | Limits and costs should be the `defaultValue` for everyone |
| `placeholderSupport: false` | Another plugin already registers the `uxmclaims` identifier |
| `visualizer.enabled: false` | The web map should not show player builds |

## Bedrock players

With Geyser or Floodgate installed, players joining from Bedrock Edition get native forms for typing
names, confirming actions and, with `bedrock.menus: true`, for the menus themselves. The vault always
stays a chest. `dialog.bedrockFallback` decides what Bedrock players get when forms are off. The
forms and their files are on [Bedrock menus](../menus/bedrock.md).

## Notes

- uxmClaims is itself a protection provider other plugins can query. uxmEssentials, for instance,
  reads it when deciding whether a home may be set: see
  [uxmEssentials claims](../../uxmessentials/integrations/claims.md).
- Your own plugin does the same through the [developer API](../developer/index.md).

Related: [config.yml](../config/config-yml.md), [Limits and costs](../permissions/limits.md)

---
title: Overview
order: 801
description: Every optional plugin, what it adds, and the switch that controls it.
icon: plug-zap
---

Every integration is a `softdepend`. The plugin starts and runs with none of them installed — each
one has a no-op implementation that simply does nothing.

| Plugin | Adds | Switch |
|---|---|---|
| Vault | Charging money for claims, chunks, warps, invites and time | `generalSettings.economySupport` |
| A permission plugin | Reading `uxmclaims.limit.*` and `uxmclaims.ability.*` | `generalSettings.permissionSupport` |
| WorldGuard | Refusing claims that overlap a region | `generalSettings.worldGuardSupport` |
| PlaceholderAPI | The `%uxmclaims_...%` expansion and placeholders in messages | `generalSettings.placeholderSupport` |
| Dynmap, BlueMap, Pl3xMap, squaremap | Claim outlines on the web map | `visualizer.enabled` |
| Discord webhooks | Event notifications — no plugin needed | `webhooks.yml` |

## What happens when one is missing

| Missing | Result |
|---|---|
| Vault | Everything is free. Costs resolve but nothing is charged. |
| Permission plugin | Every player gets the `defaultValue` from `entitlements.yml`, and ability checks fall back to operator status. |
| WorldGuard | Region overlap is not checked. |
| PlaceholderAPI | Placeholder tokens render as literal text. |
| A map plugin | The visualizer does nothing. |

None of these are startup failures. The one configuration that *does* fail at startup is
`generalSettings.economyProvider` naming a Vault provider that is not registered — the plugin refuses
to start rather than silently charging the wrong currency.

## Turning one off deliberately

Setting the switch to `false` disables the integration even when the plugin is installed. That is
worth doing when:

- **`worldGuardSupport: false`** on a server where regions and claims are meant to overlap, such as a
  city plot world sitting inside one big region.
- **`economySupport: false`** while testing, so limits can be exercised without a balance.
- **`placeholderSupport: false`** if a placeholder in a message is causing recursion with another
  plugin's expansion.

## Also worth knowing

uxmClaims itself is a protection provider other plugins can query. uxmEssentials, for instance, reads
the active claim plugin when deciding whether a home may be placed — see
[uxmEssentials → Claims](../../uxmessentials/integrations/claims.md). The
[developer API](../developer/) is how a plugin of your own does the same.

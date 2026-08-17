---
title: Vault
order: 802
description: Charging for claims, chunks, warps and time, and picking a currency.
icon: coins
---

Vault bridges uxmClaims to whatever economy plugin you run: EssentialsX, CMI, CoinsEngine and the
rest.

```yaml
generalSettings:
  economySupport: true
  economyProvider: ''
```

Without Vault, or with `economySupport: false`, everything is free. Costs still resolve; nothing is
charged.

## What can cost money

| Action | Node | Default |
|---|---|---|
| Creating a claim | `uxmclaims.cost.claim.<level>.<price>` | `0.0` |
| Buying a chunk | `uxmclaims.cost.chunk.<count>.<price>` | `25.0` |
| Creating a warp | `uxmclaims.cost.warp.<count>.<price>` | `0.0` |
| Creating a role | `uxmclaims.cost.role.<count>.<price>` | `0.0` |
| Teleporting to a warp | `uxmclaims.cost.warptp.<public\|private>.<price>` | `0.0` |
| Sending an invitation | `uxmclaims.cost.invite.<count>.<price>` | `0.0` |
| Renaming a claim | `uxmclaims.cost.rename.<price>` | `0.0` |
| Extending, **per second** | `uxmclaims.cost.time.<price>` | `5.0` |

Only chunks and time cost anything out of the box.

Costs use the `MIN` strategy: see [Limits and costs](../permissions/limits.md), so a rank's node can
only ever discount below `defaultValue`. Granting a higher number changes nothing.

## Picking a currency

```yaml
economyProvider: 'coins'
```

The value is matched against the registered Vault provider name, the Bukkit plugin name, and the
currency's singular and plural names. Empty means Vault's default provider.

<Callout type="warning" title="A provider name that does not resolve fails startup">

This is deliberate. On a multi-currency server the alternative (silently falling back to the default
economy) means charging players in the wrong currency for however long it takes someone to notice.
Better a server that will not start.

</Callout>

## Refunds

`/claim chunk remove` sells a chunk back. Deleting a claim does **not** refund what was spent on it,
and there is no refund percentage setting: price accordingly, especially `cost.time`, which at the
shipped `5.0` per second makes a week's extension expensive.

## Not enough money

The action is refused with `errorInsufficientFunds` from `messages.yml`, and nothing is charged
partially. The economy call and the claim change are one operation.

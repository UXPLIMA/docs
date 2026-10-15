---
title: Vault
order: 802
description: Charging for claims, chunks, warps and time, and picking a currency.
icon: coins
---

Vault connects uxmClaims to the economy plugin you run, such as EssentialsX, CMI or CoinsEngine.
Every price comes from `entitlements.yml` and the `uxmclaims.cost.*` permission nodes.

## Settings

```yaml
generalSettings:
  economySupport: true
  economyProvider: ''
```

| Key | Default | Meaning |
|---|---|---|
| `economySupport` | `true` | Charge through Vault. `false` makes everything free |
| `economyProvider` | `''` | Which Vault economy to use; empty means Vault's default |

Both are read at startup. Without Vault installed, nothing is charged.

## What can cost money

| Action | Node | Default |
|---|---|---|
| Creating a claim | `uxmclaims.cost.claim.<level>.<price>` | `0.0` |
| Adding a chunk | `uxmclaims.cost.chunk.<count>.<price>` | `25.0` |
| Creating a warp | `uxmclaims.cost.warp.<count>.<price>` | `0.0` |
| Creating a role | `uxmclaims.cost.role.<count>.<price>` | `0.0` |
| Teleporting to a warp | `uxmclaims.cost.warptp.<public\|private>.<price>` | `0.0` |
| Sending an invite | `uxmclaims.cost.invite.<count>.<price>` | `0.0` |
| Renaming a claim | `uxmclaims.cost.rename.<price>` | `0.0` |
| Adding time | `uxmclaims.cost.time.<price>` | `5.0` |

Out of the box only chunks and time cost anything. Time is priced per second **per chunk**: adding one
hour to a four-chunk claim at the shipped `5.0` costs 72,000. Removing time costs nothing and refunds
nothing.

A granted price node replaces the default, so it can raise a price as well as lower it. With LuckPerms
in use, operators and holders of `uxmclaims.admin` pay nothing. How the nodes combine is on
[Limits and costs](../permissions/limits.md).

## Picking a currency

```yaml
generalSettings:
  economyProvider: 'coins'
```

The value is matched, ignoring case, against each registered Vault economy's name, the name of the
plugin that registered it, and its singular and plural currency names.

<Callout type="warning" title="Some economy settings stop the plugin at startup">

With `economySupport: true`, uxmClaims refuses to start when Vault has no economy plugin registered,
or when `economyProvider` matches none of them. The console lists the economies it found. On a
multi-currency server this is deliberate: falling back to the default economy would charge players
in the wrong currency.

</Callout>

## Notes

- **No refunds.** Removing a chunk, removing time and deleting a claim return nothing. Price
  accordingly.
- **Not enough money** refuses the action with `errorInsufficientFunds` from `messages.yml`, and
  nothing changes.
- **Charging comes before saving.** If the save fails, the plugin pays the amount back.
- **Admin commands are free.** `/claim admin` actions skip the economy entirely.

Related: [Overview](overview.md), [Limits and costs](../permissions/limits.md), [entitlements.yml](../config/entitlements-yml.md)

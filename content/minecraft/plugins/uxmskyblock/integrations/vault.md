---
title: Vault (Economy)
order: 830
description: 'Vault connects uxmSkyblock to your server''s economy. It''s optional, but
  without it, the money side of upgrades and the bank are disabled.'
---

---

## What Vault Enables

| Feature | Needs Vault? |
|---------|--------------|
| Upgrade **level** requirements | No |
| Upgrade **money** costs | **Yes** |
| The island **bank** | **Yes** |
| Everything else | No |

---

## Setup

1. Install **Vault** and any **economy plugin** (e.g. EssentialsX Economy).
2. Make sure `economy.enabled: true` in [config.yml](../configuration/config-yml.md).
3. That's it: upgrade costs and the bank now use real money.

```yaml
economy:
  enabled: true
```

---

## Running Without an Economy

If Vault is missing (or `economy.enabled: false`):

- Upgrades **only** check the island level: `required-money` is ignored.
- The bank commands are unavailable.

This is fully supported: the plugin is designed to work with or without an economy.

---

## Related

- [Upgrades](../progression/upgrades.md): where money costs apply
- [Bank & Economy](../commands/bank.md): the island bank

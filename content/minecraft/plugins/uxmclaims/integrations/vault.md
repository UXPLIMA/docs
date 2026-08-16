---
title: Vault (Economy)
order: 340
icon: landmark
---

## What is Vault?

Vault is a bridge plugin that connects economy plugins (like EssentialsX, CMI, or QuickShop) with other plugins.

**You need:**

1. Vault plugin installed
2. An economy plugin (EssentialsX, CMI, etc.)

---

## Enabling/Disabling

In `config.yml`:

```yaml
generalSettings:
  economySupport: true
```

| Value   | Effect                       |
|---------|------------------------------|
| `true`  | Economy features are enabled |
| `false` | Everything is free           |

---

## What Uses Economy?

| Action           | Where to Configure                           |
|------------------|----------------------------------------------|
| Creating a claim | `entitlements.yml` → `uxmclaims.cost.claim`  |
| Buying chunks    | `entitlements.yml` → `uxmclaims.cost.chunk`  |
| Creating warps   | `entitlements.yml` → `uxmclaims.cost.warp`   |
| Using warps      | `entitlements.yml` → `uxmclaims.cost.warptp` |
| Creating roles   | `entitlements.yml` → `uxmclaims.cost.role`   |
| Inviting members | `entitlements.yml` → `uxmclaims.cost.invite` |
| Renaming claims  | `entitlements.yml` → `uxmclaims.cost.rename` |
| Extending time   | `entitlements.yml` → `uxmclaims.cost.time`   |

See [entitlements.yml](../config/entitlements-yml.md) for details.

---

## Setup Checklist

1. ✅ Install Vault plugin
2. ✅ Install an economy plugin
3. ✅ Set `economySupport: true` in config.yml
4. ✅ Configure costs in entitlements.yml
5. ✅ Restart server

---

## Troubleshooting

### "Economy plugin not found"

- Make sure Vault is installed
- Make sure an economy plugin is installed
- Check console for economy registration

### "Players can't afford claims"

- Check their balance with `/balance`
- Lower the costs in entitlements.yml

### "I want everything free"

Set `economySupport: false` in config.yml, or set all costs to `0.0` in entitlements.yml.

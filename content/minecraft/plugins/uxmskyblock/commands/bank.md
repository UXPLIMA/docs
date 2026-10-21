---
title: Bank & Economy
order: 550
description: 'Every island has a shared bank: a single balance the whole team can pay
  into. The bank is backed by Vault, so it uses your server''s real economy.'
icon: landmark
---

<Callout type="note" title="Vault required">

The bank only works when Vault and an economy plugin are installed. Without
them, the bank commands are unavailable.

</Callout>

---

## `/is bank`

Shows the island's current bank balance.

## `/is bank deposit <amount>`

Moves money from **your wallet** into the island bank. **Any member** can deposit.

```
/is bank deposit 5000
```

## `/is bank withdraw <amount>`

Moves money from the island bank into **your wallet**. Requires the `BANK`
permission (member rank and above by default).

```
/is bank withdraw 2000
```

---

## Who Can Do What

| Action | Required permission |
|--------|---------------------|
| Deposit | Any team member |
| Withdraw | `BANK` |
| View balance | Any team member |

This split is intentional: everyone can chip in, but only trusted ranks can take
money out. Adjust which roles have `BANK` in
[roles.yml](../configuration/roles-yml.md) or with `/is role perm`.

---

## Placeholder

The current balance is exposed through PlaceholderAPI as:

```
%skyblock_bank%
```

See [PlaceholderAPI](../integrations/placeholderapi.md).

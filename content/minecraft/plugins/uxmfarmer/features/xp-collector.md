---
title: XP Collector
order: 35
icon: sparkles
---

XP Collector picks up the experience dropped by entities that die near the farmer and holds it in
the farmer's own XP store. Players withdraw it, or sell it.

```yaml
xp-collector:
  name: "XP Collector"
  status: true
  default-status: false
  sellable-by-default: true
  permission-required: true
  xp-unit-price: 0.001
  xp-tax: 0.1
```

| Option | What it does |
|---|---|
| `permission-required` | Requires the owner to hold `uxmfarmer.xpcollector` |
| `sellable-by-default` | Whether new farmers allow their XP to be sold |
| `xp-unit-price` | Money paid per point of XP |
| `xp-tax` | Tax on XP sales, as a fraction — `0.1` is 10% |

Selling XP additionally requires the owner to hold `uxmfarmer.sellxp`. Without it, no member of
that farmer can sell XP even if the farmer's own `XP_SELLABLE` setting is on — the owner's
permissions gate the farmer, and the farmer gates the members.

## Pricing XP

`xp-unit-price` is deliberately tiny. At `0.001`, a mob dropping 5 XP is worth half a cent, and a
grinder running all night is worth a modest sum — which is the intent. XP is a by-product here, not
an income stream, and raising this by an order of magnitude turns every mob farm into a money
printer without the player having to build anything new.

Set `xp-unit-price: 0` and `sellable-by-default: false` if you want XP collected for convenience
only, with no money attached at all.

## Withdrawing

Players take XP out through the farmer menu. The amount is entered on a sign or in chat, depending
on `take-amount.input-type` in `general-settings.yml`, and the operation is rate-limited by
`cooldowns.sell-xp-cooldown` — 120 seconds by default.

## Placeholders

```
%uxmfarmer_<uuid>_stored_xp%
%uxmfarmer_<uuid>_all_time_collected_xp%
%uxmfarmer_<uuid>_all_time_sold_xp%
%uxmfarmer_<uuid>_all_time_earned_xp%
%uxmfarmer_<uuid>_all_time_tax_xp%
```

---
title: Production
order: 31
description: Measuring output and income per minute, hour and day.
icon: trending-up
---

Production is the module that measures. It does not collect anything itself — it watches what the
farmer collects and turns it into a rate, so a player can see that their wheat is worth 4,200 an
hour and decide whether the next level is worth buying.

```yaml
production:
  enabled: true
  permission-required: true
```

The permission is `uxmfarmer.production`, held by the farmer's **owner**.

## What it measures

For each product, per minute, per hour and per day:

- **Rate** — how many units the farmer collected
- **Income** — that rate multiplied by the product's current price from your price source

Both are available as placeholders, which is how they reach menus and holograms:

```
%uxmfarmer_<uuid>_production_WHEAT_rate_hourly%
%uxmfarmer_<uuid>_production_WHEAT_income_daily%
```

The full list is on the [Placeholders](../placeholders.md) page.

## Reading the numbers

Rates are extrapolated from the per-minute figure, so the hourly number is what the farmer *would*
collect if the last minute repeated. This makes it responsive — a player sees the effect of
expanding their farm immediately — but it also means the daily figure jumps around while a crop
cycle is in progress. It is a projection, not a total. For totals, use the all-time statistics
placeholders.

Income uses the live price for the player asking, which means a player with a
`uxmfarmer.taxdiscount.<percentage>` permission and one without will see different numbers for the
same farmer. That is correct — the income figure is what *you* would earn selling it.

## Debugging

```
/uxmfarmer select
/uxmfarmer productiondebug [product]
```

Prints what the module is counting for the selected farmer. Use it when a player reports that a
product shows zero income while its storage is clearly filling — usually the answer is that the
product has no price at your price source, so the rate is right and the income is honestly zero.

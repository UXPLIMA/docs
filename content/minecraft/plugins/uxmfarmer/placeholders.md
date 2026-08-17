---
title: Placeholders
order: 70
description: Every placeholder the expansion registers, per farmer, product and statistic.
icon: braces
---

uxmFarmer registers a PlaceholderAPI expansion under the identifier `uxmfarmer`. Every placeholder
identifies a farmer by its UUID:

```
%uxmfarmer_<uuid>_<what>%
```

The UUID is the farmer's, not the player's — get it from `/uxmfarmer uuidof <player>`,
`/uxmfarmer select`, or `/uxmfarmer nearfarmers`. An unknown UUID returns an empty string.

Inside menu and hologram lore the plugin's own `<uuid>` tag is substituted first, which is why the
shipped files are written as:

```
<papi:'uxmfarmer_<uuid>_amount_GOLD_NUGGET'>
```

That form works in any lore the plugin draws for a specific farmer, and needs no hard-coded UUID.

## Farmer

| Placeholder | Returns |
|---|---|
| `..._owner` | Owner's name, or their UUID if the name is unknown |
| `..._region` | The region id the farmer belongs to |
| `..._sales_status` | Whether Auto Sell is on, as configured text |
| `..._collecting_status` | Whether natural drop collection is on |
| `..._total_products` | Total units across every product |
| `..._stored_xp` | XP currently held |

`sales_status` and `collecting_status` return the strings from your language file, so they read as
"Enabled" / "Disabled" in whatever language you are running.

## Per product

`<material>` is a Bukkit material name.

| Placeholder | Returns |
|---|---|
| `..._amount_<material>` | Units in storage |
| `..._capacity_<material>` | Capacity at the current level |
| `..._level_<material>` | Current level |
| `..._sellable_<material>` | Whether it is set to sell |
| `..._collecting_<material>` | Whether it is being collected |

An unknown material returns `0` for the numeric ones and the disabled text for the others.

## Production

| Placeholder | Returns |
|---|---|
| `..._production_<material>_rate_minute` | Units per minute |
| `..._production_<material>_rate_hourly` | Projected units per hour |
| `..._production_<material>_rate_daily` | Projected units per day |
| `..._production_<material>_income_minute` | Money per minute |
| `..._production_<material>_income_hourly` | Projected money per hour |
| `..._production_<material>_income_daily` | Projected money per day |

The hourly and daily figures are extrapolated from the per-minute one, so they are projections and
move as the farm does. Income uses the live price for the player reading the placeholder, which
means two players can see different numbers for the same farmer — correctly, because their tax
discounts differ.

## All-time statistics

| Placeholder | Returns |
|---|---|
| `..._all_time_collected` | Units collected, ever |
| `..._all_time_collected_xp` | XP collected, ever |
| `..._all_time_sold` | Units sold |
| `..._all_time_sold_xp` | XP sold |
| `..._all_time_earned` | Money earned from products |
| `..._all_time_earned_xp` | Money earned from XP |
| `..._all_time_tax` | Total tax paid |
| `..._all_time_tax_product` | Tax paid on products |
| `..._all_time_tax_xp` | Tax paid on XP |

These are totals and do not move backwards, which makes them the right thing to put on a leaderboard
or a scoreboard. `all_time_tax` is `all_time_tax_product` plus `all_time_tax_xp`.

Money values are formatted with the plugin's money formatter, counts with its number formatter — so
they arrive readable and do not need a `math` expansion to make them presentable.

## Using placeholders as conditions

Level requirements and conversion requirements take any PlaceholderAPI placeholder, compare its
output to a string, and gate the purchase on the result:

```yaml
requirements:
  "vip":
    display: "<green>VIP Rank"
    condition: "%vault_hasgroup_VIP%"
    required-output: "yes"
```

uxmFarmer's own placeholders work here too — a level that requires the player's farmer to already
hold a certain amount of another product, for example.

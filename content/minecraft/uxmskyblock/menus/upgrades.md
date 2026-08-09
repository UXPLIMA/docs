---
title: Upgrades
order: 610
---

`/is upgrade` opens the **Upgrades menu**. Items are filled **automatically** from
[upgrades.yml](../configuration/upgrades-yml.md) — each upgrade's slot and icon are
defined there — so the menu always matches your configured upgrades.

---

## Reading an Upgrade

Each upgrade item shows the current tier, the next tier, and what the next tier
costs:

```
Current: <value> (Lvl <n>)
Next:    <value> (Lvl <n+1>)

Required Level: <island level>
Required Money: <Vault cost>

Click to upgrade!
```

For the **generator** upgrade, the "value" shown is the **tier number**, since the
output is a weighted set of blocks.

Maxed-out upgrades instead show `✔ Maximum level!`.

---

## Default Upgrades

| Upgrade | Effect |
|---------|--------|
| **Team Limit** | Max members on the island |
| **Island Size** | Size of the protected area |
| **Crop Double Chance** | % chance to double a harvested crop |
| **Mob Drops** | Drop multiplier from mobs |
| **Spawner Rates** | Spawner speed multiplier |
| **Generator** | What the cobble generator produces |

Full tier tables are on the [Upgrades](../progression/upgrades.md) page.

---

## Buying an Upgrade

Clicking a tier checks two requirements:

1. **Island level** ≥ the tier's `required-level`
2. **Bank/economy** has the tier's `required-money` (only when Vault economy is
   enabled — see [economy](../configuration/config-yml.md))

If both pass, the upgrade is purchased and applied immediately (the size upgrade,
for instance, instantly refreshes the island border).

---

## Customizing

The frame items (info, back button) and lore templates live in
`menus/upgrades.yml`; the upgrades themselves (slots, icons, tiers, costs) live in
`upgrades.yml`. See [Menu Customization](../configuration/menus.md).

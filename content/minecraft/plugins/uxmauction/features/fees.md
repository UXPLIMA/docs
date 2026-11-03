---
title: Fees
order: 306
description: The four charges, flat or percent, floors, caps and group rates.
icon: receipt
---

`fees` in [config.conf](../config/config-conf.md#fees) has four charges, all shipping at zero:

| Charge | Paid by | When |
|---|---|---|
| `listing` | The seller | When the listing is written; not given back when it expires |
| `reprice` | The seller | When they lower a price |
| `sale-tax` | The seller | Kept out of what they earn |
| `purchase-tax` | The buyer | On top of the price |

Each takes the same keys:

| Key | Meaning |
|---|---|
| `type` | `flat` for the same amount every time, `percent` for a share of the price |
| `value` | The amount, or the rate out of a hundred |
| `minimum` | A floor, so a charge of a fraction is still worth taking |
| `cap` | A ceiling, so a large sale does not take a house |
| `groups` | Permission nodes with a rate of their own |

```hocon
fees {
  sale-tax {
    type = "percent"
    value = "5"
    cap = "1000"
    groups { "uxmauction.fee.vip" { value = "2" } }
  }
}
```

A player who holds two group nodes pays the lower rate. A player who is not online holds nothing, so a seller paid while
away pays the house rate. A sale tax never takes more than the sale brought. The group nodes are yours to name; the
plugin does not declare them.

Related: [config.conf](../config/config-conf.md#fees)

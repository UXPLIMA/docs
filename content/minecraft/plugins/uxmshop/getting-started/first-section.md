---
title: Your first section
order: 104
description: A section of ores with a moving price and a daily allowance, step by step.
icon: footprints
---

1. Hold a diamond and run `/uxmshop iteminfo`: it prints an `item` block to paste.
2. Create `shops/ores.conf`:

```hocon
order    = 30
menu     = "section"
currency = "coins"
icon { material = "diamond" }
name { any = "Ores", tr = "Madenler" }
band { enabled = true }
lines {
  diamond {
    order = 10
    type  = item
    item  { material = "diamond", amount = 1 }
    buy   = 250
    sell  = 100
    match = "material"
    stock {
      sell {
        player { enabled = true, cap = 64, every = "24h" }
      }
    }
  }
}
```

3. Run `/uxmshop reload`. If the console names a problem, fix it and reload again.
4. `/shop` shows the new section; `/shop open ores` opens it directly.
5. Sell a diamond with `/sell hand` and read the price move: the band lowers what the next diamond pays and walks it
   home every 30 minutes.

`coins` is the Vault currency. Without Vault, write `currency = "experience"`.

Related: [Sections and lines](../features/sections-and-lines.md), [Prices](../features/prices.md)

---
title: Your first minion
order: 204
description: Give a miner, place it, fuel it, read its panel and upgrade it.
icon: sparkles
---

## 1. Give one

```
/uxmminions give Steve miner
```

Steve gets a miner item at level 1. Into a full inventory nothing is handed over and you are told so.

## 2. Place it

Steve places the item like a block, facing the stone he wants mined. The block is not placed: a minion appears there,
drawn with packets. A limit, a world or a claim may refuse it, with a line that says which.

## 3. Fuel it

Right click the minion to open its [panel](../menus/panel.md), then the coal button. Click a grade to put one item of it
in: coal is 32 actions at 10 percent faster. With `fuel.enabled = false` it runs at its plain interval.

## 4. Watch it work

Every 50 ticks at level 1 it breaks the block it faces into its 3 slot storage. When it is full it stops and says so.
The storage button takes items out; the hopper button picks where output goes.

## 5. Upgrade

The bottle opens the [upgrade window](../menus/upgrade.md). Level 2 asks for 4800 and 64 cobblestone; with the shipped
`economy.currency = "exp"` the 4800 is experience points. Name an economy in `config.conf` to charge money.

Related: [The shipped types](../features/catalogue.md), [Commands](../commands/player.md)

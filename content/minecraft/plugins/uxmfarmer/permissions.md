---
title: Permissions
order: 80
description: Module, command and tiered permissions, and a working default group.
icon: shield-check
---

uxmFarmer has three kinds of permission, and they are not interchangeable:

- **Module permissions**: checked against the farmer's **owner**, and decide what the farmer can
  do at all.
- **Command permissions**: checked against whoever typed, and decide what a player may run.
- **Rank permissions**, not Bukkit permissions. They live in
  [`ranks.yml`](features/members-and-ranks.md) and decide what a member may do with someone else's
  farmer.

## Module permissions

Every one of these is checked against the owner. A member's own permissions have no effect on
whether a module runs.

| Permission | Grants |
|---|---|
| `uxmfarmer.production` | The Production module |
| `uxmfarmer.autoharvest` | The Auto Harvest module |
| `uxmfarmer.autokill` | The Auto Kill module |
| `uxmfarmer.autokill.modify` | Changing Auto Kill's settings |
| `uxmfarmer.autosell` | The Auto Sell module |
| `uxmfarmer.autosell.modify` | Changing Auto Sell's settings |
| `uxmfarmer.xpcollector` | The XP Collector module |
| `uxmfarmer.sellxp` | Selling collected XP |
| `uxmfarmer.collectplayerdrops` | Collecting items dropped by players |
| `uxmfarmer.useglow` | The farmer's glow effect |
| `uxmfarmer.usebabyvariant` | Making the farmer entity a baby |
| `uxmfarmer.addmember` | Adding members, when `adding-member-requires-permission` is on |

Each module's `permission-required: false` in `modules.yml` switches its check off, which is how you
give a module to everyone without touching a permission plugin.

## Tiered permissions

These take a number in the node itself. Give the player the highest one they should have.

| Permission | Effect |
|---|---|
| `uxmfarmer.maxfarmer.<amount>` | Raises `maximum-farmer-per-player` for this player |
| `uxmfarmer.taxdiscount.<percentage>` | Reduces sale tax by that percentage |
| `uxmfarmer.upgradediscount.<percentage>` | Reduces the upgrade-all price by that percentage |

So `uxmfarmer.taxdiscount.50` halves the tax and `uxmfarmer.maxfarmer.5` allows five farmers.
Discounts stop at 100: a higher number counts as 100, and the console warns about it. A sale made
from the menu uses the seller's tax discount; an automatic sale uses the owner's.

<Callout type="note" title="These nodes are not in the permission list">

Wildcard nodes cannot be declared with a real value, so `uxmfarmer.maxfarmer.<amount>` does not
appear in your permission plugin's auto-complete. Type it out in full.

</Callout>

## Player command permissions

| Permission | Command | Default |
|---|---|---|
| n/a | `/farmer`, `/farmer buy` | Everyone |
| `uxmfarmer.command.menu` | `/farmer menu` | Everyone |
| `uxmfarmer.command.teleport` | `/farmer teleport` | Operators |
| `uxmfarmer.command.disappear` | `/farmer disappear` | Operators |
| `uxmfarmer.command.sellall` | `/farmer sellall` | Operators |
| `uxmfarmer.changeskin` | `/farmer skin` | Operators |
| `uxmfarmer.returnfarmer` | `/farmer return` | Operators |

Most of these default to operators only, which means a fresh install gives players a farmer they
cannot teleport, sell from, or return. Granting `uxmfarmer.command.teleport`,
`uxmfarmer.command.sellall` and `uxmfarmer.returnfarmer` to your default group is part of setting
the plugin up, not an extra.

## Admin command permissions

| Permission | Command |
|---|---|
| `uxmfarmer.command.admin` | `/uxmfarmer select` |
| `uxmfarmer.command.info` | `/uxmfarmer` |
| `uxmfarmer.command.help` | `/uxmfarmer help` |
| `uxmfarmer.command.reload` | `/uxmfarmer reload` |
| `uxmfarmer.command.save` | `/uxmfarmer save` |
| `uxmfarmer.command.bypass` | `/uxmfarmer bypass` |
| `uxmfarmer.command.adminmenu` | `/uxmfarmer menu` |
| `uxmfarmer.command.givefarmer` | `/uxmfarmer givefarmer` |
| `uxmfarmer.command.give` | `/uxmfarmer give` |
| `uxmfarmer.command.delete` | `/uxmfarmer delete` |
| `uxmfarmer.command.deleteall` | `/uxmfarmer deleteall` |
| `uxmfarmer.command.deleteuuid` | `/uxmfarmer deleteuuid` |
| `uxmfarmer.command.setowner` | `/uxmfarmer setowner` |
| `uxmfarmer.command.uuidof` | `/uxmfarmer uuidof` |
| `uxmfarmer.command.nearfarmers` | `/uxmfarmer nearfarmers` |
| `uxmfarmer.command.adminteleport` | `/uxmfarmer teleport` |
| `uxmfarmer.command.editsetting` | `/uxmfarmer editsetting` |
| `uxmfarmer.command.bulkedit` | `/uxmfarmer bulkedit` |
| `uxmfarmer.command.fixstatus` | `/uxmfarmer fixstatus` |
| `uxmfarmer.command.setproductlevel` | `/uxmfarmer setproductlevel` |
| `uxmfarmer.command.statistics` | `/uxmfarmer statistics` |
| `uxmfarmer.command.sweep` | `/uxmfarmer sweep` |
| `uxmfarmer.command.checkage` | `/uxmfarmer checkage.target`, `/uxmfarmer checkage.chunk` |
| `uxmfarmer.command.productiondebug` | `/uxmfarmer productiondebug` |
| `uxmfarmer.command.checkperm` | `/uxmfarmer checkperm` |
| `uxmfarmer.command.clearpermcache` | `/uxmfarmer clearpermcache` |
| `uxmfarmer.command.clearlogs` | `/uxmfarmer clearlogs` |
| `uxmfarmer.seedebugs` | Receiving debug output in chat |

All default to operators.

## A working default group

```
uxmfarmer.command.menu
uxmfarmer.command.teleport
uxmfarmer.command.sellall
uxmfarmer.command.disappear
uxmfarmer.returnfarmer
uxmfarmer.production
uxmfarmer.autoharvest
uxmfarmer.maxfarmer.1
```

That is a farmer that harvests and can be sold from, with the paid modules (Auto Kill, Auto Sell,
XP Collector) and extra farmer slots left as things to earn.

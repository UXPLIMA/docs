---
title: Upgrades
order: 406
description: Buying a level, the requirement list, rewards, and consuming one minion into another.
icon: circle-arrow-up
---

## Buying a level

The bottle on the panel opens the [upgrade window](../menus/upgrade.md): the current level, the next one, and each
requirement with the operator's own message for it, so a player sees what they miss and never a raw string. The buy
button runs the next level's `requirements`, then its `rewards`. The take verbs in the rewards are the charge, checked
whole before any is taken. `effects.upgraded` runs after.

The shipped ladders charge money and an item for every level, and from level 4 a count of actions the minion did.

## Consuming

```hocon
upgrades {
  consume-minion = false
}
```

On, a player feeds one minion to another of the same type and level to raise it: `/minions consume <eaten> <into>`, or
the minecart in the upgrade window, which eats the least worked candidate. The whole move is one transaction: the check
is made again inside it, the eaten row is deleted and the level is raised together, or neither happens. It ships off
because it is a duplication surface in both products that ship it; the key is read on each call.

Related: [Types and levels](types-and-levels.md#requirements)

---
title: Placing and using
order: 303
description: The minion item, placing, the panel, facing, trust, names, teleporting, pickup and the pocket.
icon: mouse-pointer-click
---

## The item

`/uxmminions give` hands a player an item that carries the minion's identity. Place it like a block: the block is never
placed, and a minion appears there instead, facing the way the player faced. Placing is refused, with a line that says
why, by a world list, a [limit](limits.md), or a [claim](claims.md) where the owner may not build.

The block a minion stands on cannot be broken while it stands there.

## The panel

A click on a minion opens its [panel](../menus/panel.md). The owner, a player on its trust list, and the claim leader on
their own land (`claims.leader-bypass`) may open it; anybody else is told it is not theirs. An operator opens any with
`/uxmminions open <id>`.

## Facing

The compass turns it one step clockwise (left click) or anticlockwise (right click); four banners point it north, east,
south or west in one tap, so a Bedrock player reaches every direction.

## Trust

`/minions trust <id> <player>` lets a friend use one minion; the [trusted window](../menus/trusted.md) takes them off.
The trust list serves the friend, and the claim leader bypass serves the island.

## Teleporting

The ender pearl on the panel, or a right click in [your minions](../menus/my-minions.md), takes you to a minion.

```hocon
teleport {
  enabled          = true
  cost             = 250.0
  cooldown-seconds = 5
}
```

The cost is in `economy.currency`; zero is free.

## Pickup and the pocket

The barrier on the panel opens the [pickup question](../menus/pickup.md); nothing is picked up until the player clicks
yes. A pickup keeps everything: the level, fuel, stock, name and skin stay with the identity, and the item names it. A
picked up minion that did not fit the inventory waits in the [pocket](../menus/pocket.md), opened from your minions, to
be taken out again.

## Your minions

`/minions` lists every minion you placed with its level, status and world.

Related: [Panel](../menus/panel.md), [Commands](../commands/player.md)

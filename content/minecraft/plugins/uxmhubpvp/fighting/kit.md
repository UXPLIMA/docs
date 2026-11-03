---
title: The kit
order: 402
description: What a fighter is handed, where their own items go while they fight, and how they come back.
icon: shield-half
---

A player who turns PvP on gives up their inventory for the length of the fight and holds the kit of `kit.conf`
instead. The kit is the same for everybody; a player cannot bring their own items into a fight.

## Entering

1. The whole inventory is taken: the 36 slots, the armour and the off hand. The slot the player held is remembered.
2. It is written to the database, in the `inventory` table.
3. Only when the write has finished is the kit handed out. The weapon slot is selected.
4. The leave face of the toggle item goes on top, in `toggle-item.in-pvp.slot`.

If the inventory cannot be taken or written, the items go straight back, PvP stays off, and the player reads *Your
items could not be put away, so PvP stayed off. You lost nothing.*

## Leaving

However the fight ends, by the toggle, by a death, by quitting or by the server stopping, the same steps run:

1. PvP is switched off and the glow taken away.
2. The inventory is emptied, kit and all, and the saved items are put back, with the slot the player held.
3. Only then is the saved copy deleted from the database.

Game mode, health, hunger, experience and potion effects are not touched by entering or leaving.

## After a crash

If the server stops hard while a player fights, step 3 never ran and their items are still in the database. The
next time the player joins, they get them back and read *The server stopped while you were fighting. Your items are
back.* On a network sharing one database, the server they join next does this, whichever it is.

## The shipped kit

| Piece | Item |
|---|---|
| Helmet, chestplate, leggings, boots | Iron, unbreakable |
| Weapon, in slot 0 | Diamond sword named *Lobby Blade*, Sharpness I, unbreakable |
| Off hand | Shield, unbreakable |
| Slot 1 | 5 golden apples |
| Slot 2 | 16 cooked beef |

Every piece hides its attributes and its unbreakable line. To change it, see [kit.conf](../config/kit-conf.md).

<Callout type="warning" title="Kit items can leave the fight">

The kit is not locked. A fighter can drop a piece or put it in a container, and whoever picks it up keeps it, an
unbreakable diamond sword included. In the other direction, anything a fighter picks up during the fight is gone when
their own inventory comes back. Keep containers out of reach of fighters, or keep the kit to items that do no harm in
the lobby.

</Callout>

## Notes

- **The kit is read again on reload.** A fighter keeps the kit they were given; the next player to enter gets the new
  one.
- **A piece with an unknown material** is left out of the kit; an unknown enchantment or item flag is left off the
  piece. The console names each.

Related: [kit.conf](../config/kit-conf.md), [Turning PvP on and off](overview.md), [Database](../database/overview.md)

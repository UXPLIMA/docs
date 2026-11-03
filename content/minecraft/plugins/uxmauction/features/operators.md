---
title: Operators
order: 411
description: Locking the house, the audit log, restoring, returning, purging, lookups and the timed work.
icon: shield
---

## Locking

`/uxmauction lock on` closes the house on every server; `off` opens it. A closed house shows every window and moves
nothing: no listing is created, bought, cancelled or expired. The door lives in the database; `house.locked` only seeds a
house that no server ever closed or opened.

## The log

Every move is a line of the audit log. `/uxmauction log [player]` shows the newest `admin.log-lines` (10): a player reads
them in the [log window](../menus/admin-log.md), where one click gives an item back; the console reads them in text.
`/uxmauction restore <transaction>` gives the item of one line back, once.

## Taking things down

| Command | Does |
|---|---|
| `/uxmauction cancel <id>` | Takes any listing or order down; an auction with bids gives the money back first |
| `/uxmauction return <player>` | Empties a player out of the house into their mailbox; a name, or the id from a lookup |
| `/uxmauction purge <days>` | Deletes closed rows older than that; nothing open is reached |

## Reading

`/uxmauction lookup <listing>` reads one listing in full; `/uxmauction stats` reads the size of the house.

## The timed work

The house sweeps on a timer, `house.work`:

| Key | Default | Meaning |
|---|---|---|
| `sweep-every` | `"1m"` | How often it ends what expired and finishes what a crash left half done; never zero |
| `expiry-batch-size` | `200` | Listings or orders one sweep takes; the rest go to the next |
| `auto-fill-candidates` | `20` | Orders a new listing is offered to |
| `watches-told` | `20` | Watchers a new listing tells |
| `reconciliation-window` | `"5m"` | How old a claimed sale has to be before the house finishes it as a half sale; longer than a purchase can take |

The defaults suit most servers.

## Item lines

`item.hide-lines` lists the data components whose client lines are kept quiet under a tile: attribute modifiers,
unbreakable, tool, weapon, equippable and the rest. What is not in the list is what a buyer reads before they pay:
enchantments, potion effects, shulker contents, book text, firework effects. The house never hides one of those. Write
an empty list and the client says everything.

Related: [Admin commands](../commands/admin.md)

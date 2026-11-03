---
title: "Player shops"
order: 50
description: "A player shop is the half of the plugin where another player is the counterparty."
---

A player shop is the half of the plugin where another player is the counterparty. A container in the world
is the handle a visitor clicks. **The stock is a number in the database and never the contents of the
container.** A chunk that does not load, an explosion, a hopper or another plugin removing the block costs no
item, and the stock may hold more than the container could.

## Opening a shop

1. The owner opens a container they are allowed to open and looks at it.
2. `/myshop open` opens a shop behind it.
3. `/myshop add <buy> <sell> <match>` adds the item in their hand as a line. `match` is one of the modes in
   [the catalogue](catalogue.md).
4. `/myshop stock <offer> <amount>` moves items from their inventory into a line, and `/myshop take` moves
   them back.
5. `/myshop price`, `/myshop remove`, `/myshop list`, `/myshop find` and `/myshop profits` manage and read
   the rest. See the [reference](reference.md).

A visitor right-clicks the container and the shop opens instead of the container. Sneaking while clicking
opens the container itself, so an owner can reach their own chest. Breaking the container is refused for
everybody but the owner, who closes the shop with `/myshop close` and gets the stock back.

An owner can turn off selling, turn off buying and choose whether they are told about a trade, from the shop
window.

## The claim check

A shop can only be opened behind a container the player has just opened. A claim plugin that refused the
interaction has already answered, so the shop needs no claim plugin hook and works with whichever one the
server runs. `player-shops.container-memory` (default `30s`) is how long a container stays open to becoming a
shop after the player reached it.

## Limits

| Key | Meaning |
|---|---|
| `player-shops.limit` | How many shops a player may own when they hold no numbered permission. |
| `player-shops.limit-permission` | The prefix of the numbered permission, `uxmshop.shops`. A player with `uxmshop.shops.10` may own ten and `uxmshop.shops.*` is no limit. |
| `player-shops.offers` | How many lines one shop may hold. |
| `player-shops.tax.percent` | Taken out of what the receiving side is paid, in both directions, so a payer never pays more than the price shown. |

A shop an operator opened is not counted against anybody's limit.

## Staff

`/myshop staff add <player>` lets another player restock and reprice. Staff never delete the shop and never
take the money. `/myshop staff remove <player>` takes them off.

## Shops with the server behind them

`/uxmshop adminshop`, while looking at a container, opens a shop where the server is the counterparty. It
never runs out and never fills up, and only the visitor's balance moves. It names no owner and is drawn with
`player-shops.hologram.admin-lines`.

## The hologram

The text above a shop is sent to the players near it and is never an entity on the server, so it is in no
entity count, no region file holds it, an entity sweep cannot reach it and nobody can pick it up. Each line
is its own display, the first highest.

| Key | Meaning |
|---|---|
| `enabled` | Draws the hologram at all. |
| `range` | How far away, in blocks, a player is sent it. |
| `refresh` | How often every shop is read again from the database. A trade redraws its own shop at once, so this is a safety net. |
| `frame` | How often one shop works out who is near and what its text says. It reads no database. A server with very many shops raises it. |
| `height` | How far above the container the first line sits. |
| `lines`, `admin-lines` | The text. A line written as `@key` is that line of the language files, read in each player's language. |

In a line, `<buy>...</buy>` disappears when the shop is not selling, `<sell>...</sell>` when it is not
buying, and `<separator>...</separator>` when either did. A line left with nothing in it is not drawn. The
names `<owner>`, `<item>`, `<stock>`, `<price-buy>` and `<price-sell>` are filled in.

## What is not guaranteed

Two buyers of the last item in one offer, clicking in the same instant, can both pass the stock check. The
overshoot is bounded by how many click at once. A trade that fails at any step leaves no money or item
moved.

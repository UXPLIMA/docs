---
title: Player shop commands
order: 303
description: Every /myshop branch, its node and what it does.
icon: store
---

| Command | Node | Does |
|---|---|---|
| `/myshop` | `uxmshop.myshop` | Opens your shop, or says you have none |
| `/myshop open` | `uxmshop.myshop` | Opens a shop behind the container you look at |
| `/myshop close` | none | Closes your shop and gives the stock back |
| `/myshop add <buy> <sell> <match>` | `uxmshop.myshop` | Adds the item in your hand as an offer |
| `/myshop stock <offer> <amount>` | `uxmshop.myshop` | Moves items from your inventory into an offer |
| `/myshop take <offer> <amount>` | `uxmshop.myshop` | Moves them back to you |
| `/myshop remove <offer>` | `uxmshop.myshop` | Takes an offer off the shop |
| `/myshop price <offer> <buy> <sell>` | `uxmshop.myshop` | Changes what an offer charges |
| `/myshop staff add <player>` | none | Lets a player restock and reprice |
| `/myshop staff remove <player>` | none | Takes them off |
| `/myshop list` | `uxmshop.myshop` | Opens the [list of every shop](../menus/player-shops.md) |
| `/myshop find` | `uxmshop.myshop` | Which shops trade what you are holding |
| `/myshop profits` | `uxmshop.myshop` | What your shops earned and spent, per item |
| `/myshop profits clear` | `uxmshop.myshop` | Starts the report again |

Every branch is for players only. `<match>` is one of the [matching modes](../features/item-matching.md).

Related: [Player shops](../features/player-shops.md)

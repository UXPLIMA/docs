---
title: "Reference"
order: 20
description: "Every command, permission, placeholder and file of uxmAuction."
---

## Commands

Names and aliases are the defaults from `commands.conf`, which renames or disables each of them.

| Command | Aliases | Subcommands |
|---|---|---|
| `/auction` | `/ah` | `mailbox`, `mine`, `bids`, `orders`, `history`, `watch`, `search`, `profile`, `sell`, `auction`, `reprice`, `buy`, `bid`, `order`, `fill`, `cancel`, `claim`, `lang`, `lang set`, `lang reset`, `lang set-player`, `lang reset-player` |
| `/uxmauction` | `/auctionadmin`, `/ahadmin` | `cancel`, `return`, `purge`, `log`, `restore`, `stats`, `lookup`, `lock`, `doctor`, `reload`, `version` |

## Permissions

| Permission | What it allows | Default |
|---|---|---|
| `uxmauction.language` | Read this plugin in a language of your own. | true |
| `uxmauction.admin.language` | Set or clear the language another player reads | op |
| `uxmauction.use` | Open the auction house | true |
| `uxmauction.sell` | Put an item up for sale | true |
| `uxmauction.auction` | Put an item up for bidding | true |
| `uxmauction.bid` | Bid on an item | true |
| `uxmauction.buy` | Buy an item outright | true |
| `uxmauction.order` | Ask the house to buy an item for you, and hold the money until it does | true |
| `uxmauction.fill` | Sell the item in your hand into somebody's order | true |
| `uxmauction.cancel` | Take your own listing back | true |
| `uxmauction.reprice` | Change the price of your own listing | true |
| `uxmauction.claim` | Take what you have won or what came back to you | true |
| `uxmauction.mailbox` | Open the mailbox the house keeps for you | true |
| `uxmauction.mine` | Read your own listings | true |
| `uxmauction.watch` | Watch a listing and be told when it moves | true |
| `uxmauction.search` | Search the house | true |
| `uxmauction.history` | Read what has sold | true |
| `uxmauction.profile` | Read a seller's profile | true |
| `uxmauction.admin.*` | Everything an operator does with this plugin | op |
| `uxmauction.admin.cancel` | Take somebody else's listing down | op |
| `uxmauction.admin.return` | Give a listing or a payment back to whoever it belongs to | op |
| `uxmauction.admin.restore` | Put back something the house took away | op |
| `uxmauction.admin.lock` | Stop the house taking new listings | op |
| `uxmauction.admin.purge` | Delete what a retention rule has aged out | op |
| `uxmauction.admin.lookup` | Read one listing in full, whoever owns it | op |
| `uxmauction.admin.log` | Read the audit trail | op |
| `uxmauction.admin.stats` | Read what the house has done | op |
| `uxmauction.admin.doctor` | Ask how this plugin is | op |
| `uxmauction.admin.version` | Read the version this server runs | op |
| `uxmauction.admin.reload` | Read the configuration again | op |

## Placeholders

The plugin registers a PlaceholderAPI expansion when PlaceholderAPI is installed.

## Files

The plugin writes these into `plugins/uxmAuction/` on first start and reads them again on reload:

- `categories.conf`
- `commands.conf`
- `config.conf`
- `currencies.conf`
- `menus/admin/log.conf`
- `menus/bids.conf`
- `menus/category.conf`
- `menus/history.conf`
- `menus/house.conf`
- `menus/listing.conf`
- `menus/mailbox.conf`
- `menus/mine.conf`
- `menus/order-create.conf`
- `menus/orders.conf`
- `menus/profile.conf`
- `menus/search.conf`
- `menus/sell.conf`
- `menus/watch.conf`
- `messages/messages_en.conf`
- `messages/messages_tr.conf`
- `theme.conf`

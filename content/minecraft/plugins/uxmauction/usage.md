---
title: "Guide"
order: 10
description: "How uxmAuction works, how to set it up and what each file holds."
---

uxmAuction is an auction house: players list items at a fixed price or for bids, place buy orders that the house fills from new listings, and collect what they are owed from a mailbox. It prices in several currencies at once and never converts between them.

## Install

1. Put the jar in `plugins/`.
2. Start the server once. The plugin writes its files into `plugins/uxmAuction/`.
3. Check `currencies.conf`: `coins` (Vault) and `experience` ship on. Turn on the ones your server runs.
4. Run `/uxmauction reload` after any change.

PlaceholderAPI, Vault and the other economy plugins are optional. A currency whose plugin is missing stays listed and says it is not ready; listings in it are refused until the plugin is installed.

## Files

| File | What it holds |
|---|---|
| `config.conf` | house rules, history, events, webhook, placeholders, chat questions, item lines, storage, network, fees, effects |
| `currencies.conf` | the currencies the house prices in and how amounts are written |
| `categories.conf` | the tabs of the house and which items fall into each |
| `menus/*.conf` | every window: house, category, listing, sell, search, mailbox, mine, bids, orders, watch, history, profile and the admin log |
| `commands.conf` | the name, aliases and on or off switch of every command |
| `messages/messages_<language>.conf` | the words players read |
| `theme.conf` | the colours of the messages |

## Selling and buying

- `/auction sell <price> [amount] [-t <time>] [-c <currency>] [-r]` lists the item in hand at a fixed price. Without a price it opens the sell window. `-r` (retail) lets buyers take part of the stack; otherwise a buyer takes all of it.
- `/auction auction <price> [-t <time>] [-c <currency>]` lists the item in hand for bids, starting at the price. A bidding listing is never sold in parts.
- `/auction buy <listing> [amount]` buys from a fixed price listing.
- `/auction bid <amount> <listing>` bids on an auction. The bid must be at least the leading bid plus the step of that currency.
- `/auction reprice <listing> <price>` lowers the price of your own listing.
- `/auction cancel <id>` takes your listing or order back, when `house.allow-cancelling` is on.

Times are written like `30s`, `5m`, `2d`. Without `-t` a listing stands for `house.listing.default-duration`; a longer time than `maximum-duration` is refused. Without `-c` the price is in the currency with the lowest `order`.

When a listing sells, the seller is paid straight into their balance if the currency can pay a player who is offline, and otherwise into their mailbox. An unsold item, an outbid refund and anything else the house owes goes to the mailbox: `/auction mailbox`, or `/auction claim <row>`.

## Buy orders

`/auction order <price> <amount> [-t <time>] [-c <currency>] [-m <material|enchanted|exact>] [-x]` asks the house to buy the item in hand for you and holds the money until it does. Without a price it opens the order window.

`-m` sets how closely a listing must match: `material` (any item of that type), `enchanted` (the same enchantments) or `exact` (the same item in every detail). A new listing at or below the order's price is offered to the best paying orders first and sells into them at once, unless the order was made with `-x` (manual). A seller can also sell the item in hand into an order with `/auction fill <order> [amount]`.

## House rules

| `house` key | Meaning |
|---|---|
| `locked` | start closed; afterwards the state lives in the database and `/uxmauction lock` changes it on every server |
| `allow-buying-your-own` | let a seller buy or bid on their own listing |
| `allow-cancelling` | let a seller take a listing back |
| `allow-retail` | let a seller offer a stack in parts |
| `max-listings-per-seller`, `max-orders-per-buyer`, `max-watches-per-player` | limits; `0` is no limit |
| `listing.default-duration`, `listing.maximum-duration` | how long a listing or order stands |
| `price.minimum.<currency>`, `price.maximum.<currency>` | the price band per currency; an unnamed currency has none |
| `bidding.step.<currency>` | the least a new bid must add to the leading one |
| `bidding.anti-snipe.window`, `.extension` | a bid inside the last `window` adds `extension` to the auction; a window of `0s` turns it off |
| `work.*` | the sweep that ends expired listings and finishes sales a crash left half done; the defaults suit most servers |

```hocon
house {
  max-listings-per-seller = 10
  price {
    minimum { coins = "1" }
    maximum { coins = "1000000000" }
  }
  bidding {
    step { coins = "10" }
    anti-snipe { window = "30s", extension = "1m" }
  }
}
```

## Fees

`fees` has four charges: `listing` (paid by the seller when the listing is written, not refunded on expiry), `reprice`, `sale-tax` (kept out of what the seller earns) and `purchase-tax` (paid by the buyer on top of the price). Each takes:

| Key | Meaning |
|---|---|
| `type` | `flat` for the same amount every time, `percent` for a share of the price |
| `value` | the amount, or the rate out of a hundred |
| `minimum`, `cap` | an optional floor and ceiling |
| `groups` | permission nodes with a rate of their own; a player with several pays the lowest |

A player who is offline holds no group node, so a seller paid while away pays the house rate. A sale tax never takes more than the sale brought. Everything ships at zero.

```hocon
fees {
  sale-tax {
    type = "percent"
    value = "5"
    cap = "1000"
    groups { "uxmauction.fee.vip" { value = "2" } }
  }
}
```

## Currencies

Each block in `currencies.conf` is one currency, keyed by its id. Every price, bid, fee and refund of a listing stays in the currency it was listed in.

| Key | Meaning |
|---|---|
| `type` | `vault`, `vaultunlocked`, `playerpoints`, `ecobits`, `treasury`, `item`, `experience`, `placeholder` or `bridge` |
| `order` | the tab order; the lowest is the default currency |
| `enabled` | turn it off rather than deleting it, so stored rows keep their meaning |
| `currency-name` | for `ecobits` and `treasury`, the currency inside that plugin |
| `pays-offline` | whether a seller who is away can be paid straight; otherwise the money waits in the mailbox |
| `material` | for `item`, the item counted |
| `unit` | for `experience`, `levels` or `points` |
| `display` | how an amount is written: `format`, `format-one`, `decimals`, `trim`, `thousands`, `point`, `short`, `short-suffixes`. A format may name a line of the messages files (`@currency.coins`), so each player reads it in their own language |

An `item` currency counts whole numbers only, so a fractional price is refused. A shown price is rounded up, never down. The file documents the `placeholder` and `bridge` types for economy plugins without a supported API.

## Categories

Each block in `categories.conf` is a tab: `order` (lowest first, and the winner when two tabs accept an item), `icon`, and `matches`, a list of item ids where `*` stands for any run of characters. An item no tab accepts goes to the `fallback` category.

## Notices

- `effects` holds a list of actions for `listed`, `listed-and-sold`, `bought`, `bid-leading`, `bid-outbid`, `bid-beaten`, `auction-sold`, `auction-won` and `mailbox-took`, in the same `[verb] payload` form as the other UXM plugins; `@line` is the plugin's own sentence for that moment.
- `webhook` posts new listings, sales and operator moves to a Discord webhook. Set `enabled` and `url`; `least.listing.<currency>` and `least.sale.<currency>` skip small events. Keep the address out of version control.
- `events.enabled` publishes Bukkit events for other plugins.
- `history` keeps one row per finished sale for `keep-for` (default `30d`); `/auction history` shows what the item in hand has sold for.

## Network

For several servers on one database, set `storage` to a shared `mysql`, `mariadb` or `postgresql` database and `network.transport` to `sql` (polls the database every `poll-every`) or `redis` (instant, needs a Redis server and the `redis` block). `server-name` may stay empty. `keep-for` is how long a network message is kept and must be longer than `poll-every`.

## Commands

| Command | Permission | What it does |
|---|---|---|
| `/auction` | `uxmauction.use` | open the house |
| `/auction sell`, `auction`, `buy`, `bid`, `reprice`, `cancel` | `uxmauction.sell`, `.auction`, `.buy`, `.bid`, `.reprice`, `.cancel` | see above |
| `/auction order`, `fill` | `uxmauction.order`, `uxmauction.fill` | buy orders |
| `/auction mailbox`, `claim <row>` | `uxmauction.mailbox`, `uxmauction.claim` | the mailbox |
| `/auction mine`, `bids`, `orders` | `uxmauction.mine` | your listings, leading bids and orders |
| `/auction watch` | `uxmauction.watch` | the items you are waiting for |
| `/auction search <text>` | `uxmauction.search` | find items by name |
| `/auction history` | `uxmauction.history` | what the item in hand has sold for |
| `/auction profile [player]` | `uxmauction.profile` | a seller's shelf and earnings |
| `/auction lang ...` | `uxmauction.language`, `uxmauction.admin.language` | languages |
| `/uxmauction cancel <id>` | `uxmauction.admin.cancel` | take anyone's listing or order down |
| `/uxmauction return <player>` | `uxmauction.admin.return` | give everything of one player back |
| `/uxmauction purge <days>` | `uxmauction.admin.purge` | delete what closed more than that many days ago |
| `/uxmauction log [player]` | `uxmauction.admin.log` | the newest `admin.log-lines` lines of the audit log |
| `/uxmauction restore <transaction>` | `uxmauction.admin.restore` | give the item of one logged line back |
| `/uxmauction lookup <listing>` | `uxmauction.admin.lookup` | everything about one listing |
| `/uxmauction stats` | `uxmauction.admin.stats` | the size of the house |
| `/uxmauction lock [on\|off]` | `uxmauction.admin.lock` | close or open the house on every server |
| `/uxmauction reload`, `version`, `doctor` | `uxmauction.admin.reload`, `.version`, `.doctor` | the usual operator commands |

The names and aliases come from `commands.conf`. The [reference](reference.md) lists the default aliases and every permission.

## Placeholders

The identifier is `placeholders.identifier`, `uxmauction` by default. Values are cached for `fresh-for`; `waiting` is shown before the first read and `none` for an item nobody sells.

| Placeholder | Value |
|---|---|
| `%uxmauction_listings%`, `%uxmauction_auctions%` | listings standing, and how many take bids |
| `%uxmauction_orders%`, `%uxmauction_mailbox%` | open buy orders, and mailbox rows waiting |
| `%uxmauction_my_listings%`, `_my_bids%`, `_my_orders%`, `_my_mailbox%`, `_my_sales%` | the reader's own counts |
| `%uxmauction_my_earnings_<currency>%` | what the reader earned in one currency |
| `%uxmauction_category_<name>%` | listings under one tab |
| `%uxmauction_top_price_<MATERIAL>%` | the most anybody asks for one material |

## Troubleshooting

- **A listing is refused because the currency is not ready.** Its plugin is not installed or not loaded; `/uxmauction doctor` says which.
- **A price with a fraction is refused.** The currency counts whole numbers (`item`, or a plugin that is whole).
- **Bids go up by one coin.** Set `house.bidding.step.<currency>`.
- **Nothing moves at all.** The house is locked; `/uxmauction lock off`.

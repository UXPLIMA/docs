---
title: Reference
order: 801
description: Every %uxmauction_...% placeholder, caching, and what waiting and none mean.
icon: list
---

PlaceholderAPI is required; the identifier is `placeholders.identifier`, `uxmauction` by default.

| Placeholder | Answer |
|---|---|
| `%uxmauction_listings%` | How many listings stand |
| `%uxmauction_auctions%` | How many of them take bids |
| `%uxmauction_orders%` | How many buy orders are open |
| `%uxmauction_mailbox%` | How many rows wait for their owners |
| `%uxmauction_my_listings%` | The reader's listings |
| `%uxmauction_my_bids%` | The bids the reader leads |
| `%uxmauction_my_orders%` | The reader's open orders |
| `%uxmauction_my_mailbox%` | What waits for the reader |
| `%uxmauction_my_sales%` | How many sales the reader finished |
| `%uxmauction_my_earnings_<currency>%` | What the reader earned in one currency |
| `%uxmauction_category_<name>%` | How many listings stand under one tab |
| `%uxmauction_top_price_<MATERIAL>%` | The most anybody asks for one material |

Nothing is counted on the thread that draws the board: the plugin answers with the number it has and reads the new one
behind the answer, every `fresh-for` (10s). Before the first read it answers `waiting` (`...`); for a thing nobody sells,
`none` (`-`). The `my_` names belong to whoever reads the board; asked with nobody looking, they answer nothing.

## Examples

```
Listings: %uxmauction_listings%
Your mailbox: %uxmauction_my_mailbox%
Diamonds go for: %uxmauction_top_price_DIAMOND%
```

Related: [config.conf](../config/config-conf.md#placeholders)

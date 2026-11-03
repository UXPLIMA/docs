---
title: config.conf
order: 601
description: Every key of config.conf, block by block, with its default and what it does.
icon: file-cog
---

## language

| Key | Default | Meaning |
|---|---|---|
| `default` | `"en"` | What a player reads when nothing else answers |
| `follow-client` | `true` | A player who chose nothing reads their client's language |
| `force` | `""` | One language for everybody |

## house

| Key | Default | Meaning |
|---|---|---|
| `locked` | `false` | Seeds the door of a house that no server ever closed or opened; the database holds it after |
| `allow-buying-your-own` | `false` | A seller may buy or bid on their own listing |
| `allow-cancelling` | `true` | A seller may take a listing back |
| `allow-retail` | `true` | A seller may offer a stack in parts |
| `max-listings-per-seller` | `0` | 0 is no limit |
| `max-orders-per-buyer` | `0` | 0 is no limit |
| `max-watches-per-player` | `50` | 0 is no limit |
| `listing.default-duration` | `"2d"` | When the seller names none |
| `listing.maximum-duration` | `"7d"` | The longest a listing or order may stand |
| `price.minimum.<currency>` | none | The floor, per currency |
| `price.maximum.<currency>` | none | The ceiling, per currency |
| `bidding.step.<currency>` | `coins = "1"` | The least a new bid must add |
| `bidding.anti-snipe.window` | `"30s"` | A bid inside it extends the auction; `0s` turns it off |
| `bidding.anti-snipe.extension` | `"1m"` | By how much |
| `work.expiry-batch-size` | `200` | |
| `work.auto-fill-candidates` | `20` | |
| `work.watches-told` | `20` | |
| `work.reconciliation-window` | `"5m"` | |
| `work.sweep-every` | `"1m"` | |

The `work` keys are in [Operators](../features/operators.md#the-timed-work).

## history, events

| Key | Default | Meaning |
|---|---|---|
| `history.enabled` | `true` | Keep a row per sale |
| `history.keep-for` | `"30d"` | `"0"` keeps nothing |
| `events.enabled` | `true` | Publish Bukkit events |

## webhook

`enabled`, `url`, `timeout`, `listing`, `sale`, `admin`, `least.listing.<currency>`, `least.sale.<currency>`; see
[Notices](../features/notices.md#the-discord-webhook).

## placeholders

| Key | Default | Meaning |
|---|---|---|
| `enabled` | `true` | Publish the placeholders |
| `identifier` | `"uxmauction"` | The name between the percent signs |
| `fresh-for` | `"10s"` | How long one number stays fresh; nothing is counted on the thread that draws the board |
| `waiting` | `"..."` | Before the first read |
| `none` | `"-"` | For a thing nobody sells |

## asking, item, admin

| Key | Default | Meaning |
|---|---|---|
| `asking.wait-for` | `"60s"` | How long a typed answer is waited for |
| `asking.stop-word` | `"cancel"` | Stops the question; case does not matter |
| `item.hide-lines` | sixteen components | The client lines kept quiet under a tile |
| `admin.log-lines` | `10` | Lines of the log one report shows |

## storage

| Key | Default | Meaning |
|---|---|---|
| `type` | `"sqlite"` | `sqlite`, `mysql`, `mariadb` or `postgresql` |
| `file` | `"auction.db"` | Inside `data/` |
| `host`, `port` | `"localhost"`, `0` | Port 0 is the server's own |
| `database`, `username`, `password` | `"minecraft"`, `"minecraft"`, `""` | |
| `table-prefix` | `"auction_"` | |
| `pool-size` | `10` | SQLite uses one |
| `local-only` | `false` | This one server keeps its own rows |
| `copy-from-file` | `false` | Copies the SQLite rows into an empty network database |

## network

`transport`, `server-name`, `poll-every`, `redis.*`, `keep-for`; see [A network](../features/network.md).

## fees

`listing`, `reprice`, `sale-tax`, `purchase-tax`, each with `type`, `value`, `minimum`, `cap`, `groups`; see
[Fees](../features/fees.md).

## effects

`listed`, `listed-and-sold`, `bought`, `bid-leading`, `bid-outbid`, `bid-beaten`, `auction-sold`, `auction-won`,
`mailbox-took`; see [Notices](../features/notices.md#effects).

## menu

`menu.sounds.click`, `page` and `denied`: `block.note_block.pling`, `item.book.page_turn`, `block.note_block.bass`.

Related: [Configuration](index.md)

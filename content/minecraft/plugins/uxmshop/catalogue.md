---
title: "The catalogue"
order: 20
description: "The catalogue is the half of the plugin where the server is the counterparty."
---

The catalogue is the half of the plugin where the server is the counterparty. The operator writes it in
`plugins/uxmShop/shops/<section>.conf`, and a player browses it with `/shop`, sells with `/sell` and asks
`/value` what an item is worth. The shipped `blocks.conf` and `tools.conf` explain the grammar in a comment
at the top of each file.

## Sections and lines

A section is a file. Its file name is the section id: the argument of `/shop open <section>`, and the name
in the permission `uxmshop.section.<section>`. Each file holds the section's own settings and a `lines`
block, one entry per product.

| Section key | Meaning |
|---|---|
| `order` | Where the section sits on the hub page. Needed because HOCON does not keep the order a block was written in. |
| `menu` | Which window in `menus/` draws the section. |
| `permission` | The node needed to open it. Left out, everybody may. |
| `buy-permission` | The node needed to buy from it. |
| `sell-permission` | The node needed to sell to it. A section with only a buy node is bought from and not sold to. |
| `currency` | Which currency of `currencies.conf` its lines price in. Left out, `shop.default-currency` applies. |
| `icon` | The tile the hub shows. |
| `name`, `description` | The section's own words. A plain string is one text for everybody; a block gives one per language, `name { any = "Tools", tr = "Aletler" }`. |
| `band` | The dynamic price band every line takes unless it names its own. |
| `rotation` | Shows `size` lines at a time and swaps them every `every`. The choice is worked out from the clock, so every server behind one proxy shows the same lines. |

| Line key | Meaning |
|---|---|
| `order` | Where the line sits on the page. |
| `type` | `item`, `command`, `shortcut`, `decoration` or `blank`. |
| `item` | What an item line hands over: `material`, `amount`, and optionally `name`, `lore`, `enchants`, `stored-enchants`, `custom-model-data`, `damage`, `unbreakable`, `potion`, `potion-effects`, `colour`, `firework`, `trim`, `head`, `spawner`, `patterns`, `model`. |
| `icon` | The face of the tile, when it differs from what is sold. |
| `buy` | What a player pays. Left out, the line cannot be bought. |
| `sell` | What a player is paid. Left out, the line cannot be sold. |
| `match` | How strictly an item a player sells is matched. See below. |
| `permission` | The node needed for this line. |
| `conditions` | What a player must meet, written as `"%placeholder% >= 10"`. |
| `actions` | What runs after a purchase. |
| `floor`, `ceiling` | The lowest and highest the price may reach. |
| `band` | A dynamic band for this line alone. |
| `stock` | Server stock and per player quotas. |
| `currency` | The currency of this line. |
| `name`, `description` | The line's own words. |

Run `/uxmshop iteminfo` with an item in your hand to get an `item` block you can paste.

A direction is turned off by leaving its price out; there is no second key to disagree with it. A `command`
line cannot be sold back, and a file that says otherwise is refused. A line that pays more than it charges
is refused when the file is read. An item field the plugin does not know is refused too, so a typo is never
mistaken for a setting that worked. The console lists every problem found at start and on reload, and
`/uxmshop doctor` asks again.

## Item matching

`match` is a value on each line, because one comparison cannot be right for cobblestone and for a named
sword at once.

| `match` | What it takes |
|---|---|
| `material` | Anything of that material, whatever has been done to it. |
| `material-and-name` | The material and the display name. |
| `full-meta` | Every field. |
| `full-meta-ignoring:damage,lore` | Every field except the ones named. |

The fields that may be ignored are `name`, `lore`, `enchants`, `stored-enchants`, `custom-model-data`,
`damage`, `unbreakable`, `potion`, `potion-effects`, `colour`, `firework`, `trim`, `head`, `spawner`,
`patterns` and `model`. Use the last form for tools, so a used pickaxe sells as a pickaxe. When two lines
accept the same item, the stricter one wins, so the answer never depends on file order.

## Prices

A number is a valid price on its own. Beyond that:

- **Dynamic band.** Four numbers on a line or a section: `move-per-trade` (how far one item moves the price),
  `ceiling` and `floor` (the limits, as multiples of the base), `normalisation` (how much of the distance
  home is walked each interval) and `normalisation-interval`. There is no expression language.
- **Ranks.** `shop.ranks` in `config.conf` multiplies a price by `buy` or `sell` for a player who holds the
  rank's permission. A player with two ranks takes the better one in each direction; they never stack.
- **Modifiers.** `/uxmshop modifier` gives one player a multiplier over everything, a section or a line. The
  narrowest one that names the trade wins; they never stack.
- **Order.** Base price, band, best rank, narrowest modifier, then the line's floor and ceiling, then the
  quantity, then tax.
- **Rounding.** A buy rounds up and a sell rounds down, to the places the currency holds.
- **Tax.** `shop.tax.percent` is taken out of what a player is paid for a sale and never added to what they
  pay, so a price on a tile is what is charged. A player with `shop.tax.bypass` (default
  `uxmshop.bypass.tax`) pays none.

## Stock and quotas

A line may limit each direction, and both limits ship off. A line that names neither is unlimited and
writes no counter.

```hocon
stock {
  buy {
    server { enabled = true, capacity = 640, refill = 64, every = "1h" }
    player { enabled = true, cap = 64, every = "24h" }
  }
}
```

`server` is a stock shared by everybody that refills by `refill` every `every`, up to `capacity`. `player`
is how much one player may trade per `every`. A trade has to pass every limit that is written. A window
that closed while the server was down is closed in one step. The counters are stored in the database, so
servers sharing a database share them.

The check and the write are two steps, so two players buying the last item in the same instant can both
pass. The overshoot is bounded by how many click at once, and the money is never at risk: a trade that fails
at any step leaves nothing moved. `/uxmshop resetstock` and `/uxmshop resetquota` put the counters back.

## Selling

| Command | What it does |
|---|---|
| `/sell`, `/sell hand` | The stack in your hand. |
| `/sell all` | Everything in the inventory the catalogue buys, up to `shop.sell-all-limit` items per call. |
| `/sell window` | A window to drop items into. They are handed back first, then what is left is sold. |
| `/value` | What the item in your hand is worth, in both directions. |

Every path goes through the same till, so a line permission, a condition, a quota and a band still hold. A
sale of a whole inventory is many trades, and the log records it as many.

## Blacklist

`blacklist.materials` in `config.conf` lists materials nobody may trade in either half. A player with
`uxmshop.bypass.blacklist.<material>` (the prefix is `blacklist.bypass`) is past it for that material.

## What a trade does

Each moment of a trade is a list in `config.conf` under `effects`, run through the uxmLib action engine.

| Moment | When | Filled in |
|---|---|---|
| `bought` | A purchase went through. | `<item>` `<amount>` `<price>` |
| `sold` | A sale went through. | `<item>` `<amount>` `<price>` |
| `taxed` | On top of the two above, when the trade was taxed. | `<tax>` |
| `cannot-afford` | The player has not got the money. | `<price>` `<balance>` |
| `out-of-stock` | The line has less than was asked for. | `<left>` |
| `quota-reached` | The player has traded their share for now. | `<cap>` `<time>` |
| `no-room` | The player has nowhere to put it. | none |

Text that starts with `@` names a line of `messages_<language>.conf`. The verbs are `[message]`,
`[broadcast]`, `[actionbar]`, `[title]`, `[subtitle]`, `[bossbar]`, `[particle]`, `[sound]`, `[effect]`,
`[console]` and `[player]`, each able to carry `{delay=, chance=, repeat=, every=, if=}`. An empty list is
silence. These run for every trade; the `actions` of a line run for that line only.

## Log

Every trade of both halves is a row in the database. `log.keep-days` (default 30, 0 keeps everything) is how
long a row is kept, and `/uxmshop prune <days>` deletes older rows by hand. The operator reads it with
`/uxmshop logs`; a player reads their profit report with `/myshop profits`.

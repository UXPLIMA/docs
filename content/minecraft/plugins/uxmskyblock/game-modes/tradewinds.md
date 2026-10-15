---
title: TradeWinds
order: 120
description: An island that is a trading vessel on a sea of its own, sailing between ports to buy
  low, sell high, carry contracts and climb the ranks.
icon: ship
---

A TradeWinds island is a trading vessel on a sea of its own. The team sails it between the ports
`modules/tradewinds.conf` names, buys low and sells high from the vessel's cargo hold, and climbs
the ranks.

## Turning it on

1. Set `enabled = true` in `modules/tradewinds.conf`.
2. Add `tradewinds` to `presets.modes` in `modules/presets.conf`.

The shipped preset `tradewinds` starts with `"uxm:vessel"`, which launches the vessel on its sea.
Players arrive on its deck.

## The sea

| Setting | Shipped | What it does |
| --- | --- | --- |
| `sea.radius` | `48` | How far the sea reaches from the vessel, each way, from 8 to 128 |
| `sea.depth` | `12` | How deep the sea is, from 2 to 64 |
| `sea.floor` | `SANDSTONE` | The block it lies on. One that does not fall |

## The cargo hold and the ranks

`/is cargo` opens the hold for anyone of the team aboard whose role lets them. A click moves a
stack between the hold and the player. The hold belongs to the vessel, not to whoever filled it.

`ranks` lists the ranks a vessel climbs as it trades, lowest first:

```hocon
ranks = [
    { id = "dinghy", volume = 0, hold-rows = 3 }
    { id = "sloop", volume = 500000, hold-rows = 4 }
]
```

- `id` names the rank here and in the language files, as `tradewinds.ranks.<id>`.
- `volume` is the trade, in the bank's minor units, a vessel must have done to hold the rank. The
  first is held from no trade at all.
- `hold-rows` is how many rows of nine its hold has, from 1 to 6, never fewer than the rank
  before.

The leaderboards rank vessels by the same trade, as the metric `uxm:tradewinds_trade`.

## Ports

`ports` lists the ports vessels sail between, in the order `/is sail` lists them. The name players
see is `tradewinds.ports.<id>` in every language file.

| Key | What it does |
| --- | --- |
| `id` | How this file and the language files name the port: a-z, 0-9, `_` and `-` |
| `icon` | The item the port is shown as |
| `voyage-seconds` | How long the voyage to it takes, from 0 to 86400 |
| `market` | What it trades, in the order its window shows it |
| `customs` | How often customs board a vessel arriving at a lawful port, from 0 to 1 |
| `takes-contraband` | `true` makes it a port that buys contraband |
| `contracts` | The deliveries it offers |

A market line is `{ item, lot, pays, asks }`. A price is per item in the bank's minor units, so
250 is 2.50. `pays` is what the port pays when the crew sells one, and `asks` what it asks when
the crew buys one. Leave either out and the port does not trade that way. `lot` is how many items
one click moves.

### Contraband and customs

`contraband.items` are the goods only ports that take contraband buy. Every other port is lawful:
it does not buy them, even when its market lists them. Customs seize every contraband item in the
hold and the vessel loses `contraband.standing-lost` of its standing in that port. They fire
`vessel-customs-seized`, or `vessel-customs-cleared` for a clean hold, in `modules/effects.conf`.

### Contracts

`/is contracts` takes and delivers the contracts of the port the vessel lies in. A contract is so
many of a good carried to another port within a time:

| Key | What it does |
| --- | --- |
| `id` | Its name, worded as `tradewinds.contracts.names.<id>` |
| `item`, `count` | The good, and how many, from 1 to 3456 |
| `to` | The port it is delivered to, not the one offering it |
| `within` | How long the crew has from taking it, from `"1m"` to `"7d"` |
| `preview` | The words its tile shows for its reward |
| `reward` | Effect lists: `deliverer` for the player who delivers it, `members` for every online member |

The goods leave the hold as the contract is delivered. One not delivered in time fails: its crew is
told, and `vessel-contract-failed` fires. A contract delivered or failed can be taken again.
`contracts-at-once` is how many one vessel holds at a time.

## How prices move

| Setting | Shipped | What it does |
| --- | --- | --- |
| `daily-demand.goods` | `1` | How many of the goods it buys each port wants more each day |
| `daily-demand.bonus` | `0.25` | How much more it pays for them, as a share of the price |
| `daily-demand.zone` | `UTC` | The zone a day begins at midnight in |
| `market-drift.per-item` | `0.001` | How much each item sold lowers, and each bought raises, that good's prices |
| `market-drift.floor`, `.ceiling` | `0.5`, `1.5` | How far prices may move from those written |
| `market-drift.half-life` | `6h` | How fast the push fades, so a market left alone comes back |
| `standing.step` | `50000` | The trade in a port that makes one step of standing there |
| `standing.percent` | `1` | How many percent better each step trades |
| `standing.max-steps` | `10` | The most steps that count |

The day's wanted goods are drawn from the port and the day alone, so every server of a network
wants the same. The market's push is kept, so a restart keeps the market. A port never pays more
than it asks.

## At sea

`voyage-events` rolls each kind once as the vessel sets sail, as often as its `chance` says.

- `storm`: the voyage takes `delay` longer, at most `"1d"`.
- `pirates`: they take `toll` from the island bank. When the bank cannot pay, `boarders` of
  `creature` come next to each of the crew on the vessel.
- `find`: a roll of the loot table `loot` goes into the hold, whole or not at all.

Each fires its list in `modules/effects.conf`: `vessel-storm`, `vessel-pirates-toll`,
`vessel-pirates-boarded`, `vessel-find` and `vessel-find-lost`.

### Piracy

Piracy is off as shipped. With `piracy.enabled`, a member whose role steers their vessel may take
`/is plunder` while both vessels are at sea, standing on the other vessel's water off its deck.
A plunder takes `share` of each stack in the other hold into the plunderer's hold, whole or not at
all. Both vessels wait `cooldown` before either plunders or is plundered again. Every plunder is
written down and said in the console, and fires `vessel-plunder` and `vessel-plundered`.

## Windows

`menus/vessel-cargo.conf`, `menus/tradewinds-sail.conf`, `menus/tradewinds-market.conf` and
`menus/tradewinds-contracts.conf`. A Bedrock player gets each as a form.

## Commands

| Command | What it does |
| --- | --- |
| `/is cargo` | Opens the cargo hold of the vessel you are aboard |
| `/is sail` | Chooses the port your vessel sails for next |
| `/is market` | Opens the market of the port your vessel lies in |
| `/is contracts` | Takes and delivers the contracts of the port your vessel lies in |
| `/is plunder` | Takes part of the hold of the vessel you stand by, at sea |
| `/isadmin tradewinds setrank <target> <rank>` | Gives a vessel a rank |
| `/isadmin tradewinds setstanding <target> <port> <standing>` | Sets a vessel's standing in a port |
| `/isadmin tradewinds moveport <target> <port>` | Puts a vessel in a port at once |

A rank is given by setting the vessel's trade to what the rank takes, so the next trade carries on
from there. Each admin verb asks `uxmskyblock.admin.tradewinds.<verb>`.

## Trading between players

Players on any mode can trade with `/is trade <player>`, from `modules/trade.conf`. Two players open
one window, `menus/player-trade.conf`, each puts in what they offer from their own inventory, and
the trade happens when both agree. What is offered stays in its owner's inventory until then, so
nothing is lost when a trade is called off or the server stops.

| Setting | Shipped | What it does |
| --- | --- | --- |
| `enabled` | `true` | Whether players can trade |
| `request-seconds` | `60` | How long a request waits for an answer, from 5 to 600 |
| `max-distance` | `-1` | How far apart two players may be, in blocks. Below zero for anywhere on the server |

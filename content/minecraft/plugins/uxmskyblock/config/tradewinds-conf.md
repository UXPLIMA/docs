---
title: tradewinds.conf
order: 650
description: "TradeWinds: the vessel, ports, markets, contracts and piracy."
icon: ship
---

TradeWinds: an island that is a trading vessel on a sea of its own. The team sails it between the ports this file names, buys low and sells high from the vessel's cargo hold, and climbs the ranks.

The file is `plugins/uxmSkyblock/modules/tradewinds.conf`. Every default below is the shipped value.

| Key | Default | What it does |
|---|---|---|
| `enabled` | `true` | Whether islands can be made as TradeWinds vessels. |

## `sea`

The sea a new vessel is launched on. The vessel floats on its surface and players arrive on its deck.

| Key | Default | What it does |
|---|---|---|
| `radius` | `48` | How far the sea reaches from the vessel, each way. From 8 to 128. |
| `depth` | `12` | How deep the sea is. From 2 to 64. |
| `floor` | `SANDSTONE` | The block the sea lies on. One that does not fall. |
| `ranks` | a list of 4 entries, below | The vessel's cargo hold, opened with /is cargo by anyone of the island's team aboard it whose role lets them. A click moves a stack between the hold and the player. The hold belongs to the vessel, not to whoever filled it. The ranks a vessel climbs as it trades, lowest first. id names a rank here and in the language files, as tradewinds.ranks.`<id>`. volume is the trade, in the bank's minor units, a vessel has done at the markets to hold the rank; the first is held from no trade at all. hold-rows is how many rows of nine slots its hold has, from 1 to 6, never fewer than the rank before. The leaderboards rank vessels by the same trade, as uxm:tradewinds_trade. |
| `ports` | a list of 3 entries, below | The ports vessels sail between, in the order /is sail lists them. /is market opens the market of the port the vessel lies in. Each entry is a port: id is how this file and the language files name it, from 1 to 64 of a-z, 0-9, _ and -, and the name players see is tradewinds.ports.`<id>` in every language file. voyage-seconds is how long the voyage to the port takes, from 0 to 86400. icon is the item the port is shown as. market is what the port trades, in the order its window shows it. A price is per item, in the bank's minor units, so 250 is 2.50. pays is what the port pays when the crew sells it one, and asks is what it asks when the crew buys one; leave either out and the port does not trade that way. A port asks more for an item than it pays for one. lot is how many items one click sells or buys. takes-contraband true makes the port one that buys contraband. Every other port is lawful: it does not buy contraband, even when its market lists it, and customs may board a vessel as it arrives there, as often as customs says, from 0 to 1. Customs seize every contraband item in the hold and the vessel loses standing in that port. They fire vessel-customs-seized, or vessel-customs-cleared for a clean hold, in modules/effects.conf. contracts are the deliveries the port offers, taken with /is contracts while the vessel lies in it. Each is so many of a good carried to another port of this file within a time: id how this file and the language files name it, as tradewinds.contracts.names.`<id>`, from 1 to 64 of a-z, 0-9, _ and -, and never the same as another contract's item the good, as the server names the item count how many of it, from 1 to 3456 to the port it is delivered to, not the one offering it within how long the crew has from taking it, from "1m" to "7d" preview the words its tile shows for its reward, plain words or a @key of the language files reward what runs when it is delivered, each an effect list as in modules/effects.conf: deliverer once for the player who delivers it, members for every member of the island who is online. %contract%, %item%, %count% and %port% say which and where. The goods leave the hold as the contract is delivered. One not delivered in time fails: its crew is told, and modules/effects.conf fires vessel-contract-failed for them. A contract delivered or failed can be taken again. |

`ranks`:

| # | `id` | `volume` | `hold-rows` |
|---|---|---|---|
| 1 | `dinghy` | `0` | `3` |
| 2 | `sloop` | `500000` | `4` |
| 3 | `brigantine` | `2500000` | `5` |
| 4 | `galleon` | `10000000` | `6` |

`ports`:

| # | `id` | `icon` | `voyage-seconds` | `customs` | `takes-contraband` |
|---|---|---|---|---|---|
| 1 | `emerald-bay` | `EMERALD` | `60` | `0.3` |  |
| 2 | `saltmarsh` | `PRISMARINE_SHARD` | `90` |  | `true` |
| 3 | `ironhaven` | `IRON_INGOT` | `120` |  | `true` |

## `daily-demand`

Each port wants some of the goods it buys more each day, and pays bonus more for them, as a share of the price: 0.25 is a quarter more, and never as much as the port asks. goods is how many, from 0 to 64, drawn afresh each day from the port and the day alone, so every server of a network wants the same. A day begins at midnight in zone. The market window marks the goods wanted today.

| Key | Default | What it does |
|---|---|---|
| `goods` | `1` |  |
| `bonus` | `0.25` |  |
| `zone` | `UTC` |  |

## `piracy`

Piracy, off as shipped. With enabled true a member whose role steers their vessel may take /is plunder while both their vessel and another are at sea, standing on the other vessel's water off its deck, the deck-radius blocks around its middle. A plunder takes share, from above 0 to 1, of each stack in the other hold, rounded down, into the plunderer's hold, whole or not at all. A vessel that plundered, or was plundered, waits cooldown before it does either again. Every plunder is written down and said in the console, and fires vessel-plunder and vessel-plundered in modules/effects.conf.

| Key | Default | What it does |
|---|---|---|
| `enabled` | `false` |  |
| `share` | `0.25` |  |
| `cooldown` | `30m` |  |
| `deck-radius` | `8` |  |

## `contraband`

The goods only ports that take contraband buy. standing-lost is what a vessel caught with them loses of its standing in the port whose customs caught it, in minor units, never below none.

| Key | Default | What it does |
|---|---|---|
| `items` | `GUNPOWDER` |  |
| `standing-lost` | `50000` |  |

## `voyage-events`

What may happen to a vessel at sea. Each kind is rolled once as the vessel sets sail, as often as its chance says, from 0 to 1, and comes at a moment of the voyage. A chance left out is 0. A voyage that ends another way, as when an operator moves the vessel, takes its events with it.

```text
storm     the voyage takes delay longer, at most "1d"
pirates   take toll from the island bank, in its minor units; when the bank cannot pay, or the toll
          is 0, boarders of creature come next to each of the crew standing on the vessel, from 0 to 16
find      a roll of the loot table of loot/ named loot goes into the hold, whole or not at all; a
          find the hold has no room for is lost
```

Each fires its list in modules/effects.conf: vessel-storm, vessel-pirates-toll, vessel-pirates-boarded, vessel-find and vessel-find-lost.

## `voyage-events.storm.pirates.find`

| Key | Default | What it does |
|---|---|---|
| `voyage-events.storm.pirates.contracts-at-once` | `3` | How many contracts one vessel holds at a time, from 1 to 54. |

## `voyage-events.storm.pirates.market-drift`

How a port's trade moves its prices, for every vessel alike. Each item the crews sell at a port lowers what it pays and asks for that good by per-item of the price, and each they buy raises both, never below floor or above ceiling of the prices written above. The push fades by half every half-life, so a market left alone comes back to its written prices. The push is kept, so a restart keeps the market. enabled = false keeps every price as written.

| Key | Default | What it does |
|---|---|---|
| `enabled` | `true` |  |
| `per-item` | `0.001` |  |
| `floor` | `0.5` |  |
| `ceiling` | `1.5` |  |
| `half-life` | `6h` |  |

## `voyage-events.storm.pirates.standing`

A vessel's standing in a port is the trade it did there. Every step of standing makes the port pay that many percent more for what the crew sells and ask that much less for what it buys, up to max-steps steps and never so far that the port pays more than it asks. step is in minor units.

| Key | Default | What it does |
|---|---|---|
| `step` | `50000` |  |
| `percent` | `1` |  |
| `max-steps` | `10` |  |

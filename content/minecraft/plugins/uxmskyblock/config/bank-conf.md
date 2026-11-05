---
title: bank.conf
order: 613
description: "The island bank: currencies, interest, refunds and upkeep."
icon: landmark
---

The file is `plugins/uxmSkyblock/modules/bank.conf`. Every default below is the shipped value.

## `bank`

The island bank: money its members share, interest, refunds and the upkeep an island pays.

| Key | Default | What it does |
|---|---|---|
| `operation-retention` | `30d` | How long a settled idempotency record is kept. Every deposit, withdrawal, upgrade purchase, shop trade and upkeep charge writes one so a retry of it is answered rather than applied twice, and it has to outlive every retry anything can make. A record that never settled is never swept, whatever this says. |

## `bank.interest`

Interest on the money in an island bank, paid into it once each interval to an island one of whose members played lately. It never fills a bank past the limit of its bank limit upgrade.

| Key | Default | What it does |
|---|---|---|
| `enabled` | `false` |  |
| `interval` | `24h` | How often an island is paid, at most once in each |
| `percent` | `1.0` | The share of the bank's money paid, in percent |
| `active-within` | `72h` | How lately a member must have played for the island to be paid |
| `max-per-payment` | `0` | The most one payment gives, in money; 0 for no most |

## `bank.delete-refund`

When an island is deleted, the share of its bank's money paid back to its owner's own wallet, from 0 for nothing to 1 for all of it.

| Key | Default | What it does |
|---|---|---|
| `share` | `0.0` |  |

## `bank.upkeep`

Upkeep fee and bankruptcy escalation system

| Key | Default | What it does |
|---|---|---|
| `enabled` | `false` | Whether recurring upkeep debiting is active |
| `interval` | `24h` | Cycle interval between upkeep runs (e.g. 24h) |
| `base-fee` | `500.00` | Base flat fee per cycle (currency units, e.g. 500.00 = 50000 minor units) |
| `per-member-fee` | `100.00` | Additional fee per registered island member (currency units, e.g. 100.00 = 10000 minor units) |
| `grace-duration` | `72h` | Duration before an island in GRACE escalates to LOCKED lockout |
| `auto-remediate-on-deposit` | `true` | Automatically remediate accumulated debt upon bank deposit if funds suffice |

## `bank.money`

The island's own money, as the bank window draws it. It is always kept, so it has no "enabled", and it stands among the currencies below as one of them.

```text
display-name  its name, a key of the language files written with a leading @, or your words
icon          the item its tile wears
order         where it stands among the currencies, the lowest first
quick         the amounts its window moves in one click. The window shows the first two.
format        how an amount of it is written in every line and window. "economy" lets the
              economy plugin write it, with its own sign, as its own commands do. Or write a
              pattern with <amount> where the number goes: "$<amount>", "<amount> coins".
```

| Key | Default | What it does |
|---|---|---|
| `display-name` | `@bank.currencies.money` |  |
| `icon` | `GOLD_INGOT` |  |
| `order` | `0` |  |
| `quick` | `100`, `1000` |  |
| `format` | `economy` |  |

## `bank.currencies`

The currencies an island bank keeps beside the island's own money.

The island's own money is the server's economy, read through Vault: it is what upgrades, the shop and upkeep spend, and it is always kept. Everything listed here is kept too, so members can put their experience, their points or a stack of diamonds away together and take them out again. Each is kept in whole numbers.

The id of a currency is what the bank keeps it under, and what a player writes after an amount: /is bank deposit 50 experience. It is lower case letters, digits, _ and -, and it may not be primary, crystals, exp or vault, which the bank keeps for itself. Once an island holds some of a currency, keep its id: turn it off with "enabled = false" rather than renaming or deleting it, so what the islands hold still has a name.

Every currency takes:

```text
type          where it lives, one of the nine below
order         where it stands among the others in the bank window, the lowest first
enabled       whether islands can use it now
display-name  its name, a key of the language files written with a leading @, or your
              own words
icon          the item its tile in the bank window wears
quick         the amounts its window moves in one click, [10, 100] when left out. The
              window shows the first two
```

Nine types:

```text
vault           the Vault economy, for a second balance on a server that runs two
vaultunlocked   VaultUnlocked, the modern Vault. currency-name picks one of its currencies
playerpoints    PlayerPoints
ecobits         EcoBits. currency-name is the id you gave the currency in EcoBits
treasury        Treasury. currency-name picks one of its currencies, wait-for how long a
                call may take before the bank gives up on it, "10s" when left out
item            an item in the inventory, as material = "minecraft:diamond". Only the
                plain item counts, never a renamed or enchanted one
experience      the player's experience, unit = "points" or "levels". Points are the fair
                count: a level is worth more the higher it is
bridge          any other economy plugin, described by hand. See the example at the end
placeholder     a balance read through a placeholder and moved by two console lines
```

A currency whose plugin is not installed, or which is described wrongly, stays out of the bank and the console says why. The rest of the bank is not touched. So every currency below ships turned on: points shows only on a server that runs PlayerPoints.

Experience and items live in the player, so they move only while the player is online, which is always the case for the player putting them in or taking them out.

## `bank.currencies.experience`

| Key | Default | What it does |
|---|---|---|
| `type` | `experience` |  |
| `order` | `10` |  |
| `enabled` | `true` |  |
| `display-name` | `@bank.currencies.experience` |  |
| `icon` | `EXPERIENCE_BOTTLE` |  |
| `unit` | `points` |  |
| `quick` | `10`, `100` |  |
| `bank.currencies.points.type` | `playerpoints` |  |
| `bank.currencies.points.order` | `20` |  |
| `bank.currencies.points.enabled` | `true` |  |
| `bank.currencies.points.display-name` | `@bank.currencies.points` |  |
| `bank.currencies.points.icon` | `AMETHYST_SHARD` |  |
| `bank.currencies.points.quick` | `10`, `100` |  |
| `bank.currencies.diamonds.type` | `item` |  |
| `bank.currencies.diamonds.order` | `30` |  |
| `bank.currencies.diamonds.enabled` | `true` |  |
| `bank.currencies.diamonds.display-name` | `@bank.currencies.diamonds` |  |
| `bank.currencies.diamonds.icon` | `DIAMOND` |  |
| `bank.currencies.diamonds.material` | `minecraft:diamond` |  |
| `bank.currencies.diamonds.quick` | `1`, `16` |  |

---
title: Currencies
order: 409
description: The nine currency types, the shipped four, placeholder and bridge currencies, and how an amount is written.
icon: coins
---

A line, a section or a player shop prices in one currency, named by its id in `currencies.conf`. Nothing is ever
converted: a price, a tax and a refund stay in the currency of the line.

Each entry has a `type`, an `order` and `enabled`. The lowest `order` is the first tab of a window and the currency a
price with no name is written in. Turn a currency off with `enabled = false` rather than deleting it, so the ids in the
database still mean something. A currency whose plugin is missing answers that it is not ready, and the shop refuses to
price in it.

## The nine types

| Type | Is | Extra keys |
|---|---|---|
| `vault` | The Vault economy; one balance, no name | `pays-offline` |
| `vaultunlocked` | VaultUnlocked; moves a decimal, so a large price keeps its last penny. Prefer it where installed | |
| `playerpoints` | PlayerPoints | |
| `ecobits` | EcoBits, with several currencies | `currency-name` |
| `treasury` | Treasury, with several currencies; each call is bounded | `currency-name`, `wait-for` |
| `item` | An item in the inventory, in whole numbers | `material` |
| `experience` | The player's experience | `unit`: `levels` or `points` |
| `placeholder` | A balance read through a placeholder and moved by two commands | `placeholder`, `take`, `give`, `thousands`, `pays-offline`, `whole` |
| `bridge` | Any other economy plugin, described by hand | see below |

## Shipped

| Id | Type | Order | On |
|---|---|---|---|
| `coins` | `vault` | 10 | yes |
| `points` | `playerpoints` | 20 | no |
| `diamonds` | `item`, `minecraft:diamond` | 30 | no |
| `experience` | `experience`, points | 40 | yes |

`shop.default-currency` (`coins`) is the currency of a section that names none and of every player shop; it must be on.
The shipped sections price in `experience`.

## Item and experience

They live in the player, so they are read and written on the thread that owns the player. A player who is away holds
neither, so a player shop cannot price in one. The catalogue may, because the counterparty is the server.

## Placeholder

For an economy with no API: the shop asks the placeholder for the balance and sends the `give` and `take` lines, with
`{player}`, `{uuid}` and `{amount}` filled in. A command answers nothing, so the shop reads the balance first and refuses
a take the player cannot afford, then trusts the line. A balance that is not a number reads as zero. PlaceholderAPI
must be installed. A line naming a player the server has never seen, or a name an account could not have, is not sent.

## Bridge

A description of an economy plugin's own methods, so no code is compiled against it: `plugin`, `class`, `access`
(`service`, `static`, `plugin` or `class`), `accessor`, `balance`, `take`, `give`, `argument` (`offline-player`,
`player-id` or `player-name`), `answer` (`boolean`, `vault-response` or `nothing`), `whole`, and for several balances
`currencies` (`one`, `by-name` or `by-object`), `currency-name`, `currency-class`, `currency-lookup`, `take-negates`
and `naming`. The amount, currency and player are placed by reading the method's parameter types. A wrong method name
turns that bridge off and the console says so. The file ends with a commented example.

## Paying an owner who is away

`pays-offline` (default true) says whether the plugin can pay an offline player. A currency that cannot is refused for
player shops where the owner chooses it; nothing is held for later.

## How an amount is written

| Key in `display` | Meaning |
|---|---|
| `format` | `{amount}` and `{currency}`; `@currency.coins` writes it in each reader's language |
| `format-one` | The same, for exactly one |
| `decimals` | Places a fraction may take |
| `trim` | Drop a fraction of zeros, so ten is `10` |
| `thousands`, `point` | The group and decimal marks |
| `short`, `short-suffixes` | Shorten large numbers, so 1250 is `1.2k`; suffixes from the first thousand up |

`gui`, `chat` and `hologram` blocks inside `display` write over it for one surface, naming only what they change. The
shipped `coins` shortens in windows and holograms with one decimal.

Related: [currencies.conf](../config/currencies-conf.md)

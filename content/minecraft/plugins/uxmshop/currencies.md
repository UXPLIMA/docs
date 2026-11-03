---
title: "Currencies"
order: 30
description: "A catalogue line, a section or a player shop prices in one currency, named by its id in `plugins/uxmShop/currencies.conf`."
---

A catalogue line, a section or a player shop prices in one currency, named by its id in
`plugins/uxmShop/currencies.conf`. Nothing is ever converted: a price, a tax and a refund stay in the
currency of the line. The shipped file explains every key in comments.

Each entry under `currencies` has a `type`, an `order` and an `enabled` switch. HOCON does not keep the
order a block was written in, so the lowest `order` is the first tab of a menu and the currency a price with
no name is written in. Turn a currency off with `enabled = false` rather than deleting it, so the ids in the
database still mean something. A currency whose plugin is not installed stays in the list and answers that
it is not ready; the shop refuses to price in it and says why.

## The nine types

| Type | What it is | Extra keys |
|---|---|---|
| `vault` | The Vault economy. Holds one balance, so it takes no name. | `pays-offline` |
| `vaultunlocked` | VaultUnlocked. Moves a decimal, so a large price keeps its last penny. Prefer it where installed. | |
| `playerpoints` | PlayerPoints. | |
| `ecobits` | EcoBits, which holds several currencies. | `currency-name` |
| `treasury` | Treasury, which holds several currencies. Each call is bounded. | `currency-name`, `wait-for` |
| `item` | An item in the inventory, counted in whole numbers. | `material` |
| `experience` | The experience of the player. | `unit` is `levels` or `points` |
| `placeholder` | A balance read through a placeholder and moved by two commands. | `placeholder`, `take`, `give`, `thousands`, `pays-offline`, `whole` |
| `bridge` | Any other economy plugin, described by hand. | see below |

The shipped file turns on `coins` (Vault, order 10) and `experience` (order 40), and ships `points`
(PlayerPoints) and `diamonds` (an item) switched off. The shipped catalogue sections price in
`experience`, so a bare server trades. Point sections at `coins` once the server runs an economy.

`shop.default-currency` in `config.conf` is the currency a section uses when it names none, and the one a
player shop uses. It must be turned on in `currencies.conf`.

### Item and experience

These live in the player, so they can only be read and written on the thread that owns the player. The shop
hops over and comes back. A player who is away holds neither, so a player shop cannot price in one: the
owner would have to be present to be paid. The catalogue may, because the counterparty is the server.

### Placeholder

`placeholder` is for an economy with no API: the shop asks the placeholder for the balance and sends the
`give` and `take` lines. `{player}`, `{uuid}` and `{amount}` are filled in. A command answers nothing, so
the shop cannot learn that a take failed: it reads the balance first and refuses a take the player cannot
afford, then trusts the line. A balance that is not a number is read as zero. PlaceholderAPI must be
installed. A line that names a player the server has never seen, or whose name an account could not have, is
not sent.

### Bridge

A bridge is a description of an economy plugin's own methods, so no code is compiled against it. The keys:
`plugin`, `class`, `access` (`service`, `static`, `plugin` or `class`), `accessor`, `balance`, `take`,
`give`, `argument` (`offline-player`, `player-id` or `player-name`), `answer` (`boolean`, `vault-response`
or `nothing`), `whole`, and for plugins with several balances `currencies` (`one`, `by-name` or
`by-object`), `currency-name`, `currency-class`, `currency-lookup`, `take-negates` and `naming`. The amount,
currency and player are placed in a call by reading the method's parameter types. A wrong method name turns
that bridge off and the console says so; it never stops the server. The file ends with a commented example.

## How an amount is written

Each currency may carry a `display` block.

| Key | Meaning |
|---|---|
| `format` | Where the number and the name go. `{amount}` is the number and `{currency}` is the id. A line of the messages files (`@currency.coins`) writes it in each player's own language. |
| `format-one` | The same, for exactly one. |
| `decimals` | How many places a fraction may take. |
| `trim` | Drop a fraction of zeros, so ten is `10` and not `10.00`. |
| `thousands` | What goes between the groups of three. |
| `point` | What goes before the fraction. |
| `short` | Shorten large numbers, so 1250 is `1.2k`. |
| `short-suffixes` | The short forms, from the first thousand upwards. |

Three blocks inside `display`, `gui`, `chat` and `hologram`, write over it for one surface, naming only what
they change. A number with more places than `decimals` allows is rounded up, so a price shown is never under
the price charged.

## Player shops need an economy that pays an absent owner

A player shop may only price in a currency that can pay a player who is offline. The refusal happens where an
owner chooses a currency, and it says why. `pays-offline` on a currency states whether the plugin can pay an
offline player; when it cannot, the money is left in a mailbox instead and nothing is lost.

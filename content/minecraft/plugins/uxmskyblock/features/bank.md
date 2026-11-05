---
title: Bank
order: 403
description: Currencies, deposits and withdrawals, interest, upkeep and debt.
icon: landmark
---

The bank is what the team shares. Upgrades, the island shop, chunk prices, Boxed growth and upkeep spend its money;
a visit fee pays into it.

## Currencies

The island's own money is the server's economy, read through Vault. Beside it the bank keeps the currencies in
[bank.conf](../config/bank-conf.md), each in whole numbers:

| Type | Where it lives |
|---|---|
| `vault` | A second Vault economy |
| `vaultunlocked` | VaultUnlocked, one of its currencies |
| `playerpoints` | PlayerPoints |
| `ecobits` | EcoBits |
| `treasury` | Treasury, one of its currencies |
| `item` | A plain item in the inventory, such as diamonds |
| `experience` | The player's experience, in points or levels |
| `bridge` | Any other economy plugin, described by hand |
| `placeholder` | A balance read through a placeholder and moved by two console lines |

A currency whose plugin is not installed stays out of the bank and the console says why. The island also earns
crystals, a currency of its own that missions pay.

## Moving money

`/is bank deposit <amount> [currency]` and `/is bank withdraw <amount> [currency]`, or the
[bank window](../menus/economy.md#bank). `all` moves everything you carry of a currency. Depositing needs the island
permission Put money in the bank; withdrawing needs Take money from the bank. Every move is recorded once, so a retry
after a lag is answered rather than applied twice. How much the bank holds is its [Bank Limit upgrade](upgrades.md).

## Interest

Off as shipped. When on, an island one of whose members played lately is paid a share of its money once each
interval, never past its bank limit.

## Upkeep and debt

Off as shipped. When on, an island pays a base fee and a fee per member each cycle. An island that cannot pay goes
into debt, then into grace, then is locked when grace runs out. `/is bank status` shows the state and
`/is bank paydebt` pays it. A deposit pays the debt first when `auto-remediate-on-deposit` is on.

## When an island is deleted

`bank.delete-refund.share` pays a share of the bank's money back to the owner, 0 as shipped.

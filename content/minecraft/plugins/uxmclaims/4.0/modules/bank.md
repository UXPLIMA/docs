---
title: Bank
order: 1323
description: A shared balance for each town, nation and, optionally, land, with deposits, withdrawals and a history.
icon: landmark
---

The bank module gives every town and nation an account its members pay into and its leaders spend from.
Taxes, upkeep and other modules take their money from these accounts. Personal lands can have a bank too, but
that is off unless you turn it on.

Module `bank` · disabled by default · `modules/bank/config.yml` · requires [Economy](economy.md), [Lands](lands.md)

## Commands

| Command | What it does | Permission |
|---|---|---|
| `/town bank` | Opens your town's bank menu. | `uxmclaims.command.bank.use` |
| `/town bank balance` | Shows your town's balance. | `uxmclaims.command.bank.use` |
| `/town bank deposit <amount>` | Pays money from your wallet into your town's bank. | `uxmclaims.command.bank.use` |
| `/town bank withdraw <amount>` | Takes money from your town's bank into your wallet. | `uxmclaims.command.bank.use` |
| `/town bank history` | Shows your town's bank history. | `uxmclaims.command.bank.use` |
| `/nation bank [balance\|deposit\|withdraw\|history]` | The same for your nation. | `uxmclaims.command.bank.use` |
| `/claim bank [balance\|deposit\|withdraw\|history]` | The same for the land you stand in. Only with `holders.land: true`. | `uxmclaims.command.bank.use` |
| `/uxm bank <holder> balance` | Shows any bank's balance. | `uxmclaims.admin.bank` |
| `/uxm bank <holder> set <amount>` | Sets a bank's balance. | `uxmclaims.admin.bank` |
| `/uxm bank <holder> add <amount>` | Adds money to a bank. | `uxmclaims.admin.bank` |
| `/uxm bank <holder> take <amount>` | Takes money from a bank. | `uxmclaims.admin.bank` |
| `/uxm bank balance\|set\|add\|take <land\|town\|nation> <holder> [amount]` | The same with the holder type named, for a name several kinds share. | `uxmclaims.admin.bank` |

The `/town` commands need [Towns](towns.md) and the `/nation` commands need [Nations](nations.md).

### Who may do what

| Action | Land bank | Town or nation bank |
|---|---|---|
| See balance and history | Members, and anyone who may deposit or withdraw | Residents of the town or towns of the nation |
| Deposit | The owner, or a member whose role or override allows `bank:deposit` | The mayor or leader, or a rank with `bank:deposit` |
| Withdraw | The owner, or a member whose role or override allows `bank:withdraw` | The mayor or leader, or a rank with `bank:withdraw` |

For a land owned by a town or nation, the people who run that town or nation count as the owner. Holders of
`uxmclaims.admin.bank` may use every bank.

## Permissions

| Node | Default | Grants |
|---|---|---|
| `uxmclaims.command.bank.use` | everyone | `/claim bank`, `/town bank`, `/nation bank`. |
| `uxmclaims.admin.bank` | op | Using every bank and changing balances with `/uxm bank`. |

| Role permission | Given to | Grants |
|---|---|---|
| `bank:deposit` | owner, member | Paying into the bank. |
| `bank:withdraw` | owner | Taking money out of the bank. |

## Settings

| Key | Default | What it does |
|---|---|---|
| `holders.town` | `true` | Towns have a bank. |
| `holders.nation` | `true` | Nations have a bank. |
| `holders.land` | `false` | Personal lands have a bank. Land upkeep and land member tax in [Taxes](taxes.md) need this. |
| `max-balance.town` | `0` | Highest town balance. `0` is no cap. |
| `max-balance.nation` | `0` | Highest nation balance. `0` is no cap. |
| `max-balance.land` | `0` | Highest land balance. `0` is no cap. |
| `min-amount` | `1` | Smallest amount for one deposit or withdrawal. |
| `history.keep-days` | `90` | Days a history line is kept. `0` keeps them forever. |
| `history.menu-lines` | `135` | Lines loaded into the history menu. |
| `deleted-holder-balance` | `OWNER` | What happens to the money of a deleted holder: `OWNER`, `NATION` or `VOID`. See below. |
| `notify-owner` | `true` | Tell the holder's owner when someone else deposits or withdraws, through the [Inbox](inbox.md) when it is on. |
| `personal-fallback` | `false` | When a personal land's bank cannot pay a charge such as land upkeep, take the rest from the owner's own balance. |

## When a holder is deleted

| Deleted | `OWNER` | `NATION` | `VOID` |
|---|---|---|---|
| Player's land | To the owner's wallet | To the owner's wallet | Gone |
| Town or nation land | To the town or nation bank | To the nation bank above it, else the town bank | Gone |
| Town | To the mayor's wallet | To its nation's bank, else the mayor's wallet | Gone |
| Nation | To the leader's wallet | To the leader's wallet | Gone |

## Placeholders

| Placeholder | Renders |
|---|---|
| `%uxmclaims_bank_town%` | Your town's balance, formatted by the economy. |
| `%uxmclaims_bank_nation%` | Your nation's balance, formatted by the economy. |
| `%uxmclaims_bank_land%` | The balance of your selected land, the land you stand in or your only land. |
| `%uxmclaims_bank_<holder>_raw%` | The same as a plain number, such as `%uxmclaims_bank_town_raw%`. |

A placeholder is empty when you have no such holder or banks are off for that kind.

## Notes

- `holders.town: false` or `holders.nation: false` keeps the commands registered, but they answer that banks
  are off for that kind of holder.
- In `/uxm bank <holder> ...`, put a holder name in quotes when its first word is `balance`, `set`, `add` or
  `take`.
- Some town and nation actions pay from the bank and then save the change separately. A crash between a failed
  save and its refund can lose the refund; the history shows it.
- A land brought back with the lands module's restore does not get its bank back: its money was already paid
  out by `deleted-holder-balance`.

Related: [Economy](economy.md), [Taxes](taxes.md), [Towns](towns.md), [Nations](nations.md)

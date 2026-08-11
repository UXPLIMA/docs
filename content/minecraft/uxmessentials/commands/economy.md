---
title: Economy & Banking
order: 980
---

## Economy

The `economy` module is a DB-backed, **multi-currency** wallet system. Balances live in the database (never in player data), so they survive world rollbacks. Out of the box it ships a single default currency, but it can run several side by side.

The core commands (`/balance`, `/pay`, `/baltop`, and the base `/eco` admin verbs) are always available. A second tier of features is **gated behind flags in `economy.conf`** and marked with a **†** below: they only register when the matching flag is switched on.

<Callout type="info" title="Multi-currency">

Where a command accepts a trailing `[currency]` argument, you can target any configured currency by its id (e.g. `coins`, `gems`, `tokens`). Omitting it uses the default currency. When a currency has `permission-required = true`, using it also needs the per-currency gate `uxmessentials.economy.currency.<id>`.

</Callout>

---

## Balances & Payments

| Command | Aliases | Description | Permission |
|---------|---------|-------------|------------|
| `/balance [player] [currency]` | `bal`, `money` | Show your own or another player's balance | `uxmessentials.economy.balance` / `.balance.others` |
| `/pay <player> <amount> [currency]` | - | Transfer funds to a player | `uxmessentials.economy.pay` |
| `/payconfirm` | - | Confirm a large `/pay` above the threshold | `uxmessentials.economy.pay` |
| `/payall <amount> [currency]` | - | Pay every online player | `uxmessentials.economy.payall` |
| `/paytoggle` | - | Refuse incoming `/pay` transfers | `uxmessentials.economy.pay.toggle` |
| `/baltop [currency] [page]` | `balancetop` | View the top balances | `uxmessentials.economy.baltop` |
| `/wallet [currency]` † | - | Visual wallet dashboard | `uxmessentials.economy.wallet` |

### Examples

```
/balance
/balance Steve
/balance Steve gems
/pay Alex 500
/pay Alex 500 gems
/baltop
/baltop gems 2
```

---

## Selling & Item Worth

| Command | Description | Permission |
|---------|-------------|------------|
| `/worth [item]` | Report the sell value of an item | `uxmessentials.economy.worth` |
| `/setworth [item] <price>\|clear` | Set or clear an item's sell value | `uxmessentials.economy.setworth` |
| `/sell [amount]` | Sell the held item | `uxmessentials.economy.sell` |
| `/sellall` † | Sell all sellable items in your inventory | `uxmessentials.economy.sell` |

### Examples

```
/worth
/setworth diamond 100
/sell
/sell 16
/sellall
```

---

## Physical Money, Banks, Loans & Exchange

These convert virtual balance to physical notes, manage shared accounts, and move value between currencies. All of them are **†** flag-gated in `economy.conf`.

| Command | Description | Permission |
|---------|-------------|------------|
| `/withdraw <amount>` † | Turn virtual money into a physical banknote | `uxmessentials.economy.withdraw` |
| `/deposit` † | Cash a physical banknote back into your balance | `uxmessentials.economy.deposit` |
| `/bank [create\|deposit\|withdraw\|addmember\|removemember …]` † | Shared-bank accounts | `uxmessentials.economy.bank` |
| `/loan [status\|take\|pay …]` † | Loans and credit-rating | `uxmessentials.economy.loan` |
| `/exchange [<amount> <src> <dst>]` † | Convert between currencies | `uxmessentials.economy.exchange` |

### Examples

```
/withdraw 1000
/deposit
/bank create
/bank deposit 500
/loan take 2000
/exchange 100 coins gems
```

---

## Admin: The `/eco` Subtree

`/eco` (alias `economy`) is the staff economy toolkit. The bare command opens the admin GUI hub; the core mutation verbs are always on, while the extra subcommands are **†** flag-gated in `economy.conf`.

| Command | Description | Permission |
|---------|-------------|------------|
| `/eco` | Open the admin economy GUI hub | `uxmessentials.economy.admin` |
| `/eco give\|give-random <player> <amount…>` | Give a fixed amount / random range | `uxmessentials.economy.admin.give` |
| `/eco take <player> <amount>` | Take money | `uxmessentials.economy.admin.take` |
| `/eco set\|reset <player> [amount]` | Set / reset a balance | `uxmessentials.economy.admin.set` |
| `/eco giveall\|giverandom\|resetall …` | Bulk server-wide mutations | `uxmessentials.economy.admin.bulk` |
| `/eco history [player]` | Transaction-history GUI | `uxmessentials.economy.admin.history` |
| `/eco logs` | Global economy log GUI | `uxmessentials.economy.admin.logs` |
| `/eco note give <player> <amount>` † | Mint an admin banknote (no debit) | `uxmessentials.economy.admin.note` |
| `/eco backup\|export` † | Snapshot / CSV export | `uxmessentials.economy.admin.backup` |
| `/eco restore <player> <date>` † | Restore a player's balance | `uxmessentials.economy.admin.restore` |

### Examples

```
/eco give Steve 1000
/eco give-random Steve 100 500
/eco take Steve 250
/eco set Steve 0
/eco giveall 100
/eco history Steve
/eco note give Steve 5000
```

---

## Bypass & Exempt Nodes

| Node | Purpose |
|------|---------|
| `uxmessentials.economy.currency.<id>` | Use a specific currency (when it requires permission) |
| `uxmessentials.economy.bypasscmdcost` | Skip per-command money costs |
| `uxmessentials.economy.baltop.exempt` | Hide your balance from `/baltop` |
| `uxmessentials.economy.pay.confirm.bypass` | Skip the large-payment confirmation prompt |

<Callout type="note" title="† Flag-gated commands">

Commands marked **†**: `/wallet`, `/sellall`, `/withdraw`, `/deposit`, `/bank`, `/loan`, `/exchange`, and the extra `/eco` subcommands (`note`, `backup`, `export`, `restore`): register only when their feature flag is enabled in `economy.conf`. On a fresh install some may not appear until you turn them on.

</Callout>

---

## Next Steps

- [💰 Economy](../features/economy.md) - Currencies, banks, loans, worth, and provider integration
- [🔐 Permission Reference](../permissions/reference.md) - Per-currency gates and bypass nodes
- [⚙️ Command Configuration](../config/commands-conf.md) - Rename or disable economy commands

---
title: Economy
order: 9050
description: Balances, payments and the leaderboard, plus banks, loans, banknotes and a sell price per item.
---

Every player holds a wallet with a balance per currency. One currency ships (`coins`), `/balance` and `/pay` do
the everyday work, and `/eco` is the admin surface. Balances live in the database rather than in player data, so
they survive a world rollback.

Module `economy` · enabled by default · `modules/economy/config.conf` and `currencies.conf`

## Commands

{/* generated:commands */}
| Command | What it does | Permission |
|---|---|---|
| `/balance` (`/bal`, `/money`) | See your own balance. | `uxmessentials.economy.balance` |
| `/baltop` (`/balancetop`) | View the top balances. | `uxmessentials.economy.baltop` |
| `/bank` | Open the bank panel to move money between your wallet and your bank balance. | `uxmessentials.economy.bank` |
| `/deposit` | Move money from your wallet into your bank balance. | `uxmessentials.economy.deposit` |
| `/eco` (`/economy`) | Umbrella for eco-admin mutations (/eco give, take, set). | `uxmessentials.economy.admin` |
| `/exchange` | Convert between two currencies at the configured rate. | `uxmessentials.economy.exchange` |
| `/loan` | Take, review and repay a loan against the configured limit and interest. | `uxmessentials.economy.loan` |
| `/pay` | Transfer funds. | `uxmessentials.economy.pay` |
| `/payall` | Pay every online player from your own wallet. | `uxmessentials.economy.payall` |
| `/payconfirm` | Transfer funds. | `uxmessentials.economy.pay` |
| `/paytoggle` | Refuse all incoming /pay transfers. | `uxmessentials.economy.pay.toggle` |
| `/sell` | Sell held items at their configured worth. | `uxmessentials.economy.sell` |
| `/sellall` | Sell held items at their configured worth. | `uxmessentials.economy.sell` |
| `/setworth` | Set or clear an item's sell worth override. | `uxmessentials.economy.setworth` |
| `/wallet` | Open your own wallet panel listing every currency you hold. | `uxmessentials.economy.wallet` |
| `/withdraw` | Move money from your bank balance back into your wallet. | `uxmessentials.economy.withdraw` |
| `/worth` | Report an item's configured sell value. | `uxmessentials.economy.worth` |
{/* /generated */}

## Permissions

{/* generated:permissions */}
| Node | Default | Grants |
|---|---|---|
| `uxmessentials.economy.admin` | op | Umbrella for eco-admin mutations (/eco give, take, set). |
| `uxmessentials.economy.admin.bulk` | op | /eco giveall, /eco giverandom, /eco resetall server-wide bulk mutations. |
| `uxmessentials.economy.admin.give` | op | /eco give \<player> \<amount> [currency] only. |
| `uxmessentials.economy.admin.set` | op | /eco set \<player> \<amount> [currency] and /eco reset only. |
| `uxmessentials.economy.admin.take` | op | /eco take \<player> \<amount> [currency] only. |
| `uxmessentials.economy.balance` | everyone | /balance [currency] to see your own balance. |
| `uxmessentials.economy.balance.others` | op | /balance \<player> [currency] to view another player's balance. |
| `uxmessentials.economy.baltop` | everyone | /baltop [currency] [page] to view the top balances. |
| `uxmessentials.economy.baltop.exempt` | off | Marks the holder as hidden from every /baltop leaderboard. |
| `uxmessentials.economy.bank` | everyone | /bank: open the bank panel to move money between your wallet and your bank balance. |
| `uxmessentials.economy.bank.create` | everyone | /bank create \<name>: open a new shared bank account. Held by default alongside the bank node. |
| `uxmessentials.economy.bank.deposit` | everyone | /bank deposit: pay into a shared bank account. Held by default alongside the bank node. |
| `uxmessentials.economy.bank.members` | everyone | /bank addmember and removemember: change who may use a shared bank account. |
| `uxmessentials.economy.bank.withdraw` | everyone | /bank withdraw: take money out of a shared bank account; the capability worth withholding from junior members. |
| `uxmessentials.economy.bypasscmdcost` | op | Skip the configured per-command economy charge (command-costs in economy.conf). |
| `uxmessentials.economy.currency.<currency>` | op | Use one currency that is configured to require a permission. |
| `uxmessentials.economy.deposit` | everyone | /deposit \<amount> [currency]: move money from your wallet into your bank balance. |
| `uxmessentials.economy.exchange` | everyone | /exchange \<amount> \<from> \<to>: convert between two currencies at the configured rate. |
| `uxmessentials.economy.loan` | off | /loan: take, review and repay a loan against the configured limit and interest. |
| `uxmessentials.economy.loan.take` | everyone | /loan take: borrow. Held by default alongside the loan node, so negating it leaves a borrower able to check and repay what they already owe but not to take more. |
| `uxmessentials.economy.pay` | everyone | /pay \<player> \<amount> [currency] and /payconfirm to transfer funds. |
| `uxmessentials.economy.pay.toggle` | everyone | /paytoggle to refuse all incoming /pay transfers. |
| `uxmessentials.economy.payall` | op | /payall \<amount> [currency]: pay every online player from your own wallet. |
| `uxmessentials.economy.salary.amount.<amount>` | off | The periodic salary you are paid; the largest tier held wins. |
| `uxmessentials.economy.sell` | everyone | /sell [amount] to sell held items at their configured worth. |
| `uxmessentials.economy.setworth` | op | /setworth [item] \<price>\|clear to set or clear an item's sell worth override. |
| `uxmessentials.economy.tax.bypass` | op | Send a /pay without the configured transfer tax being deducted. |
| `uxmessentials.economy.wallet` | everyone | /wallet: open your own wallet panel listing every currency you hold. |
| `uxmessentials.economy.withdraw` | everyone | /withdraw \<amount> [currency]: move money from your bank balance back into your wallet. |
| `uxmessentials.economy.worth` | everyone | /worth [item] to report an item's configured sell value. |
| `uxmessentials.module.economy` | op | Hot-reload / inspect the economy module (wallets, banks, currencies and the provider bridge). |
{/* /generated */}

## Settings

{/* generated:settings */}
| Key | Default | What it does |
|---|---|---|
| `amount-format` | `"full"` | full (grouped digits) \| compact (1.2k style) |
| `wallet.default-currency` | `"coins"` | id of the currency used when a command names none |
| `wallet.starting-balance` | `"0"` |  |
| `wallet.min-balance` | `"0"` |  |
| `wallet.max-balance` | `"1000000000000"` |  |
| `wallet.gui-enabled` | `true` | whether to enable the visual /wallet GUI |
| `gui.timezone` | `"UTC"` | zone id transaction-history timestamps render in (e.g. Europe/Istanbul); UTC keeps every server in a network consistent |
| `banknotes.enabled` | `true` | whether to enable physical banknote items (/withdraw and /deposit) |
| `bank.enabled` | `true` | whether to enable bank accounts (/bank) |
| `bank.interest.enabled` | `false` | off by default |
| `bank.interest.interval-hours` | `24` | how often interest is paid |
| `bank.interest.tiers` | `[...]` | "minBalance:ratePercent"; the highest qualifying tier applies |
| `loans.enabled` | `true` | whether to enable loan & debt system (/loan) |
| `loans.limit-multiplier` | `"1000"` | max principal = credit-score * this |
| `loans.interest-max` | `"0.22"` | interest rate at the lowest credit score |
| `loans.interest-span` | `"0.20"` | how far the rate drops from interest-max to the highest score |
| `loans.installment-cycle-hours` | `24` | hours between installments (auto-repayment due interval) |
| `loans.score-on-time-manual` | `25` | credit-score gain for a manual on-time payment |
| `loans.score-on-time-auto` | `10` | credit-score gain for a successful automatic repayment |
| `loans.score-missed` | `-50` | credit-score penalty for a missed automatic repayment |
| `loans.score-floor` | `100` | lowest a credit score can fall to |
| `loans.score-ceiling` | `1000` | highest a credit score can rise to |
| `pay.min-pay` | `"0.01"` |  |
| `pay.confirm-threshold` | `""` | empty disables the /pay confirmation prompt |
| `pay.confirm-timeout-ms` | `30000` | how long a pending /pay confirmation stays valid |
| `pay.toggle-default` | `true` | new players accept incoming payments by default |
| `pay.tax.enabled` | `false` | off by default; turn on to tax payments |
| `pay.tax.percent` | `"0"` | percent of the amount taken (e.g. "5" = 5%) |
| `pay.tax.flat` | `"0"` | a flat cut added on top, in the currency's units |
| `pay.tax.sink` | `"void"` | "void" destroys the tax, or "account:\<uuid>" routes it to a holding wallet |
| `pay.tax.bypass-permission` | `"uxmessentials.economy.tax.bypass"` | holders pay no tax |
| `baltop.page-size` | `10` |  |
| `baltop.cache-ttl-ms` | `60000` | how often the /baltop snapshot is refreshed |
| `baltop.snapshot-capacity` | `100` | rows retained per currency snapshot |
| `baltop.exempt-permission` | `"uxmessentials.economy.baltop.exempt"` |  |
| `baltop.exclude-banned` | `false` | drop banned players from the leaderboard |
| `baltop.min-balance` | `"0"` | hide rows below this balance (0 = show everyone) |
| `provider.register` | `true` | register the native economy with Vault/Treasury via ServicesManager |
| `provider.priority` | `"Normal"` | Lowest \| Low \| Normal \| High \| Highest |
| `allow-nonatomic-recurring` | `false` |  |
| `persistence.write-debounce-ms` | `250` | coalesce rapid balance writes |
| `persistence.batch-flush-ms` | `1000` | transaction-telemetry flush interval |
| `worth.enabled` | `true` |  |
| `worth.economyshopgui-fallback` | `true` |  |
| `worth.items` | `[...]` |  |
| `command-costs-enabled` | `true` |  |
| `command-costs` | `[...]` |  |
| `exchange.enabled` | `true` |  |
| `exchange.rates` | `[...]` |  |
| `salary.enabled` | `false` |  |
| `salary.interval-seconds` | `3600` |  |
| `salary.default-amount` | `100` |  |
| `fraud-detection.enabled` | `false` |  |
| `fraud-detection.webhook-url` | `""` |  |
| `fraud-detection.single-limit` | `"1000000"` |  |
| `fraud-detection.velocity-seconds` | `60` |  |
| `fraud-detection.velocity-limit` | `"5000000"` |  |
| `maintenance.enabled` | `false` | master switch; off means no maintenance runs at all |
| `maintenance.dry-run` | `true` | true = only log what WOULD be removed; set false to actually delete |
| `maintenance.run-interval-hours` | `24` | how often the task runs |
| `maintenance.prune-transactions-days` | `90` | delete transaction telemetry older than this many days |
| `maintenance.purge-inactive-days` | `90` | purge wallets of players unseen this long (never those with a loan/bank/score tie) |
| `daily-reward.enabled` | `false` | off by default |
| `daily-reward.amount` | `"100"` | how much to credit per claim |
| `daily-reward.currency` | `""` | currency id, or blank for the default currency |
| `daily-reward.cooldown-hours` | `24` | minimum gap between claims |
| `logs.file-enabled` | `false` | write the human-readable operations.log |
{/* /generated */}

## Placeholders

{/* generated:placeholders */}
| Placeholder | Renders |
|---|---|
| `%uxmessentials_balance%` | The player's balance in the default currency, as a plain number. |
| `%uxmessentials_balance_formatted%` | The player's balance with the currency symbol and grouping applied. |
| `%uxmessentials_baltop_position%` | The player's place on the default-currency leaderboard. |
| `%uxmessentials_economy_balance%` | The player's balance in the default currency, as a plain number. |
| `%uxmessentials_economy_balance_<currency>%` | The player's balance in one named currency, as a plain number. |
| `%uxmessentials_economy_balance_compact%` | The player's balance shortened to 1.2k / 3.4M. |
| `%uxmessentials_economy_balance_formatted%` | The player's balance with the currency symbol and grouping applied. |
| `%uxmessentials_economy_balance_formatted_<currency>%` | The player's balance in one named currency, with its symbol applied. |
| `%uxmessentials_economy_balance_short%` | The same shortened balance, under the spelling a config may prefer. |
| `%uxmessentials_economy_baltop_<currency>_<n>_amount%` | The balance of the player ranked nth in one named currency, as a plain number. |
| `%uxmessentials_economy_baltop_<currency>_<n>_formatted%` | The balance of the player ranked nth in one named currency, with its symbol applied. |
| `%uxmessentials_economy_baltop_<currency>_<n>_name%` | The name of the player ranked nth on one named currency's leaderboard. |
| `%uxmessentials_economy_baltop_<currency>_<n>_uuid%` | The uuid of the player ranked nth on one named currency's leaderboard. |
| `%uxmessentials_economy_baltop_<n>_amount%` | The balance of the player ranked nth, as a plain number. |
| `%uxmessentials_economy_baltop_<n>_formatted%` | The balance of the player ranked nth, with the currency symbol applied. |
| `%uxmessentials_economy_baltop_<n>_name%` | The name of the player ranked nth on the default-currency leaderboard. |
| `%uxmessentials_economy_baltop_<n>_uuid%` | The uuid of the player ranked nth on the default-currency leaderboard. |
| `%uxmessentials_economy_baltop_position%` | The player's place on the default-currency leaderboard. |
| `%uxmessentials_economy_currency_name%` | The plural name of the default currency. |
| `%uxmessentials_economy_currency_symbol%` | The symbol of the default currency. |
{/* /generated */}

## Notes

- **Extra currencies are defined in `currencies.conf`**, each with its own symbol, formatting, limits, icon and
  flags for whether it can be paid, exchanged or ranked. Commands take an optional `[currency]` and fall back to
  the default without one.
- **A currency can be backed by another plugin.** Set `backend` on it (`playerpoints`, `coinsengine:gold`, `exp`,
  `placeholder:<name>`) and uxmEssentials reads and writes that plugin's numbers instead of keeping a second
  balance beside it. A foreign-backed currency has no `/baltop`.
- **The extras are feature flags:** banks, loans, banknotes, worth and sell, exchange and the wallet dashboard
  each register their commands only when their block is enabled.
- **Banknotes need three things aligned:** the currency marked `physical`, its `denominations` set, and the
  `banknotes` block enabled.
- **`/baltop` is a snapshot** refreshed on `cache-ttl-ms`, not a live query, which is why a payment made a second
  ago may not have moved the board yet.
- **Turning the module off makes paid features free,** not broken: home, warp and vault costs stop being charged
  rather than failing.
- **On a server that already has a Vault economy**, uxmEssentials defers to the existing provider. Set
  `provider.register` and its priority deliberately if you want it to take over.

Related: [Homes](homes.md), [Kits](kits.md), [Ranks](../features/ranks.md)

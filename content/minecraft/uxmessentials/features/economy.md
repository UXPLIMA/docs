---
title: Economy
order: 1240
description: 'The economy module is the money backbone of the server: balances, /pay,
  the /baltop leaderboard, and an admin surface for adjusting it, plus optional banking,
  loans, banknotes, an item marketplace, and currency exchange. It is a multi-currency
  economy modelled the DDD way: a player owns a Wallet aggregate that holds one balance
  per Currency, and one currency ships as the default.'
---

The single most important fact about it:

<Callout type="danger" title="Balances are DB-backed, never PDC">

Every balance lives in the database, never in a player's PDC (persistent-data container). This is a hard invariant. It means **balances survive world rollbacks**, `/reload`, crashes, and restarts: a rolled-back world never rolls back anyone's money. It also means the economy needs no external service on the default SQLite backend, and the double-spend guard (an atomic guarded `UPDATE` at the DB) holds even across a cluster.

</Callout>

---

## Currencies & the Wallet

Out of the box a single default currency ships (`coins`). The `currencies.conf` file defines additional currencies (the shipped template covers `coins`, `gems`, `tokens`), each with its own:

- `symbol`, `plural`, `format`, `precision`: how amounts render.
- `starting`, `min-balance`, `max-balance`, `min-pay`: the numeric rules.
- `icon-material`: its icon in GUIs.
- `transfer-allowed`, `exchange-allowed`, `leaderboard-enabled`: whether it can be paid, converted, and ranked.
- `permission-required`: when `true`, holding a currency needs `uxmessentials.economy.currency.<id>`.
- `physical` / `denominations { … }`: whether it can be withdrawn as a banknote, and in what denominations.
- `backend`: where the balances actually live (see below).

A player's `/wallet` shows every currency they hold; most base commands take an optional `[currency]` argument and fall back to the default when it is omitted.

---

## Currency Backends: using another plugin's economy

By default every currency lives in this plugin's own ledger. A currency can instead be pointed at **an economy some other plugin owns**, per currency, with one key in `currencies.conf`:

```hocon
points { backend = "playerpoints", precision = 0, symbol = "pt" }
gold   { backend = "coinsengine:gold" }
xp     { backend = "exp", precision = 0 }
crowns { backend = "placeholder:crowns" }
```

That makes uxmEssentials a **front end** for that economy: `/balance`, `/pay`, shop prices, warp fees and every other charge read and write the foreign plugin's numbers, so there is one balance, not two that drift.

| `backend` value | Balances live in | Available when |
|---|---|---|
| omitted, or `native` | This plugin's DB ledger | Always |
| `exp` | The player's vanilla experience total | Always |
| `vault` | Whatever plugin owns the Vault economy | Vault is installed |
| `playerpoints` | PlayerPoints | PlayerPoints is installed |
| `coinsengine:<currency>` | The named CoinsEngine currency | CoinsEngine is installed **and** the currency is listed under `backends.coinsengine` |
| `zessentials:<currency>` | The named zEssentials currency | zEssentials is installed **and** the currency is listed under `backends.zessentials` |
| `placeholder:<name>` | Anything, via PlaceholderAPI plus console commands | The block is defined under `backends.placeholder` |

Multi-currency hosts have to be enumerated in `modules/economy/config.conf`, because the plugin cannot guess which of their currencies you want exposed:

```hocon
backends {
  coinsengine { gold {} }
  zessentials { tokens {} }
}
```

Every backend is probed by plugin presence before any of its classes are named, so a server without CoinsEngine never loads a CoinsEngine class. Backends that are actually present are logged on startup as `event=currency_backend_registered`.

### The placeholder escape hatch

For an economy nobody wrote a bridge for, the `placeholder` backend reads the balance from a PlaceholderAPI placeholder and performs credit and debit through console commands you supply:

```hocon
backends {
  placeholder {
    crowns {
      balance-placeholder = "%myeconomy_balance%"
      give-command        = "myeco give %player% %amount%"
      take-command        = "myeco take %player% %amount%"
      works-offline       = false
      integral            = false   # true rounds every amount HALF_UP for a whole-unit token economy
    }
  }
}
```

A console command returns nothing, so this backend cannot tell whether its take command actually worked. It checks the balance is sufficient, dispatches the take, and trusts it. That is the weakest backend on offer and it exists so an unusual economy is still usable, not because it is safe to bill against.

### What a foreign backend gives up

Pointing a currency at another plugin costs three things, and the server log names the currency each time:

- **`/baltop` is empty for it.** No foreign economy lets us enumerate every account.
- **Exchange, banks and loans switch off for it.** Those three write this plugin's wallet table directly, so running them against a currency the plugin does not own would invent money.
- **Scheduled charges refuse it.** Only the built-in ledger can promise that two simultaneous debits never both succeed past zero. A module that bills on a timer refuses a foreign-backed currency at startup unless you accept the risk:

```hocon
allow-nonatomic-recurring = false
```

Player-warp rent is the charge this guard exists for. Salary and bank interest pay **out** rather than charge, and loans are already limited to the built-in ledger.

<Callout type="tip" title="Mix freely">

Backends are per currency, not per server. A perfectly ordinary setup is `coins` on the native ledger (so `/baltop`, banks and loans all work) alongside `gold` on CoinsEngine and `xp` on vanilla experience.

</Callout>

---

## Everyday Money: Balance, Pay, Baltop

These are the always-on core commands (they cannot be feature-flagged off):

| Command | Description | Permission |
|---------|-------------|------------|
| `/balance [player] [currency]` | Show own / another's balance | `.economy.balance` / `.balance.others` |
| `/pay <player> <amount> [currency]` | Transfer funds | `uxmessentials.economy.pay` |
| `/payall <amount> [currency]` | Pay every online player | `uxmessentials.economy.payall` |
| `/paytoggle` | Refuse incoming `/pay` | `uxmessentials.economy.pay.toggle` |
| `/baltop [currency] [page]` | View the top balances | `uxmessentials.economy.baltop` |

Large payments can require a confirmation step. The `pay` config block sets `min-pay`, a `confirm-threshold` (amounts above it prompt for `/payconfirm`), a `confirm-timeout-ms`, an optional `tax`, and the `toggle-default`. Holders of `uxmessentials.economy.pay.confirm.bypass` skip the confirmation.

`/baltop` is served from a cached snapshot (its query is bounded and runs off-thread) so it never lags the server. `baltop` config controls `page-size`, `cache-ttl-ms`, `snapshot-capacity`, `exclude-banned`, `min-balance`, and an `exempt-permission`: players holding `uxmessentials.economy.baltop.exempt` stay off the leaderboard.

### Taxing payments

`pay.tax` skims a cut off every `/pay` as an inflation sink. The receiver is credited the remainder:

```hocon
pay.tax {
  enabled = false
  percent = "0"          # percent of the amount
  flat = "0"             # a flat cut on top, in the currency's units
  sink = "void"          # "void" destroys it, or "account:<uuid>" banks it
  bypass-permission = "uxmessentials.economy.tax.bypass"
}
```

Route the tax to a holding wallet with `account:<uuid>` when you want a visible treasury rather than a burn.

---

## Charge Receipts: when another feature takes money

Most money does not move through `/pay`. It moves because a player used a paid warp, claimed a priced kit, set a home past their free slots, bought a rank, opened an extra vault, took an NPC action, paid a teleport link, or paid to enter (or rent, or sponsor) a player warp. Every one of those charges is reported to the person who paid:

```
[ECONOMY] Paid $250.00 for a warp.
```

The sentence itself is one catalog line, `eco.charged`, and each feature supplies its own label through a companion key, so you translate the sentence once and the nine labels once:

| Key | Default label |
|---|---|
| `eco.charge.warp` | a warp |
| `eco.charge.kit` | a kit |
| `eco.charge.home` | a home |
| `eco.charge.rank` | a rank |
| `eco.charge.trade` | a trade |
| `eco.charge.action` | that action |
| `eco.charge.teleport` | a teleport |
| `eco.charge.vault` | a vault |
| `eco.charge.playerwarp` | a player warp |

A charge that is refused (not enough funds) sends no receipt: the feature already explains why it did not go through, and no money moved. A charge of zero, a free warp or a waived kit price, sends nothing either. To go silent entirely, blank the `eco.charged` value in your catalog.

---

## The `/eco` Admin Surface

`/eco` (alias `economy`) is the staff toolbox. Bare, it opens an admin GUI hub (`uxmessentials.economy.admin`). Its subcommands each carry their own node:

| Command | Purpose | Permission |
|---------|---------|------------|
| `/eco give` · `take` · `set <player> <amt> [currency]` | Adjust one balance | `.admin.give` / `.admin.take` / `.admin.set` |
| `/eco reset <player> [currency]` | Back to the starting balance | `.economy.admin.set` |
| `/eco giveall` · `giverandom <amt>` | Bulk server-wide credit | `.economy.admin.bulk` |
| `/eco resetall --confirm` | Wipe every balance (confirm-gated) | `.economy.admin.bulk` |
| `/eco give-random <player> <min> <max>` | Give an amount inside a range | `.economy.admin.give` |
| `/eco history [player]` · `/eco logs` | Transaction history / global log GUI | `.admin.history` / `.admin.logs` |
| `/eco note give <player> <amt>` | Mint an admin banknote (no debit) | `.economy.admin.note` |
| `/eco backup` · `export` · `restore <player> <date>` | Snapshot / CSV export / restore | `.admin.backup` / `.admin.restore` |

Every mutation is written to the DB-backed ledger, which is what `/eco history` and `/eco logs` read back.

---

## Feature-Flagged Extras

The base commands above are always on. The following are **gated by feature flags in the module config**: they only register and respond when their block is enabled.

| Feature | Commands | Config block |
|---------|----------|--------------|
| **Banks** | `/bank [create\|deposit\|withdraw\|addmember\|removemember …]` | `bank { enabled, interest { tiers } }` |
| **Loans / credit** | `/loan [status\|take\|pay …]` | `loans { enabled, limit-multiplier, interest-*, installment-cycle-hours, score-* }` |
| **Banknotes** | `/withdraw <amount>` · `/deposit` | `banknotes { … }` |
| **Marketplace** | `/worth [item]` · `/setworth …` · `/sell [amount]` · `/sellall` | `worth { enabled, economyshopgui-fallback, items[] }` |
| **Exchange** | `/exchange [<amount> <src> <dst>]` | `exchange { enabled, rates[] }` |
| **Wallet dashboard** | `/wallet [currency]` | `wallet { gui-enabled }` |

- **Banks** are shared accounts with members and optional tiered interest.
- **Loans** lend against a credit score, repaid on an installment cycle.
- **Banknotes** turn virtual money into a physical item (`/withdraw`) and back (`/deposit`). The currency must be marked `physical` with `denominations`. Admins can mint notes with `/eco note give` without debiting anyone.
- **Worth/sell** gives items a sell value (`worth.items[]`) so `/sell` and `/sellall` turn inventory into cash. An entry is `MATERIAL:price`, or `MATERIAL:price:currencyId` to pay out in another currency.

<Callout type="tip" title="Already priced your shop? Don't price it twice">

With **EconomyShopGUI** installed (either edition, free or premium) and
`worth.economyshopgui-fallback = true` (the default),
an item with no `/setworth` override and no `worth.items[]` entry falls back to the shop's
own sell price. Your prices always win: the fallback is consulted only where you set
nothing. An item the shop does not sell either stays unsellable, exactly as before.

A worth is a property of the item for us, not of who is holding it, so the price read is
the shop's base sell price for a single item. On a shop with dynamic (stock-based) pricing
that is the base number rather than the price after the day's trading: price such an item
explicitly if you need the live one.

With the plugin absent the setting does nothing, so leaving it on costs you nothing.

</Callout>

- **Exchange** converts between currencies at configured `rates[]`, each `"src:dst:rate:feePercent"`.

<Callout type="note" title="Turning a feature off makes it disappear">

If a block is disabled, its commands are not registered at all: a disabled module wires nothing. Disabling the whole economy module makes paid features elsewhere (home costs, warp costs, vault costs) become **free** rather than erroring.

</Callout>

---

## Passive Income and Housekeeping

Four smaller blocks, all shipped off, that run on a timer rather than on a command.

```hocon
salary {
  enabled = false
  interval-seconds = 3600      # how often active players are paid
  default-amount = 100
}

daily-reward {
  enabled = false
  amount = "100"
  currency = ""                # blank uses the default currency
  cooldown-hours = 24
}

fraud-detection {
  enabled = false
  webhook-url = ""             # where suspicious transfers are reported
  single-limit = "1000000"     # one transfer above this is flagged
  velocity-seconds = 60        # the sliding window
  velocity-limit = "5000000"   # total moved inside the window before flagging
}

maintenance {
  enabled = false              # master switch
  dry-run = true               # true only logs what WOULD be removed
  run-interval-hours = 24
  prune-transactions-days = 90 # drop telemetry older than this
  purge-inactive-days = 90     # purge wallets of players unseen this long
}
```

**Salary** pays a recurring amount to players who are actually active; give a rank a bigger payslip with `uxmessentials.economy.salary.amount.<n>` and the highest node a player holds wins. **Daily reward** credits once per cooldown window on the first join inside it, which is a cheap retention nudge. **Fraud detection** watches for one huge transfer or a burst of them and reports to a webhook. 

<Callout type="warning" title="Maintenance deletes data, behind a two-step opt-in">

`maintenance` needs `enabled = true` **and** `dry-run = false` before it removes anything. Run it dry first and read the log. It never purges a player who has a loan, a bank tie or a credit score.

</Callout>

Two more small keys worth knowing: `gui.timezone` sets the zone transaction timestamps render in (leave it `UTC` across a network so every backend agrees), and `logs.file-enabled` writes a plain-text, grep-able `economy/operations.log` alongside the DB telemetry.

---

## Key Settings (module config)

`modules/economy/config.conf` plus its sibling `currencies.conf`. The headline keys:

| Key | What it does |
|-----|--------------|
| `amount-format` | `full` or `compact` (e.g. `1.2k`) number rendering. |
| `wallet { default-currency, starting-balance, min-balance, max-balance, gui-enabled }` | The default currency and wallet limits. |
| `pay { min-pay, confirm-threshold, confirm-timeout-ms, toggle-default, tax { } }` | Payment rules. |
| `baltop { page-size, cache-ttl-ms, snapshot-capacity, exempt-permission, exclude-banned, min-balance }` | Leaderboard behaviour. |
| `backends { coinsengine, zessentials, placeholder }` | Which foreign economies a currency may point at. |
| `allow-nonatomic-recurring` | Lets timed charges bill a foreign-backed currency. |
| `bank`, `loans`, `banknotes`, `worth`, `exchange` | The feature-flag blocks above. |
| `salary`, `daily-reward`, `fraud-detection`, `maintenance`, `logs` | The timed and housekeeping blocks (all ship disabled). |
| `command-costs[]` | Per-command charges. |
| `persistence { write-debounce-ms, batch-flush-ms }` | How aggressively balance writes are coalesced. |
| `provider { register, priority }` | How uxmEssentials registers itself as *the* economy provider (below). |

---

## Provider Registration (Vault / Treasury)

Other plugins (shops, jobs, quests) find an economy through Bukkit's `ServicesManager`. uxmEssentials plays this both ways with a **register-or-defer** strategy:

1. On enable it looks for an **existing** foreign economy provider, **Treasury first, then Vault**.
2. If one is found, uxmEssentials **consumes** it (the foreign economy is authoritative) and defers.
3. If none is found, uxmEssentials **registers its own** native DB-backed ledger at `ServicePriority.Normal`, so every Vault/Treasury-aware plugin uses uxmEssentials as the economy.

The `provider { register, priority }` block controls whether and at what priority it registers. Vault and non-default currencies do not mix: a Vault call for a non-default currency is rejected with `CURRENCY_UNSUPPORTED`, because Vault has no concept of multiple currencies.

See [Vault & Treasury](../integrations/vault-treasury.md) for the full integration story.

<Callout type="info" title="Provider registration and currency backends are different things">

Provider registration is about **who other plugins ask** for a balance. A currency `backend` is about **where that balance is stored**. You can register as the Vault provider while some of your currencies are backed by other plugins.

</Callout>

---

## Permissions at a Glance

| Node | Purpose | Default |
|------|---------|---------|
| `uxmessentials.economy.balance` / `.balance.others` | Own / others' balance | `true` / op |
| `uxmessentials.economy.pay` / `.payall` / `.pay.toggle` | Transfer funds | varies |
| `uxmessentials.economy.baltop` | View the leaderboard | `true` |
| `uxmessentials.economy.admin` (+ `.give` / `.take` / `.set` / `.bulk` / `.history` / `.logs` / `.note` / `.backup` / `.restore`) | Admin operations | `op` |
| `uxmessentials.economy.bank` / `.loan` / `.wallet` / `.exchange` / `.sell` / `.withdraw` / `.deposit` / `.worth` / `.setworth` | Feature commands | varies |
| `uxmessentials.economy.currency.<id>` | Per-currency gate (when `permission-required = true`) | none |
| `uxmessentials.economy.salary.amount.<n>` | Per-rank salary tier; the highest `<n>` a player holds wins, otherwise `salary.default-amount` | none |
| `uxmessentials.economy.baltop.exempt` | Stay off the leaderboard | `false` |
| `uxmessentials.economy.tax.bypass` | Pay no `/pay` tax (rename it with `pay.tax.bypass-permission`) | none |
| `uxmessentials.economy.pay.confirm.bypass` | Skip large-pay confirmation | `op` |
| `uxmessentials.economy.bypasscmdcost` | Skip per-command costs | `op` |

---

## Tips & Gotchas

- **Balances survive rollbacks; that is the point.** Never store money in PDC or scoreboards "for speed": the DB ledger is the source of truth and is already cached (Caffeine) between the repository and the database.
- **`/baltop` is a snapshot.** If a fresh payment does not show at the top immediately, that is the `cache-ttl-ms` window, not a bug. It is also permanently empty for a foreign-backed currency.
- **Disable a currency's `leaderboard-enabled` / `transfer-allowed` / `exchange-allowed` flags** to make a token-style currency non-tradeable and non-ranked.
- **Physical currency needs three things aligned:** the currency marked `physical`, `denominations` defined, and the `banknotes` block enabled, or `/withdraw` will not work.
- **A `backend` typo is not silent.** An unknown backend id is reported at startup naming the currency, so check the log after editing `currencies.conf`.
- **`exp` cannot pay an offline player.** Vanilla experience only exists on a live player, so anything that credits an offline target skips an `exp`-backed currency.
- **Adding uxmEssentials to a server that already has a Vault economy?** It defers to the existing provider by default. Set `provider.register` and its priority deliberately if you want uxmEssentials to take over.
- **Non-default currencies over Vault fail with `CURRENCY_UNSUPPORTED`.** Vault-only plugins can only ever see the default currency.

---

## Next Steps

- [💰 Economy & Banking Commands](../commands/economy.md) - The full command reference
- [🔌 Vault & Treasury](../integrations/vault-treasury.md) - Provider registration in detail
- [🔐 Permission Reference](../permissions/reference.md) - Admin, currency, and bypass nodes
- [⚙️ Per-Module Config](../config/per-module.md) - Every `economy` config and `currencies.conf` key

---
title: Trade
order: 1280
description: The trade module is a safe way for two players to swap goods face to
  face — /trade <player> opens a shared window where each side lays out items (and,
  with an economy wired, stakes money), and nothing changes hands until both players
  confirm. It replaces the old "drop it on the ground and hope" ritual, and can even
  move goods between two players on different backend servers behind a proxy. It's
  driven by a single HOCON file, modules/trade/config.conf.
---

The module ships **enabled**. Turn the whole feature off with `enabled = false`.

---

## The Trade Window

`/trade <player>` sends a request; once the target accepts, both players open the same
trade window — a six-row (54-slot) chest whose **top row is the control bar** and whose
**five rows below hold the items**, split down the centre by a glass divider column:

- **The left four columns are yours to edit** — drop in the items you're offering. That
  is 20 slots per side, which is the `slots-per-side` maximum and its default.
- **The right four columns mirror the other player's offer**, read-only, so you always
  see exactly what you're getting. Your slot and their mirror slot line up on the same
  row.
- **The top row reads viewer-left, partner-right**: your confirm button, your money
  button and your experience button on the left; the partner's staked experience, their
  staked money and their confirm status on the right.

Your confirm button is **yellow** until you click it, then turns **lime**; the partner
pane shows their status the same way — **red** while they're still deciding, **lime**
once they've locked in.

<Callout type="warning" title="Both must confirm — and any change resets both">

The swap only runs when **both** sides confirm with no pending change. The moment
anyone adds, removes, or re-prices anything — an item *or* money — **both
confirmations reset to unconfirmed**. That's the anti-scam guarantee: nobody can get
you to confirm a fair deal and then quietly swap in a worse one before the trade
settles.

</Callout>

When both sides confirm, each player receives the other's stacks. Items are never
deleted — anything that doesn't fit in a full inventory drops at the recipient's feet.
Backing out is just as safe: closing the window, disconnecting, or changing worlds
**cancels the trade and returns every staked item to its owner** exactly once.

---

## Requesting a Trade

| Command | What it does |
|---------|--------------|
| `/trade <player>` | Send a trade request to a nearby player |
| `/trade accept [player]` | Accept a pending request (the newest, or the one from `[player]`) and open the window |
| `/trade deny [player]` | Decline a pending request |

`/trade accept` and `/trade deny` take an optional name so you can pick which request to
answer when several people have asked; tab-completion suggests the players currently
waiting on you.

Requests are governed by three config knobs:

- **`request-distance`** — how near, in blocks, two players must be for `/trade` to open.
  The shipped default is `0`, meaning **no distance limit**. Set it to a positive value
  and the two players must be within that many blocks **in the same world**; leave it at
  `0` for server-wide (and cross-server) trading.
- **`cooldown-seconds`** — the per-player wait between sending requests, to stop spam.
  Default `5`. Set it to `0` to disable.
- **`request-expiry-seconds`** — how long a request stays acceptable before it lapses.
  Default `60`. After it expires, `/trade accept` no longer opens it.

A request to yourself, to someone already in a trade, or beyond the distance limit is
refused with a clear message rather than opening a broken window.

---

## Staking Money

When an economy provider is available, the control row carries an **"add money" button**.
Click it, type an amount, and it is staked into your side of the deal; the partner's
staked money shows read-only on their half of the row.

The button holds **one currency at a time**: a **left-click stakes** the selected
currency and a **right-click cycles** to the next allowed one. Each currency's staked
amount is kept across the cycle, so you can stake several currencies in one trade through
the single button. Which currencies may be staked is `currencies-allowed` (default
`["coins"]`), listing ids from `modules/economy/currencies.conf`.

<Callout type="info" title="Money moves all-or-nothing">

Staked money settles atomically through the economy provider **at the same instant**
the items swap. If a payer can't cover their stake when the trade commits, **no money
moves at all**, every item is returned to its owner, and both sides are told the
trade failed for insufficient funds. Items and money always move together or not at
all.

</Callout>

<Callout type="note" title="No economy? Items-only">

With no economy provider wired, the money button simply doesn't appear and the trade
moves items only — that control slot falls back to plain divider filler.

</Callout>

---

## Staking Experience

Next to the money button sits an **"add experience" button**. Click it, type a number of
points, and that experience is staked into your side exactly like money or an item.

Experience settles with everything else: it moves at the same instant the items swap, and
if either side cannot cover their stake at commit time **nothing** moves — no items, no
money, no experience. Only the side that actually staked any experience has a leg to
settle, so an ordinary items-only trade costs nothing extra.

The items-only cross-server window has no experience button; that slot falls back to
divider filler there.

---

## The Item Blacklist

`item-blacklist` is a list of Bukkit material ids that may **never** enter a trade window
— for example:

```hocon
item-blacklist = [ "BEDROCK", "COMMAND_BLOCK", "BARRIER" ]
```

A blacklisted stack is refused straight back into the placing player's inventory with a
notice, so it never becomes part of the offer. Leave the list empty (the default) to bar
nothing.

---

## Cross-Server Trading

With a Velocity proxy and the cross-server bus configured, two players on **different
backend servers** can trade — set `cross-server = true`. It ships **off**.

<Callout type="warning" title="Two prerequisites">

Cross-server trading needs the **network bus enabled** (a Velocity proxy with the
cross-server transport configured — see [Velocity & Redis](../cross-server/overview.md)),
and **`request-distance = 0`**, since the two players are never near each other — they
aren't even on the same server.

</Callout>

When cross-server is on and the named player isn't on your server, `/trade <name>` sends
an invite across the bus. The target's trade window **opens directly** (there's no
`/trade accept` step for cross-server), and they're told who invited them. Each side
stakes items in their own window and confirms.

Under the hood, a confirm doesn't hand items straight over — it **escrows** them. Each
backend serialises its own player's staked items into a durable, shared escrow record and
runs a **two-phase commit** with the other server:

1. **Held** — each side, on confirm, debits/holds its own player's stake in the escrow
   store and signals the peer that it's ready.
2. **Committed** — once *both* sides are held, both records flip to committed atomically.
   This is the point of no return: neither side can back out past it.
3. **Delivered** — each backend then claims the counterpart's committed record and hands
   the goods to its own local player, exactly once.

Because every step is a single guarded transition on a durable record, the flow survives
crashes and reconnects: a stranded stake is refunded and a fully-escrowed trade is settled
when the owner next logs in, and goods bound for an offline player wait until they're back
— nothing is ever lost or duplicated. Closing the window before confirming returns your
items and aborts the other side.

<Callout type="note" title="v1 limitation: cross-server trades are items-only">

In this version the **cross-server window stakes items only** — money can't be put on
the table across servers yet. Money staking is available on **same-server** trades.

</Callout>

---

## Configuration

The whole module is one file, `modules/trade/config.conf`:

```hocon
# Trade — secure player-to-player trading. /trade <player> opens a shared window where both players place items (and,
# once money is enabled, stake currency) and each side confirms before the swap happens. Any change to an offer resets
# both confirmations, so no one can confirm a fair deal and then swap in a worse one.
#
# The module ships ENABLED. Turn the whole feature off with enabled = false.
enabled = true

# Which currencies players may stake as money in a trade. List the currency ids from modules/economy/currencies.conf
# that are allowed here. Leave it at the default single currency, or add more for a multi-currency economy.
currencies-allowed = [ "coins" ]

# Materials that may never be placed into a trade window. Use Bukkit material ids (e.g. "BEDROCK", "COMMAND_BLOCK").
# A blacklisted item is refused back to the player rather than accepted into the window. Empty means nothing is barred.
item-blacklist = [ ]

# How near two players must be, in blocks, for /trade <player> to open. Set to 0 (or any value \<= 0) for no distance
# limit — required if you want cross-server trades, where the two players are on different servers entirely.
request-distance = 0

# The per-player cooldown, in seconds, between sending trade requests. Stops request spam. Set to 0 to disable it.
cooldown-seconds = 5

# How long a /trade request stays acceptable, in seconds, before it expires and /trade accept no longer opens it.
request-expiry-seconds = 60

# Whether a completed trade writes one audit line (who traded whom, item counts and money) to the shared audit
# channel — the same channel the optional Discord bridge listens on. Set to false to trade silently.
audit = true

# Whether two players on different backend servers may trade, with each server escrowing its own player's stake and a
# two-phase commit over the proxy bus. Needs a Velocity proxy and the cross-server bus configured; ships OFF.
cross-server = false

# How many item slots each player gets on their side of the trade window. Both sides get the same count. The window
# is the AxTrade layout: the top row holds the controls (confirm, money, experience) and the five rows below hold the
# items, four columns per side, so the maximum (and default) is 20 slots each.
slots-per-side = 20
```

| Key | Default | What it does |
|-----|---------|--------------|
| `enabled` | `true` | Master switch for the whole trade module |
| `currencies-allowed` | `["coins"]` | Currency ids that may be staked as money (up to three show as buttons) |
| `item-blacklist` | `[]` | Materials refused into the window; empty bars nothing |
| `request-distance` | `0` | Max blocks between players to open a trade; `0` or less is unlimited (and required for cross-server) |
| `cooldown-seconds` | `5` | Per-player wait between requests; `0` disables |
| `request-expiry-seconds` | `60` | How long a request stays acceptable before it lapses |
| `audit` | `true` | Log one line per completed trade to the audit channel |
| `cross-server` | `false` | Allow trades between different backend servers (needs a proxy + the bus) |
| `slots-per-side` | `20` | Item slots each side gets; the grid holds at most 20 per side (four columns by five rows), so a larger value is clamped to that |

<Callout type="info" title="Audit trail">

With `audit = true` (the default), each completed swap writes one structured
`event=trade_completed` line to the dedicated audit channel — the same channel the
optional Discord bridge listens on — recording both players (by UUID and name), each
side's total item count, and each side's staked money per currency. Set `audit =
false` to trade silently.

</Callout>

---

## Permissions

| Node | Default | What it grants |
|------|---------|----------------|
| `uxmessentials.trade.use` | `true` | `/trade <player>` — request a trade; `/trade accept\|deny [player]` — answer one |
| `uxmessentials.module.trade` | `op` | Hot-reload / inspect the trade module via `/uxmess reload trade` |

`uxmessentials.trade.use` ships `true`, so every player can trade out of the box; revoke
it for a group to bar them.

---

## Next Steps

- [💰 Economy](economy.md) — the currencies and provider that back staked money
- [🌐 Velocity & Redis](../cross-server/overview.md) — the proxy and bus cross-server trading rides on
- [🧩 Per-Module Config](../config/per-module.md) — how `modules/trade/config.conf` fits the layout
- [🔑 Permission Reference](../permissions/reference.md) — the full node list

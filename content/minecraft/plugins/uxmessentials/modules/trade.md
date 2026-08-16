---
title: Trade
order: 9280
description: A shared window where two players swap items, money and experience, with nothing moving until both confirm.
---

`/trade <player>` opens a window both players share: your four columns on the left, a read-only mirror of theirs
on the right, and a control row for confirming and for staking money and experience. Nothing changes hands until
both sides confirm, and with a proxy in front the two players do not even have to be on the same server.

Module `trade` · enabled by default · `modules/trade/config.conf`

## Commands

{/* generated:commands */}
| Command | What it does | Permission |
|---|---|---|
| `/trade` | Request a trade with another player and accept or deny requests. | `uxmessentials.trade.use` |
{/* /generated */}

## Permissions

{/* generated:permissions */}
| Node | Default | Grants |
|---|---|---|
| `uxmessentials.module.trade` | op | Hot-reload / inspect the trade module. |
| `uxmessentials.trade.use` | everyone | /trade: request a trade with another player and accept or deny requests. |
{/* /generated */}

## Settings

{/* generated:settings */}
| Key | Default | What it does |
|---|---|---|
| `currencies-allowed` | `[ "coins" ]` | Which currencies players may stake as money in a trade. List the currency ids from modules/economy/currencies.conf that are allowed here. Leave it at the default single currency, or add more for a multi-currency economy. |
| `item-blacklist` | `[ ]` | Materials that may never be placed into a trade window. Use Bukkit material ids (e.g. "BEDROCK", "COMMAND_BLOCK"). A blacklisted item is refused back to the player rather than accepted into the window. Empty means nothing is barred. |
| `request-distance` | `0` | How near two players must be, in blocks, for /trade \<player> to open. Set to 0 (or any value \<= 0) for no distance limit; required if you want cross-server trades, where the two players are on different servers entirely. |
| `cooldown-seconds` | `5` | The per-player cooldown, in seconds, between sending trade requests. Stops request spam. Set to 0 to disable it. |
| `request-expiry-seconds` | `60` | How long a /trade request stays acceptable, in seconds, before it expires and /trade accept no longer opens it. |
| `audit` | `true` | Whether a completed trade writes one audit line (who traded whom, item counts and money) to the shared audit channel; the same channel the optional Discord bridge listens on. Set to false to trade silently. |
| `cross-server` | `false` | Whether two players on different backend servers may trade, with each server escrowing its own player's stake and a two-phase commit over the proxy bus. Needs a Velocity proxy and the cross-server bus configured; ships OFF. |
{/* /generated */}

## Placeholders

{/* generated:placeholders */}
| Placeholder | Renders |
|---|---|
| `%uxmessentials_trading%` | Whether the two are the sides of the same live trade (yes/no). |
{/* /generated */}

## Notes

- **Items, money and experience settle together or not at all.** If a player cannot cover their stake at the
  moment the trade commits, every item goes back to its owner and both sides are told why.
- **The money button holds one currency at a time.** Left-click stakes the selected currency, right-click cycles
  to the next one in `currencies-allowed`, and each amount survives the cycle, so several currencies can ride on
  one trade. With no economy provider the button is not drawn and the trade moves items only.
- **`item-blacklist` is absolute.** A listed material can never enter a trade window, whichever side offers it.
- **Cross-server trading is off by default and needs two things:** the network bus configured behind a proxy,
  and `request-distance = 0`, since the two players are never near each other.
- **A cross-server trade escrows rather than hands over.** Each backend holds its own player's stake in a shared
  escrow record and both flip to committed only once both sides are held, so a server that dies mid-trade cannot
  duplicate or eat the goods. There is no `/trade accept` step across servers: the invited window opens directly.

Related: [Economy](economy.md), [Vaults](vaults.md), [Cross-server](../cross-server/overview.md)

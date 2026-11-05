---
title: Money and progress
order: 204
description: Bank, vault, upgrades, shop, missions, boosters, rewards, seasons and leaderboards.
icon: coins
---

## Bank and vault

| Command | What it does | Permission |
|---|---|---|
| `/is bank` | Opens the [bank window](../menus/economy.md#bank) | `uxmskyblock.island.bank` |
| `/is bank balance` | Shows what the bank holds | `uxmskyblock.island.bank` |
| `/is bank deposit <amount> [currency]` | Puts money, or another currency, in; `all` puts in everything you carry | `uxmskyblock.island.bank` |
| `/is bank withdraw <amount> [currency]` | Takes money, or another currency, out | `uxmskyblock.island.bank` |
| `/is bank status`, `/is bank upkeep` | Shows the island's upkeep and any debt | `uxmskyblock.island.bank` |
| `/is bank paydebt` | Pays the upkeep debt from the bank | `uxmskyblock.island.bank` |
| `/is vault [page]` | Opens the shared vault | |
| `/is activity` | Opens what happened on your island lately | |

What each currency is called, and which ones the bank keeps, is in [bank.conf](../config/bank-conf.md). Depositing and
withdrawing also need the island permissions Put money in the bank and Take money from the bank.

## Upgrades and shop

| Command | What it does | Permission |
|---|---|---|
| `/is upgrades [list]` | Opens the [upgrades window](../menus/economy.md#upgrades), or lists them in chat | `uxmskyblock.island.upgrade` |
| `/is upgrade` | The same as `/is upgrades` | `uxmskyblock.island.upgrade` |
| `/is shop` | Opens the island shop | |
| `/is shop list` | Lists the shop in chat | |
| `/is shop buy <item> [amount]`, `/is shop sell <item> [amount]` | Buys with the bank, or sells into it | |

## Missions, boosters and rewards

| Command | What it does |
|---|---|
| `/is missions [daily \| weekly \| challenges]` | Opens the [missions window](../menus/economy.md#missions), or one kind of mission |
| `/is challenges` | Opens the one-time challenges |
| `/is booster` | Shows the boosters running on your island and their multipliers |
| `/is booster apply <category> <multiplier> <duration>` | Starts a booster on your island; staff only, `uxmskyblock.admin.booster` |
| `/is rewards [list]` | Lists the rewards waiting in your inbox |
| `/is rewards claim [id]` | Hands you every waiting reward, or one |
| `/is season` | Shows the season under way |
| `/is season list` | Lists the seasons |
| `/is season rewards` | Hands over what the seasons owe you |
| `/is season top [season [metric]]` | Shows the standings of the season under way, or of a numbered season, by level unless you name a board |

## Leaderboards

| Command | What it does | Permission |
|---|---|---|
| `/is top [level \| worth \| bank]` | Opens the leaderboards, or one board | `uxmskyblock.island.top` |

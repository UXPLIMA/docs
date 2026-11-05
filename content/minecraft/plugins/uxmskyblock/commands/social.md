---
title: Travel and social
order: 205
description: Warps, visits and fees, guestbook, ratings, bookmarks, alliances, chat and trading.
icon: map-pin
---

## Warps and visits

| Command | What it does |
|---|---|
| `/is visit <player> [confirm]` | Visits another player's island; with a visit fee, `confirm` pays it |
| `/is warp` | Opens your island's [warps window](../menus/travel.md#warps) |
| `/is warp list` | Lists your island's warps in chat |
| `/is warp browse [category]` | Browses every public warp |
| `/is warp visit <owner> <name>` | Goes to another island's warp |
| `/is warp create <name> [category]` | Opens a warp where you stand |
| `/is warp move <name>` | Moves a warp to where you stand |
| `/is warp delete <name>` | Closes a warp for good |
| `/is warp lock <name>`, `/is warp unlock <name>` | Closes a warp to visitors, or opens it again |
| `/is warps [category]` | Browses every public warp on the server |
| `/is warplock <name>` | Closes a warp to visitors |
| `/is warpunlock <name>` | Opens it again |
| `/is visitfee [amount]` | Shows or sets what a visitor pays your island's bank to come in; 0 for nothing |

How many warps an island opens is its [warp slots upgrade](../features/upgrades.md). The categories are in
[warps.conf](../config/warps-conf.md).

## Community

| Command | What it does |
|---|---|
| `/is guestbook [sign <message>]` | Reads, or signs, the guestbook of the island you stand on |
| `/is rate <score>` | Rates the island you stand on, from one to five |
| `/is bookmarks [toggle]` | Lists your bookmarks, or bookmarks the island you stand on |
| `/is alliance` | Opens the [alliances window](../menus/travel.md#alliances) |
| `/is alliance invite <player>` | Offers an alliance to that player's island |
| `/is alliance list` | Lists your allies |
| `/is alliance invites` | Lists the offers waiting for your island |
| `/is alliance accept <player>` | Accepts an offer |
| `/is alliance decline <player>` | Turns an offer down |
| `/is alliance break <player>` | Breaks an alliance |
| `/is allychat <message>`, `/is ac` | Speaks to your island and its allies |
| `/is chat [message]`, `/is c` | Switches your chat to the island channel, or sends one line to it |
| `/is spy` | Listens in on every island chat; needs `uxmskyblock.chat.spy` |
| `/is trade <player>` | Asks a player to trade, or answers their question |

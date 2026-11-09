---
title: Trusted players
order: 309
description: Friends who can watch, pause, resume, boost and stock your builds.
icon: users
---

An owner can name players they trust with all of their builds. A trusted player helps run them without
being able to touch the money behind them.

## What trust allows

| Action | Owner | Trusted player |
|---|---|---|
| See the builds in **Active Builds** | yes | yes, marked with the owner's name |
| Pause and resume | yes | yes |
| Buy a [speed boost](boost.md) | yes | yes, paying for it themselves |
| Stock the warehouse | yes | yes, with `/builders warehouse <owner>` or `/builders warehouse give` |
| Take from the warehouse | yes | **no** |
| Cancel | yes | **no** |

Cancelling moves the owner's refund, so it stays with the owner. Staff with `uxmbuilders.admin` can
act on any build.

Trust is one-way and covers every build the owner has, now and later.

## Commands

| Command | Does |
|---|---|
| `/builders trust <player>`, `trust add <player>` | Trust a player; they are told if online |
| `/builders trust remove <player>` | Withdraw it |
| `/builders trust`, `trust list` | Who you trust, and who trusts you |
| `/builders warehouse <owner>` | Open an empty window into the owner's warehouse; what you put in is theirs when you close it |
| `/builders warehouse give <owner>` | Move the stack in your hand into the owner's warehouse |
| `/builders warehouse give <owner> all` | Move every plain block item |
| `/builders pause <id>`, `resume <id>`, `boost <id>` | Act on a trusted build by its id |

The owner, if online, is told what you stocked when you use `warehouse give`.

The id is shown in Active Builds and tab-completes for builds you may act on. The player must have
played on the server before; a name nobody has used is refused.

## Settings

| Key | Default | Meaning |
|---|---|---|
| `trust.maxPlayers` | `10` | How many players one owner may trust |

## Notes

- Resuming still needs the owner online: the land check is made for the owner, not for the helper.
- Trust is stored in the database and survives restarts. Every change is written at once, off the main
  thread. On [servers sharing a database](../config/database.md#several-servers-on-one-database), each
  server reads the trust lists when it starts.
- `/builders warehouse <owner>` and `warehouse give` load an offline owner's warehouse on demand; if it
  has not loaded yet, the command asks you to try again in a moment.
- You cannot trust yourself, and `trust.maxPlayers` caps how many you trust; `0` lets nobody be trusted.

Related: [Warehouse](warehouse.md), [Speed boost](boost.md)

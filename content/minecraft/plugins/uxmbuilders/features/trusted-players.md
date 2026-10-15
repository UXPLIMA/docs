---
title: Trusted players
order: 209
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
| Stock the warehouse | yes | yes, with `/builders warehouse give` |
| Cancel | yes | **no** |

Cancelling moves the owner's refund, so it stays with the owner. Staff with `uxmbuilders.admin` can
act on any build.

Trust is one-way and covers every build the owner has, now and later.

## Commands

| Command | Does |
|---|---|
| `/builders trust <player>` | Trust a player; they are told if online |
| `/builders trust remove <player>` | Withdraw it |
| `/builders trust list` | Who you trust, and who trusts you |
| `/builders warehouse give <owner>` | Move the stack in your hand into the owner's warehouse |
| `/builders warehouse give <owner> all` | Move every plain block item |
| `/builders pause <id>`, `resume <id>`, `boost <id>` | Act on a trusted build by its id |

The id is shown in Active Builds and tab-completes for builds you may act on. The player must have
played on the server before; a name nobody has used is refused.

## Settings

| Key | Default | Meaning |
|---|---|---|
| `trust.maxPlayers` | `10` | How many players one owner may trust |

## Notes

- Resuming still needs the owner online: the land check is made for the owner, not for the helper.
- Trust is stored in the database and survives restarts. Changes are written in the background.
- `/builders warehouse give` loads an offline owner's warehouse on demand; if it has not loaded yet,
  the command asks you to try again in a moment.

Related: [Warehouse](warehouse.md), [Speed boost](boost.md)

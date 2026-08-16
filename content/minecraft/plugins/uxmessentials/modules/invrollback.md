---
title: Inventory Rollback
order: 9080
description: Snapshots of a player's inventory at death and logout, restorable from a menu.
---

Inventory Rollback is the answer to a player losing an inventory to a lag death, a bad restart or a bug. It
snapshots their inventory at the moments things go wrong, keeps a bounded history in the database, and lets
staff browse and restore one with `/invrestore <player>`.

Module `invrollback` · disabled by default · `modules/invrollback/config.conf`

## Commands

{/* generated:commands */}
| Command | What it does | Permission |
|---|---|---|
| `/invrestore` | Open the inventory-snapshot restore GUI for a player, or export/teleport a chosen snapshot. | `uxmessentials.invrollback.restore` |
{/* /generated */}

## Permissions

{/* generated:permissions */}
| Node | Default | Grants |
|---|---|---|
| `uxmessentials.invrollback.export` | op | /invrestore export \<player> \<index> packs a snapshot into shulker boxes and gives them to you. |
| `uxmessentials.invrollback.restore` | op | /invrestore \<player> opens the inventory-snapshot restore GUI for a player. |
| `uxmessentials.invrollback.teleport` | op | /invrestore tp \<player> \<index> teleports you to where the snapshot was captured. |
| `uxmessentials.module.invrollback` | op | Hot-reload / inspect the invrollback module (inventory snapshots and restore). |
{/* /generated */}

## Settings

{/* generated:settings */}
| Key | Default | What it does |
|---|---|---|
| `capture.on-death` | `true` | snapshot the inventory when a player dies (read before the items drop) |
| `capture.on-logout` | `true` | snapshot the inventory when a player leaves the server |
| `include-enderchest` | `true` | Whether a snapshot also stores the player's ender chest alongside the main inventory, armor and offhand. |
| `retention.max-per-player` | `10` |  |
| `retention.max-age-days` | `30` |  |
{/* /generated */}

## Placeholders

{/* generated:placeholders */}
| Placeholder | Renders |
|---|---|
| `%uxmessentials_invrollback_captured%` | Whether any snapshot has been taken for the player since the last restart (yes/no). |
| `%uxmessentials_invrollback_last_capture%` | How long ago this server last snapshotted the player's inventory, since the last restart. |
| `%uxmessentials_invrollback_last_cause%` | What caused that snapshot: death or logout. |
{/* /generated */}

## Notes

- **The module ships off because every death and logout writes a row,** which is a real cost only worth paying
  on a server whose staff actually restore inventories. It starts capturing the moment you switch it on.
- **The death capture is an observer.** It reads the inventory the instant the player dies, before the items
  drop, and never alters the death, the drops or the message.
- **Snapshots are chosen by list index, where 1 is the newest,** and both restoring and exporting work on an
  offline target because they read stored data.
- **`/invrestore export` packs a snapshot into named shulker boxes** and hands them to you, which is how a lost
  inventory is inspected or given back by hand.
- **Retention has two independent limits:** how many snapshots a player keeps and how old one may get. Either
  can be switched off on its own.

Related: [Vaults](vaults.md), [Moderation](moderation.md), [Player State](playerstate.md)

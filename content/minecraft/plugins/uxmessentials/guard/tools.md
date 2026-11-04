---
title: Tools
order: 1120
description: Seeing borders, asking why, inspecting blocks, history and undo, the denial log, alerts, bypass and status.
icon: wrench
---

Contents: [Borders](#borders) · [Why and test](#why-and-test) · [Inspect](#inspect) ·
[History and undo](#history-and-undo) · [Removing and restoring](#removing-and-restoring) ·
[The denial log](#the-denial-log) · [Alerts](#alerts) · [Bypass](#bypass) · [Refusal messages](#refusal-messages) ·
[Status](#status) · [Backups](#backups)

## Borders

`/guard show [region]` draws borders for you alone; other players see nothing. Without a name it draws every
region within `display.range` blocks (64 by default) and your current selection. The borders go away after
`display.seconds` (20) or with `/guard hide`.

`display.mode` picks how: `lines` (thin glowing blocks along the edges, the default) or `particles`.

## Why and test

`/guard why <action>` is for every player. It answers where they stand: each region asked, in order, what it
said, the rule that decided, and whether that rule is the region's own, a parent's, the template's or the world
default.

`/guard test <player> <action> [region]` is the staff version for anyone, online or not, at your spot or inside
a named region. An offline player is checked with their roles but without permissions.

## Inspect

`/guard inspect` turns inspect mode on and off for you.

- A left or right click on a block does nothing to the block. Instead you get the verdict of every main
  action there (`break`, `place`, `use`, `interact`, `damage`, `item.use`, `item.drop`, `move.enter`) for a player
  with no role and no permission, with the region and rule that decide each one, or `(no rule)`.
- Every refusal you hit yourself is explained as it happens.

## History and undo

Every change to a region is kept: creating, reshaping, every rule, role, behaviour, template, tag, priority,
parent, family, scope and lock change, removal, restoring, and each region of an import.

```text
/guard history spawn
/guard history
/guard undo 412
```

`/guard history [region] [page]` lists changes newest first, with an id, the time, who made it and what changed.
Without a region it lists every region's changes. `/guard undo <id>` puts that region back as it was before the
change. The undo is itself a change in the history, so it can be undone. The [history window](menus.md#history)
does the same with a click.

## Removing and restoring

`/guard remove <region>` removes a region; in the menu it asks first. `/guard restore <region>` brings it back as
it was, from its history.

## The denial log

With `log.enabled = true`, every refused action that a player chose to try is written to the database: when,
who, where, which region and which action. Quiet refusals (picking up an item, standing on a plate, trampling)
are not written. Rows older than `log.keep-days` (14) are pruned.

```text
/guard log
/guard log Steve
/guard log region:spawn 2
```

It is off by default, because a busy server writes many rows.

## Alerts

Staff with `uxmessentials.guard.notify` get a chat line when one player is refused `alerts.threshold` times (8)
within `alerts.window-seconds` (20), at most once per `alerts.cooldown-seconds` (60) for that player. With
`alerts.discord = true` and the [Discord bridge](../integrations/overview.md) on, the alert is posted there too.
Players who hold the notify node themselves never trigger it, and quiet refusals do not count.

## Bypass

`/guard bypass` switches every rule off for you, and the region limits that stop a player: `fly = deny` no
longer grounds you, `glide = deny` no longer stops gliding, and the command lists no longer block you. A
reminder shows on your action bar every `bypass.reminder-seconds` (30) while it is on. It needs
`uxmessentials.guard.bypass`, which only operators have by default.

Bypass does not work inside a [locked region](concepts.md#locked-regions-and-locked-worlds) unless you also hold
`uxmessentials.guard.lock`. Comfort behaviours such as `invincible` still apply to you.

## Refusal messages

A refused action shows one short line, on the action bar by default (`deny-message.channel = chat` moves it to
chat), at most once per `deny-message.cooldown-ms` (1500) per player. The line is `guard.deny` in the language
files. A region can stay silent with `deny-messages = false`, and can word its own entry and exit refusals with
`enter.deny-message` and `exit.deny-message`.

## Status

`/guard status` shows:

- regions, worlds, chunk columns and region sets held in memory;
- checks so far and checks per second;
- whether a world is locked;
- per action family: checks, refusals and the average time a check took in nanoseconds.

The first checks of each chunk include building that chunk's column, so the averages settle once players have
moved around. See [Performance](performance.md).

## Backups

On every start, and before every import and every restore, all regions this server loads are written to
`plugins/uxmEssentials/guard/backups/` as a dated JSON file. A backup identical to the newest one is not written
again, so restarts do not push older backups out. `backups.keep` (10) files are kept.

```text
/guard backups
/guard backups restore 20261104-133239-start.json
```

`/guard backups` lists them, newest first. `restore` first saves the present state as a new backup (named in the
reply), then puts every region in the file back as the file has it:

- a region that was removed since comes back;
- a region that was changed since goes back to how it was;
- a region made since the backup stays;
- a locked region is left alone unless you hold `uxmessentials.guard.lock`.

Each region put back is a change in its history, so one of them can be undone on its own.

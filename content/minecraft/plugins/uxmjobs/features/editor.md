---
title: In-game editor
order: 317
description: Making, changing and deleting jobs from inside the game, and what the editor writes.
icon: pencil
---

`/uxmjobs editor` (`uxmjobs.admin.editor`) opens a [window](../menus/editor.md) with every job and a button that makes
another.

## Making one

Click the lime dye and type an id: lower case letters, digits, a dash or an underscore (`editor.bad-id` otherwise). A
new job starts as the shipped template, comments and all, and its property editor opens. An id a file already carries
answers `editor.exists`.

## The properties

| Button | Writes |
|---|---|
| Name | `name.default` |
| Description | `description.default` |
| Icon | `icon` |
| Curve shape | `curve.shape`: `geometric` or `linear` |
| Curve base | `curve.base`, 1 to 1000000 |
| Curve factor | `curve.factor`, set in hundredths (108 is 1.08) |
| Highest level | `curve.maximum-level`, 1 to 10000 |
| Level cap | `level-cap.default` |
| Level cap node | `level-cap.node` |
| Seats | `seats`, 0 to 10000 |
| Paid in | `currency` |
| Join cost | `join-cost` |
| Leave cost | `leave-cost.default` |
| Requirements | `requirements`, in a list window |

The price table and the rewards have no button, on purpose: a row is a behaviour, a target and an expression per
currency, and a button that flattened it would lie about the file. They stay in the file, whose comments teach the
grammar. A linear curve's `step` is in the file too.

Every save writes the file and reads the content again. Comments survive; keys come back in alphabetical order.

## Deleting

The barrier deletes the job's file after a second click.

## Off

`editor.enabled = false`: no editor windows, no input listener, and the command says `editor.off`. It takes a restart.

Related: [Editor windows](../menus/editor.md)

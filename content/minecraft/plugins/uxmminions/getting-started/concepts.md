---
title: Concepts
order: 103
description: The two rules, the identity, types and levels, scopes, destinations and fuel.
icon: lightbulb
---

## The two rules

A minion never works while its owner is offline, and a minion never keeps a chunk loaded. There is no key, permission or
per type exception for either. Nothing is accrued and nothing is settled for the time in between, and there is no output
cap: playtime is the cap.

## An identity, not a block

Every minion has a stable id, a database row keyed by the minion and not by the block it stands on. It survives a move
and a pickup: the level, fuel, stock, name and skin go with it. Every operator command and placeholder names a minion by
that id, never by a coordinate or an order in a list.

## Nothing is spawned

A minion is drawn with packets to each player in range. No armour stand is spawned and no entity is written to a chunk,
so nobody can take its armour, a piston cannot push it, and it costs no entity tick.

## Types and levels

A type is a file under `types/`: a job, an icon, targets and a ladder of levels. A level is a record of named fields:
interval, range, storage, smelting, sale multiplier, tax, look, fuel cost, and what buying it requires.

## Scope

Limits count per scope: an island where a claim plugin says there is one, otherwise a player. Four alternate accounts on
one island share one cap.

## Output and fuel

Output goes to the minion's own storage (stock kept in the database), a sale, or a chest nearby; the owner picks per
minion. Fuel is a budget in actions: a stopped, full or waiting minion spends nothing.

Related: [Types and levels](../features/types-and-levels.md), [Output](../features/output.md)

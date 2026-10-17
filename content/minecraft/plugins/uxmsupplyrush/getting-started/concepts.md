---
title: Concepts
order: 12
description: Events, runs, items, points, ranks and rewards: the words the other pages use.
icon: lightbulb
---

## Event

An event is a tournament you set up once and can run as often as you like. It has a name, a list
of items that count, a length, and rewards. Each event also has a short **event key**, made from
the name it was created with (`Supply Rush Event #1` becomes `supply-rush-event-1`). The key names
its file, `events/<key>.yml`, and is what the [weekly schedule](../features/auto-start.md) uses.
Renaming an event later does not change its key.

## Run

A run is one round of an event, from the moment it starts until it ends. Every run starts with
empty scores. An event can be run many times, and every finished run is kept in the
[history](../features/history-and-logs.md).

Only one event runs at a time. An event is always in one of four states:

| State | Meaning |
|---|---|
| Draft | Created, never started |
| Active | Running: players can deposit |
| Paused | Stopped for now, with its scores and its remaining time kept |
| Completed | Its last run has ended. Starting it again begins a new run |

## Items and points

The items an event accepts are its **deliverables**. Each one is worth a number of points per item:
a diamond worth 10 points pays 640 points for a full stack. An item with a custom name or lore only
matches that exact item, so a plain diamond never counts as your custom "Event Diamond".

A player's **score** is the points of everything they deposited in the current run.

## Ranks

The leaderboard orders players by score. What happens when two players have the same score is up
to you: they can share the rank, or the one who deposited more items or got there first can win.
See [Ranking](../features/ranking.md).

## Rewards

When a run ends, every rank you configured pays its reward, and everyone else who took part can get
a participation reward. Rewards are console commands and messages, so anything another plugin
offers can be a prize. A winner who is offline gets their reward the next time they join. See
[Rewards](../features/rewards.md).

## History

Every finished run keeps its leaderboard, what each player deposited, and the item list it had,
even after the event itself is deleted. Players browse their own runs from the menu.

Related: [Your first event](first-event.md)

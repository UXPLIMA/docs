---
title: Rewards
order: 403
description: "The rewards window of one track: every reward by level, its state, and claiming with a click."
icon: gift
---

A left click on a track opens its rewards: six rows, 28 rewards to a page, in level order.

## Slots

| Slot | Tile | Click |
|---|---|---|
| 10 to 16, 19 to 25, 28 to 34, 37 to 43 | One reward each, its `icon`; glowing once taken | Left click claims a manual reward that is ready |
| 48 | Previous page | Turns back a page |
| 49 | Feather, `menu.button.back` | Back to the levels window |
| 50 | Next page | Turns forward a page |
| Every other slot | Grey pane | Nothing |

## A reward

| Line | From |
|---|---|
| Title | The reward's `name` |
| Crumb | `menu.reward.crumb`: `Reward` |
| Description | The reward's `lore`, or `menu.reward.default-description` when it has none |
| Needs level | The reward's `level` |
| State | One of the four below |
| Click line | One of the four below |

| State | Shown when | Words | Click line |
|---|---|---|---|
| Claimed | Taken on this climb | `Claimed` | `You already have this one` |
| Ready | A manual reward at or below the player's level | `Ready` | `Click to claim it` |
| Locked | Above the player's level | `Not reached yet` | `Keep earning to reach it` |
| Automatic | An automatic reward not yet given | `Given on its own` | `It arrives the moment you reach the level` |

## Clicking

| Reward | Answer |
|---|---|
| Ready | `reward.claimed`, the lines run, and the window is drawn again |
| Claimed | `reward.already-claimed` |
| Locked | `reward.not-reached`: `That needs level 20. You are 12.` |
| Automatic | `reward.not-claimable`: `That one is given to you on its own.` |

A track with no rewards shows an empty window.

Related: [Rewards](../tracks/rewards.md), [Levels](main.md)

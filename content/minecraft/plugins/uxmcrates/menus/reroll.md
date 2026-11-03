---
title: Reroll
order: 509
description: Look at what an opening gave, keep it, or spend a roll on another draw.
icon: dice-5
---

Opens after a reveal for a player who has a reroll to spend. Three rows, three buttons. The player reads
`reroll.waiting`: until they choose, the reward waits in their pending rewards.

## Slots

| Slot | Tile | Click |
|---|---|---|
| 11 | The reward's item, `Look at what you won` | Says again in chat what was won |
| 13 | Lime dye, `Keep it` | Keeps the reward and closes the window |
| 15 | Ender eye, `Roll again`; the stack size is the rolls left | Spends a roll and draws the table again |
| Every other slot | Grey pane | Nothing |

## The tiles

| Tile | Lines |
|---|---|
| Look and Keep | Crumb `This opening`, `menu.reroll.won-description`, `You won` with every reward of the opening |
| Roll again | Crumb `This opening`, `menu.reroll.again-description`, `Rolls left` |

| Roll result | Message |
|---|---|
| A new draw | `reroll.used` with the new reward, and the window again while rolls are left |
| No roll left, and none can be bought | `reroll.none` |
| The reward was claimed elsewhere | `reroll.taken` |
| The table could give nothing new | `reroll.nothing`: the old reward stands |

Closing the window, logging out or a restart keeps the reward in the waiting rewards.

Related: [Rerolls and milestones](../features/rerolls-and-milestones.md#rerolls)

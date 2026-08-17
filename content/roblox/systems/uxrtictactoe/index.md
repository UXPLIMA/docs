---
title: uxrTicTacToe
order: 26
description: Physical tic tac toe tables with 3x3 to 5x5 boards, a perfect-play bot and in-world leaderboards.
icon: grid-3x3
---

Tables you build in the world. Two players sit down and play; one player can sit down and
play the bot. Marks are real parts placed on the board, not an interface drawn over the
screen.

Boards go from 3x3 up to 5x5, with a configurable number in a row to win. On 3x3 the
hardest bot solves the game outright and cannot be beaten.

## Pages

- [Setup](setup.md)
- [Building a table](table.md)
- [Settings](settings.md)
- [The bot](bot.md)
- [Stats and leaderboards](stats.md)
- [The interface](interface.md)
- [Hooks](hooks.md)

## What it does

| | |
|---|---|
| Tables | Any number, discovered from a tagged folder in the world |
| Board sizes | 3x3, 4x4, 5x5, chosen per table at the seat panel |
| Win condition | Configurable per size: three or four in a row |
| Opponents | Another player, or a bot at three difficulties |
| Move timer | Optional per-move countdown, with automove or pass on timeout |
| Stats | Wins, losses and draws, in `leaderstats` and saved to a profile |
| Leaderboards | In-world boards, global across servers, refreshed on a timer |
| Rewards | Optional currency payout per result |
| Bot avatar | A cosmetic NPC that sits in the opposite chair |

## How a game happens

1. A player sits in `Chair1` or `Chair2` of a table model.
2. The seat panel appears: board size, bot difficulty, and a start button.
3. Either a second player sits down, which starts a human game, or the first player
   starts a bot game.
4. A VS intro plays, then X moves first.
5. Marks are cloned onto the board as parts; the win line is highlighted.
6. After a delay the board clears and the table is free again.

The board geometry is derived from the plate part you built, so nothing needs calibrating:
the server and the client compute the same cell index from the same measurements.

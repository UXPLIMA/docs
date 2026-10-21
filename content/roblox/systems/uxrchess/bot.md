---
title: The bot
order: 4
description: What each ELO preset actually plays like, and why it does not lag the server.
icon: cpu
---

The bot is a negamax search with alpha-beta pruning, move ordering and a material and
position evaluation. It is not a stockfish port and does not pretend to be: it is an
opponent for a Roblox chess table.

## The strength buttons

```lua
EloPresets = { 600, 1000, 1400, 1800, 2200 },
DefaultElo = 1000,
```

Each preset becomes a button. The number is turned into two internal parameters:

| Preset | Search depth | Chance of a deliberate blunder |
|---|---|---|
| Below 600 | 1 | 50 percent |
| 600 to 999 | 2 | 30 percent |
| 1000 to 1399 | 2 | 12 percent |
| 1400 to 1799 | 3 | 4 percent |
| 1800 and above | 4 | none |

Two things follow from that table.

**The blunder chance is what creates a beatable bot.** Even a depth-1 search takes every
free piece, and a beginner cannot beat that consistently. Playing a deliberately worse
move some of the time is what makes 600 feel like 600.

**Presets between the same thresholds play identically.** `1000` and `1399` are the same
bot. Adding `1200` to the list gives players another button, not another opponent. To
create genuinely distinct strengths, pick numbers that cross the thresholds above.

## Depth 4 is the ceiling

Anything at or above 1800 gets depth 4 and no blunders. `2200` on the shipped list is
therefore the same as `1800`, offered as a separate button because players read the number
as the challenge.

A strong club player will beat it. A casual player will not.

## Keeping the server responsive

The search yields periodically rather than running to completion in one go, controlled by
`BotYieldEvery`. Larger means fewer pauses and a longer uninterrupted stretch of work;
smaller means smoother but slower.

Leave it alone unless you can measure a problem. `800` is tuned for depth 4 on a typical
position.

## The move delay

```lua
BotMoveDelay = 0.75,
```

The bot always waits this long, even when it decided in a millisecond. Without it a
depth-1 bot answers before the player's piece has finished sliding, which reads as a bug.

## The avatar

A cosmetic NPC sits opposite. With `Npc.UseFriendAvatar = true` it borrows the appearance
of a random friend of the player, otherwise `Npc.FallbackUsername`.

The game waits up to `BotLoadMaxSeconds`, eight seconds, and then starts with an empty
chair if the avatar has not arrived. The opponent is the search; the model is decoration.

## Do bot games count

Two independent settings:

| Setting | Default | Controls |
|---|---|---|
| `Leaderstats.CountBotWinLoss` | `true` | Wins, losses and draws |
| `Leaderstats.RatedVsBot` | `false` | The ELO rating |
| `Rewards.PayBotGames` | `false` | Currency payouts |

The shipped combination is the sensible one: bot games show up in a player's record but do
not inflate their rating or their wallet.

If you do turn `RatedVsBot` on, the rating change is computed against the bot's preset
number as its rating, so beating 2200 moves a player more than beating 600.

## Testing the engine

The rules, move generation, board and evaluation modules use no Roblox API and ship with
specs:

```lua
require(game.ReplicatedStorage.uxrChessSystem.Chess._spec.Run)()
```

That covers move legality, castling rights, en passant, promotion and the check and mate
detection. Run it after touching anything under `Shared/Chess`.

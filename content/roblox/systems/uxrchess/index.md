---
title: uxrChess
order: 23
description: Physical chess tables with full rules, clocks, an ELO ladder and a search-based bot.
icon: crown
---

Chess played on tables you build in the world, with real pieces that slide to their
squares. Full legal-move generation, castling, en passant, promotion, check, checkmate and
stalemate.

Two players, or one player against a bot at five strengths. Clocks are optional and come
in the usual time controls.

## Pages

- [Setup](setup.md)
- [Building a table](table.md)
- [Settings](settings.md)
- [The bot](bot.md)
- [Rating and leaderboards](rating.md)
- [The interface](interface.md)
- [Hooks](hooks.md)

## What it does

| | |
|---|---|
| Rules | Complete, including castling, en passant, promotion and stalemate |
| Tables | Any number, discovered from a folder you build |
| Opponents | Another player, or a bot at five ELO presets |
| Time controls | Untimed, blitz, rapid and classic, configurable |
| Rating | An ELO ladder with a configurable K factor |
| Stats | Wins, losses, draws and rating, in `leaderstats` and saved |
| Leaderboards | In-world boards, global across servers |
| Camera | Optional lock to a mount you place at the table |

## The part worth knowing about

Board orientation is **calibrated automatically**. The system reads where your pieces
actually are, compares that against the standard starting position, and works out which
way the board is facing.

You do not set a rotation, a flip flag or an offset. Build the board any way round; place
the pieces correctly; it works out the rest. If it cannot, it says so rather than playing
a mirrored game.

## How a game happens

1. Two players sit at `Chair1` and `Chair2`, or one player starts a bot game.
2. The chair nearer the white king plays white. That is derived from your model, not
   configured.
3. The VS intro plays and the clocks start.
4. Pieces tween to their squares; captures move to the edge of the board.
5. Checkmate, stalemate, resignation or a flag fall ends it, the result holds for a few
   seconds, then the board resets and the players stand up.

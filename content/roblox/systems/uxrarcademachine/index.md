---
title: uxrArcadeMachine
order: 20
description: Playable arcade cabinets with eight built-in games, per-game leaderboards and a framework for adding your own.
icon: gamepad-2
---

Cabinets you place in the world. Walk up, press the prompt, and a full-screen game opens.
Eight games ship, each with its own high-score table.

The games are client-hosted and single-player. The server owns the scores.

## Pages

- [Setup](setup.md)
- [Cabinets](cabinets.md)
- [The games](games.md)
- [Adding a game](adding-a-game.md)
- [Scores and leaderboards](scores.md)
- [Settings](settings.md)

## The eight games

| Id | Title | Shape |
|---|---|---|
| `Snake` | Snake | Grid, grows on food, dies on itself |
| `PingPong` | Ping Pong | Paddle against an opponent that gets faster |
| `StarBlitz` | Star Blitz | Rows of descending attackers, three lives |
| `BlockStack` | Block Stack | Falling shapes, cleared lines, increasing speed |
| `SweetMatch` | Sweet Match | Swap to match three, against a timer |
| `MineGrid` | Mine Grid | Reveal without hitting a mine |
| `GhostMaze` | Ghost Maze | Collect pellets, avoid pursuers, power-ups reverse it |
| `FlapDash` | Flap Dash | One button, gaps to fly through |

Every title is original. No trademarked game name appears anywhere in the product, in the
copy or in the code, which is deliberate and worth preserving if you add cabinets.

## What it does

| | |
|---|---|
| Cabinets | Any number, one game each, bound by an attribute |
| Interaction | A `ProximityPrompt` added for you at runtime |
| Input | Keyboard, on-screen controls and swipe, unified |
| Mobile | Every game is playable with touch. Not an afterthought |
| Scores | Personal best per game, saved to a profile |
| Leaderboards | One global top table per game |
| Extending | A new game is one module and one settings entry |

## What the designer builds

A world folder of cabinet models, each carrying a `GameId` attribute. That is all. The
prompt, the tag, the full-screen interface, the controls and the game-over screen are
built at runtime.

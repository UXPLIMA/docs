---
title: Setup
order: 1
description: Install, build one cabinet, and play a game.
icon: play
---

Install with the standard four steps in
[Installing a system](../platform/installation.md): drag `uxrArcadeMachineSystem` into
**ServerScriptService** and press Play once.

Then build somewhere to play.

## 1. Build the world folder

```
Workspace/
  uxrArcadeMachineWorkspace/
    Stands/
```

## 2. Put one cabinet in it

A model containing at least one `BasePart`. Name the part `ScreenPart` by convention.

Set a `GameId` string attribute on the model:

```
GameId = "Snake"
```

Any of the eight ids works: `Snake`, `PingPong`, `StarBlitz`, `BlockStack`, `SweetMatch`,
`MineGrid`, `GhostMaze`, `FlapDash`.

## 3. Press Play and walk up to it

A prompt appears saying **Play**. Press it and the game opens full screen.

The game shows a GET READY overlay and waits for your first input before anything moves, so
you cannot lose a life while the screen is still fading in.

## 4. Finish a run

Play until game over. The final screen shows the score, your best, and three buttons: play
again, leaderboard, exit.

Your best is saved. In Studio that needs **Game Settings, Security, Enable Studio Access to
API Services**; without it the run still plays and the score is not kept.

## 5. Check it on a phone

Press Play, switch the Studio device emulator to a phone, and try the same game. Every game
ships with on-screen controls, and this is the fastest way to confirm the layout suits your
cabinet's audience.

## 6. Build the rest of the row

Copy the cabinet, change the `GameId`. Eight cabinets is eight attributes.

<Callout type="tip" title="Two cabinets can run the same game">

Nothing is stored per cabinet. Two `Snake` machines share one leaderboard and one personal
best per player, so a busy arcade can have several of the popular one without splitting the
scores.

</Callout>

## 7. Publish

Leaderboards only fill in the live game: they read ordered stores that a Studio session
does not meaningfully write to. Expect them empty until you publish.

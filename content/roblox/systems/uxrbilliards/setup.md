---
title: Setup
order: 1
description: Install, tag a table, and take a shot.
icon: play
---

Install with the standard four steps in
[Installing a system](../platform/installation.md): drag `uxrBilliardsSystem` into
**ServerScriptService** and press Play once.

Then build a table.

## 1. Build the table model

The full contract is in [Building a table](table.md). The two required parts:

| Part | Purpose |
|---|---|
| `Surface` | The playable cloth. Its size and rotation define the whole play area |
| `JoinPad` | Where the prompt appears |

Add `Pockets`, `Seats` and `CameraPart` too; the game is much better with all three.

## 2. Tag it

Tag the model `BilliardsTable` with the Studio tag editor. Discovery is by tag, so the
model can live anywhere in the world.

## 3. Check the balls

The ball parts ship inside the package at
`ServerScriptService/uxrBilliardsSystem/Storage/Assets/Balls`, named `WhiteBall` and `1`
through `15`.

They are cloned per table at startup. Re-skin them there, once, for every table.

## 4. Press Play and join

Walk to the join pad and press **Join Pool Game**. Your movement is frozen and you are
placed at a seat.

Alone, you get solo practice and an AI option. With a second player, the match starts.

## 5. Take a shot and watch the aim line

Aim, set the power, shoot. The line the client draws should match where the balls actually
go, because both come from the same simulation.

If they disagree, the `Surface` part is probably including the wooden rail: the simulation
bounces off the edge of the surface, not off your model's cushions.

## 6. Tune the feel

Three numbers in `Shared/Billiards/Config.luau` decide how the game plays:

| Field | Default | Try |
|---|---|---|
| `params.friction` | `1.8` | Lower for a faster, heavier table |
| `params.restitution` | `0.70` | Lower for less pinball off the cushions |
| `maxPower` | `28` | Higher for a more dramatic break |

Change one at a time and break a rack after each.

## 7. Price the shop

The five cues ship at zero coins, so the shop gives everything away. Set real costs in
`Shared/Billiards/Cosmetics.luau` before launch, or the coin payouts have no purpose. See
[Rating, rewards and cosmetics](economy.md).

## 8. Set the identity fields

| Field | Default | Change it to |
|---|---|---|
| `Config.ai.FallbackUsername` | a specific account | One of yours |
| `Config.ai.difficulty` | `"medium"` | Whatever suits your players |
| `Config.coinProducts` | empty | Your Developer Product ids, if selling coins |
| `Config.vipGamepass` | `0` | Your Gamepass id, if you have one |

## 9. Publish

Leaderboards only accumulate in the live game. In Studio the global boards read empty,
which is correct.

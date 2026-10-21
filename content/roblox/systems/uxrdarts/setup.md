---
title: Setup
order: 1
description: Install, tag a court, and throw a leg of practice.
icon: play
---

Install with the standard four steps in
[Installing a system](../platform/installation.md): drag `uxrDartsSystem` into
**ServerScriptService** and press Play once.

## 1. Build a court

A model containing a `Board`, and optionally an `Avatar` part where the opponent stands.
Anything else about it is scenery.

## 2. Tag it

Tag the model `DartsCourt` with the Studio tag editor. Discovery is by tag, so the court can
live anywhere in the world.

## 3. Press Play and start a practice leg

Walk up and use the prompt. The setup panel offers the six presets and, for the AI, the
five tiers.

Pick **Practice**. That is 501 with no double in and no double out, which means a leg
always ends rather than stalling on a double you cannot hit.

## 4. Throw

Aim, time the power, throw. Three darts is a visit; the score comes off the total.

Watch where the darts land relative to where you aimed. That gap is the wobble, and it
narrows as your accuracy rises. See [Throwing](throwing.md).

## 5. Play the AI

Start again with the AI enabled at `Noob`. It should be beatable by somebody who has never
played darts. If it is not, `Config.AITiers.Noob.accuracy` is the number to lower.

Then try `Pro`. It should not be.

## 6. Set the identity fields

| Field | Default | Change it to |
|---|---|---|
| `Config.AIPersonas` | Four names with very low user ids | Accounts whose avatars you have looked at |
| `Settings.Debug` | `true` | `false` before release |
| `Settings.Leaderstats.StartElo` | `1000` | Keep it equal to `Config.Leaderboard.StartElo` |

## 7. Build the leaderboards

```
Workspace/
  uxrDartsWorkspace/
    Leaderboards/
      WinsLeaderboard
      EloLeaderboard
      EightiesLeaderboard
      NineDartersLeaderboard
```

One model per entry in `Config.Leaderboard.Stats`. The row contract is in
[Stats and leaderboards](stats.md).

## 8. Publish

Global boards only accumulate in the live game. In Studio they read empty, which is
correct.

---
title: uxrBowling
order: 22
description: Ten-pin lanes with a working pinsetter, timed aiming meters, slow-motion replays and full scoring.
icon: circle
---

Bowling lanes with the machinery: a pinsetter that lifts and racks, a sweep bar that clears
fallen pins, and a ball return that brings the ball back to the player.

Up to four players per lane, ten frames each, scored the standard way with strikes, spares
and a tenth frame that gives extra deliveries.

## Pages

- [Setup](setup.md)
- [Building a lane](lane.md)
- [Playing a game](gameplay.md)
- [The throw](throw.md)
- [Stats and leaderboards](stats.md)
- [Settings](settings.md)

## What it does

| | |
|---|---|
| Lanes | Discovered from a world folder. Lanes come in pairs per model |
| Players | One to four per lane, with a lobby countdown |
| Scoring | Standard ten-pin, including the tenth frame |
| Aiming | Three timed meters: direction, power and spin |
| Physics | Real Roblox physics, with tuned densities and friction |
| Machinery | Pinsetter, sweep and ball return, animated |
| Bumpers | Optional gutter guards, toggled in the lobby |
| Replays | Slow motion after each shot, with its own camera and music |
| Stats | Wins, losses, games, high score, strikes and spares |
| Leaderboards | In-world boards, global across servers |

## Real physics, unlike the pool table

The billiards system in this range simulates its own physics because pool needs a
predictable aim line. Bowling does not: the appeal is precisely that ten pins scatter in a
way nobody can predict.

So the ball and the pins are real Roblox parts with tuned density, friction and elasticity,
and pin counting is done by measuring which pins are still upright and roughly where they
started.

The consequence is that the physics settings matter. A ball five times heavier than the
pins goes through them without a sound; a light one bounces off. The shipped values are
tuned, and [The throw](throw.md) explains what each one does before you change it.

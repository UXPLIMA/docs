---
title: uxrBilliards
order: 21
description: Eight-ball pool with a custom physics simulation, full rules, an ELO ladder and cue cosmetics.
icon: circle-dot
---

Pool tables you place in the world. Walk up, join, aim, set the power and take the shot.
Full eight-ball rules: groups, fouls, ball in hand, called pockets and a losing scratch on
the black.

The physics is a purpose-built simulation, not Roblox physics. That is the design decision
everything else follows from.

## Pages

- [Setup](setup.md)
- [Building a table](table.md)
- [Rules and play](rules.md)
- [Physics and aiming](physics.md)
- [The AI opponent](ai.md)
- [Rating, rewards and cosmetics](economy.md)
- [Configuration](config.md)

## What it does

| | |
|---|---|
| Tables | Any number, found by tag |
| Modes | Two players, solo practice, or against the AI |
| Rules | Eight-ball, groups assigned on the first legal pot, called pockets |
| Physics | A deterministic simulation at a fixed timestep, run on the server |
| Aiming | A projected cue line with spin, and a power bar |
| Shot clock | Configurable, optional in solo |
| Rating | ELO, plus wins, losses and draws |
| Leaderboards | In-world boards, global across servers |
| Cosmetics | Cue skins, with an optional coins economy and a VIP gamepass |

## Why the physics is custom

Roblox physics is a rigid-body simulation tuned for characters and vehicles. Pool needs
consistent rolling friction, predictable cushion bounce, and an aim line that matches
where the ball will actually go.

Simulating it directly gives all three: the same shot always plays out the same way, the
aim projection is computed from the same code that will move the balls, and a table can
resolve a whole shot in a fraction of a second rather than over twelve seconds of real
time.

It also means the server is the authority. The client draws the aim; the server decides
where the balls end up.

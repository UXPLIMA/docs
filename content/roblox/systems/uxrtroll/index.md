---
title: uxrTroll
order: 34
description: Seven paid trolls players can buy against one player or the whole server.
icon: bomb
---

Players buy a troll with Robux and it lands on somebody. Kill, slow, freeze, set on fire,
fling, explode or kick, aimed at the one player they are watching or at everybody at once.

Fourteen developer products, a spectate panel to pick a target, a server cooldown so one
wallet cannot empty your game, and an immunity list for the people who should never be hit.

## Pages

- [Setup](setup.md)
- [The seven trolls](trolls.md)
- [Targeting and immunity](targeting.md)
- [Products and credits](products.md)
- [The interface](interface.md)
- [Hooks and the API](hooks.md)
- [Settings](settings.md)

## What it does

| | |
|---|---|
| Trolls | Kill, Slow, Freeze, Fire, Fling, Explode, Kick |
| Scopes | One watched player, or everyone |
| Spectating | Browse the server, camera follows, buy against who you see |
| Immunity | A user id list and a live attribute |
| Cooldown | A server-wide gap between server-wide trolls |
| Credits | A purchase whose target left is banked, not lost |
| Chat | A configurable line when a troll lands |
| Hooks | One hook for reacting in your own code |
| API | Trigger trolls and grant credits from your own scripts |

## The shape of it

Every troll is a row in `TrollSettings.Actions` with two developer product ids: one for the
single target button, one for the whole server button. Prices are never written in config,
because they are read live from Roblox.

A player picks a target in the panel, presses a button, buys the product, and the effect
lands. Everything is enforced on the server: the client asks, and the server decides.

<Callout type="info" title="This system is built on the newer UXR skeleton">

Its folders are `Configuration`, `Core`, `Logic`, `Assets` and `Interface`, rather than the
`Shared` and `Server` split used by the other systems in this section.

The [shared platform pages](../platform/) still describe the ideas correctly; the paths
differ. Where they do, this section says so.

</Callout>

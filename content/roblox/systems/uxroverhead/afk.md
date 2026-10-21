---
title: AFK and health
order: 6
description: Window-focus AFK detection, the timer, transparency, and the health bar.
icon: moon
---

## AFK detection

```lua
AFKModule = true,
AFKBodyTransparency = true,
```

A player is marked AFK when their **game window loses focus**, and unmarked when it comes
back. That is detected on their own client and reported to the server.

It is the right signal. Idle-time detection catches a player who is reading; focus loss
catches a player who has switched to another tab, which is what AFK actually means.

## What it shows

The `AFKFrame` row from the layout becomes visible, with a running timer:

```lua
{ Template = "AFKFrame", Content = "AFK - {AFKTime}", Order = 1 },
```

`{AFKTime}` counts up as minutes and seconds. The frame needs two labels inside it:

| Instance | Purpose |
|---|---|
| `AFKLabel` | The main text |
| `AFKBLabel` | The backing or shadow copy |

Both are filled with the same text, which is how a stroked or drop-shadowed label is built
without a `UIStroke`.

## Body transparency

```lua
AFKBodyTransparency = true,
```

An AFK player's character becomes transparent, like a forcefield. Their body parts return
to normal when they come back.

This is more useful than it sounds on a roleplay server: an AFK player standing in a
doorway is visibly not there rather than apparently ignoring everybody.

Set it to `false` if your game has combat and a transparent player would be an unfair
target, or an unfair advantage.

## Turning it off

`AFKModule = false` disables detection entirely. The `AFKFrame` layout row can stay; it
simply never becomes visible.

## The health bar

The `FrameBar` layout row is the health bar. It follows the character's `Humanoid`, so it
updates as they take damage and heal.

Remove the `FrameBar` entry from the layout to hide it. There is no separate setting.

## The Roblox default display

```lua
HideRobloxDisplayTag = true,
```

On by default: the built-in name and health display above each character is hidden, so the
only thing above a head is this system's nametag.

Turn it off and you get both, which is almost never what you want. The one case where it
helps is diagnosing whether the system is running at all: if you see the Roblox default,
the nametag has not been applied.

## What happens on respawn

The nametag is rebuilt when a character loads. AFK state survives it, so a player who dies
while away is still marked AFK on their new character.

## Performance

AFK detection is one client-side event per state change, not a poll. The health bar follows
a humanoid property. Neither is a per-frame cost, and neither scales with the number of
players.

The expensive part of this system is the condition checks behind ranks and tags, which is
what the caches in [Conditions](conditions.md) exist for.

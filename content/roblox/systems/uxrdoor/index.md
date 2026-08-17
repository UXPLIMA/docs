---
title: uxrDoor
order: 31
description: Swinging, sliding and double doors with keycard access, zone terminals and lockdown.
icon: door-open
---

Doors that open for the right people. Swing, slide and two-panel motions, access by team,
group rank, gamepass, keycard, user id or level, and a terminal that operates whole zones
at once.

Every door in your world is one line of configuration and two tags.

## Pages

- [Setup](setup.md)
- [Tagging a door](doors.md)
- [Motion](motion.md)
- [Access](access.md)
- [Zones and the terminal](zones.md)
- [Prompts and effects](prompts.md)
- [Settings](settings.md)

## What it does

| | |
|---|---|
| Discovery | By tag, with a `DoorId` attribute pointing at a config entry |
| Motion | Swing, slide or double, with per-panel control |
| Access | Seven factor types, combined as OR, reusable as named presets |
| Keycards | A tool by name, optionally required to be equipped |
| Touch doors | Open on contact, no prompt |
| Auto close | Per door, in seconds |
| Zones | Groups of doors, opened, closed or locked down from a terminal |
| Prompts | Four styles, held rather than clicked |
| Feedback | Keycard lights that flash green or red, a beam, a highlight |
| Mobile | An on-screen button, and prompts sized as a fraction of screen height |

## The shape of it

A door in the world is a `Model` with two things: the `uxrDoor` tag and a `DoorId`
attribute. Everything else about how it behaves is an entry in `Config/Doors.luau` keyed by
that id.

That separation is what makes thirty doors manageable. Ten identical office doors share one
`DoorId`, so changing their access changes all ten.

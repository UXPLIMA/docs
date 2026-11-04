---
title: HUD plugins
order: 1306
description: BetterHud, MythicHUD, HappyHUD, CustomNameplates and others drawn on Bedrock.
icon: gauge
---

HUD plugins draw on Java by putting pictures of their font into a boss bar's text and moving them with space letters.
On Bedrock:

| Part | Becomes |
|---|---|
| The pictures | Glyphs of the pack, like any font picture |
| Space letters that move right | Glyphs with the same width, so Bedrock measures them |
| Space letters that move left | Left out: Bedrock has nothing to move text left |
| The boss bar | A full white bar whose fill the `hud-plugins` pack draws see-through, so only the pictures show |

A boss bar whose text holds no ordinary letter counts as a HUD bar. Its progress and style changes are held back.
Every boss bar shares the empty bar's texture; a server with no boss bars of its own can add
`textures/ui/empty_progress_bar` to `hud-plugins.transparent-textures` in [gui.conf](../config/gui-conf.md).

Since left moves are lost, a HUD laid out with them sits differently on Bedrock. A player can switch the server's HUD
off with `/bedrock feature hud-plugins off` when it is allowed.

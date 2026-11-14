---
title: HUD plugins
order: 1306
description: CustomNameplates, dialogue boxes and other HUD plugins drawn on Bedrock where Java draws them.
icon: gauge
---

HUD plugins draw on Java by putting pictures of their font into an action bar or a boss bar and moving them into
place with space letters, some of them moving left. Bedrock's text cannot move left, so uxmBedrock lays such a line
out as Java does and the uxmBedrock Geyser extension draws every picture and run of letters at its own place.

| Part | Becomes |
|---|---|
| The pictures | Pictures of the pack, drawn where Java draws them |
| Letters in a font that only moves Java's letters up or down | Java's letters, moved as the font moves them |
| Space letters | The places of the next pieces, left moves too |
| An action bar HUD | Drawn from the screen's bottom as Java draws the action bar; Bedrock's own action bar stays empty |
| A boss bar HUD | Drawn from the screen's top as Java draws boss bar text, every next bar 19 units lower |

A boss bar whose text moves pieces about counts as a HUD bar. Bedrock gets the bar without text, or no bar at all
when Java draws none: when every sprite of its colour is see-through in the server's packs (as CustomNameplates makes
its yellow bar) or when its text holds no ordinary letter.

Without the uxmBedrock Geyser extension, a HUD bar falls back to a full white bar whose fill the `hud-plugins` pack
draws see-through, with left moves left out. Every boss bar shares the empty bar's texture; a server with no boss bars
of its own can add `textures/ui/empty_progress_bar` to `hud-plugins.transparent-textures` in
[gui.conf](../config/gui-conf.md).

## Plugins

| Plugin | On Bedrock |
|---|---|
| CustomNameplates | Boss bar backgrounds, name tags and chat bubbles. A bubble's background pieces stand a unit apart, since a hologram still cannot move left |
| Dialogue plugins on the action bar | The whole box and its text |
| BetterHud, MythicHUD | Not drawn. Both place their HUD with core shaders, which Bedrock does not have, and both skip Bedrock players by default (`disable-to-bedrock-player`, `disableGeyserPlayers`) |

A player can switch the server's HUD off with `/bedrock feature hud-plugins off` when it is allowed.

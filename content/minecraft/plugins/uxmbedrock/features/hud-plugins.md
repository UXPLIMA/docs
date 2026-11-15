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
| BetterHud, MythicHUD | Pictures and text placed where their shaders place them (see below) |

## Shader HUDs

BetterHud and MythicHUD write their HUD into boss bars with font letters that Java would draw far below the screen,
and a text shader in their pack moves each letter to a point of the screen: a share of its width and height, such as
35 % across and the bottom, and a place from there. Bedrock has no shaders, so uxmBedrock reads the shader the server's
packs give Java when it builds the pack, and places every piece of such a bar from the same point, at the same place,
in the same layer and opacity.

| Part | Becomes |
|---|---|
| The shader's points of the screen | Points of the screen on Bedrock, at the same shares of its width and height |
| A piece's place from its point | The same place, in Java's units |
| MythicHUD's scales | Pictures drawn that much larger; text keeps Java's size |
| BetterHud's opacity and layers | The same opacity, and higher layers drawn over lower ones |
| The shader's pictures | Pictures of the pack, one for each image however many places it is written at |

Both plugins skip Bedrock players by default. Each pack build names the setting to change in the console and in the
pack report (`/uxmbedrock pack-report`):
`disable-to-bedrock-player: false` in `BetterHud/config.yml`, `disable-geyser-players: false` in
`MythicHUD/config.yml`. Java runs one text shader, so with both plugins on one server only MythicHUD's HUD is placed.
BetterHud's rainbow effect is drawn on Bedrock: every picture and letter of a rainbow layout takes a bright colour
that changes once a second, as on Java. Java colours each corner of a letter on its own, so its letters blend from
one colour to another; on Bedrock each letter and picture is one colour, picked from twelve hues. The colours reach
the pack with the build after a player first sees them, so they show from the next join. The wave and tiny rainbow
effects are not drawn; those letters stand still.

A font with more letters than Bedrock's font pages hold is left out and named in the pack report, so it does not
fill the pages other plugins need. BetterHud's own `default` font is one: it carries over a hundred thousand letters.
Its letters are not drawn on Bedrock, but the build measures each one as Java does, so the pieces around them stand
where Java puts them.

Space letters that move the next letter forward by whole units, such as MythicHUD's `spaces` font, share five glyphs
of the pack whatever their width, so a font of a thousand widths does not fill the font pages.

A player can switch the server's HUD off with `/bedrock feature hud-plugins off` when it is allowed.

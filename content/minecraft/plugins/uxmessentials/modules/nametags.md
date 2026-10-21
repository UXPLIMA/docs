---
title: Nametags
order: 9130
description: 'The name above each player''s head: prefixes, suffixes, colours and distance-based hiding.'
icon: tag
---

The nametags module draws what floats above a player's head. Because it is packet-based it paints rank prefixes
and suffixes without the vanilla team quirks, and it can hide names entirely past a distance or for particular
players. It registers no commands: everything is config.

Module `nametags` · disabled by default · `modules/nametags/config.conf`

## Permissions

{/* generated:permissions */}
| Node | Default | Grants |
|---|---|---|
| `uxmessentials.module.nametags` | op | Hot-reload / inspect the nametags module (the name shown above a player). |
{/* /generated */}

## Settings

{/* generated:settings */}
| Key | Default | What it does |
|---|---|---|
| `hide-vanilla-name` | `true` | Hide the vanilla above-head player name for a wearer while their custom nametag is live, so a viewer sees only the custom nametag instead of both (a double name). This parks the wearer's name in a shared scoreboard team whose name-tag visibility is NEVER, re-applied automatically after the scoreboard module switches a player onto a per-player board. Default true. Set false to keep BOTH the custom nametag AND the vanilla name. Minor interaction: a viewer just outside the custom nametag's view-range sees no name at all rather than the vanilla one. |
| `refresh-ticks` | `20` | How often every wearer's nametag is re-rendered, in server ticks (20 ticks = one second). Must be positive. This is the GLOBAL cadence; the per-tick re-selection is what makes a permission/world/gamemode/sneak/vanish change take effect. |
| `nametag.lines` | `[ "<value>{player}</value>" ]` | The nametag lines, joined top to bottom with newlines into the single TextDisplay. An empty list means "no nametag". \{player} is replaced with the wearer's name; PlaceholderAPI placeholders are expanded per viewer. |
| `nametag.appearance.billboard` | `"CENTER"` | How the display faces the viewer: CENTER (always faces, recommended), FIXED, VERTICAL, or HORIZONTAL. |
| `nametag.appearance.scale` | `1.0` | Uniform text scale (1.0 = vanilla size). Must be > 0. |
| `nametag.appearance.y-offset` | `0.3` | How far above the wearer's head the nametag floats, in blocks. |
| `nametag.appearance.see-through` | `false` | Whether the text renders through blocks. |
| `nametag.appearance.text-shadow` | `false` | Background panel colour as hex (#RRGGBB or #AARRGGBB); omit for the vanilla translucent background. background-color = "#000000" Background opacity 0..255 (folded into the background colour's alpha); needs a background-color to apply. background-opacity = 64 Whether the text is drawn with a drop shadow. |
| `nametag.visibility.show-when` | `""` | A condition gating whether this wearer gets a nametag at all (see shared/display/ConditionParser for the grammar). Blank = always show. |
| `nametag.visibility.hide-while-sneaking` | `false` | Hide the nametag from EVERYONE while the wearer is sneaking. |
| `nametag.visibility.respect-vanish` | `true` | Hide the nametag from viewers who cannot see the wearer (vanish). Honoured when the presence module is on; with presence off nothing is hidden. |
{/* /generated */}

## Placeholders

{/* generated:placeholders */}
| Placeholder | Renders |
|---|---|
| `%uxmessentials_nametags_format%` | The nametag format the player wears, or a dash when they wear none. |
| `%uxmessentials_nametags_shown%` | Whether the player wears a nametag at all (yes/no). |
{/* /generated */}

## Notes

- **A nametag can carry several lines**, typically a rank prefix above the name and a title below it.
- **Name hiding is the reason to reach for this module** on an event or hardcore server: names can disappear
  past a distance, or for a chosen set of players, without touching anything else.
- **Vanished players are honoured**, because the vanish module is the single authority every surface reads.

Related: [Scoreboard](scoreboard.md), [Tablist](tablist.md), [Vanish](vanish.md)

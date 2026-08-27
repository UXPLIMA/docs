---
title: Scoreboard
order: 9210
description: A per-player sidebar drawn from packets, with animated lines and no vanilla objective conflicts.
icon: list-ordered
---

The scoreboard module draws the sidebar on the right of the screen. It is packet-based, so it is genuinely
per-player, never touches the vanilla scoreboard objectives other plugins fight over, and needs no client mod.
Players toggle their own sidebar with `/scoreboard` and open their settings panel with `/scoreboard gui`.

Module `scoreboard` · disabled by default · `modules/scoreboard/config.conf`

## Commands

{/* generated:commands */}
| Command | What it does | Permission |
|---|---|---|
| `/scoreboard` (`/sb`) | Toggle whether you see the scoreboard display. | `uxmessentials.scoreboard.use` |
{/* /generated */}

## Permissions

{/* generated:permissions */}
| Node | Default | Grants |
|---|---|---|
| `uxmessentials.module.scoreboard` | op | Hot-reload / inspect the scoreboard module (per-player sidebar and tablist on uxmlib-hud). |
| `uxmessentials.scoreboard.gui` | everyone | /scoreboard gui (and the scoreboard entry on the /uxmess gui hub) to open the per-player scoreboard settings panel: the show/hide toggle. |
| `uxmessentials.scoreboard.use` | everyone | /scoreboard (alias /sb) to toggle whether you see the scoreboard display. |
{/* /generated */}

## Settings

{/* generated:settings */}
| Key | Default | What it does |
|---|---|---|
| `refresh-ticks` | `20` | How often every viewer is re-rendered, in server ticks (20 ticks = one second). Must be positive. This is the GLOBAL refresh cadence shared by every board; per-board refresh intervals are not yet honoured. With the back-compat single-board shape below, the cadence is read from scoreboard.refresh-ticks instead. |
| `scoreboard.title` | `"<h:'uxmEssentials'>"` | The sidebar heading. Leave blank for no heading. |
| `scoreboard.lines` | `[...]` | Candidate lines, top to bottom. Up to 128 may be authored; conditions and hide-when-empty are evaluated per viewer, then the first 15 visible rows are shown. A blank string is a real spacer with its own stable identity. |
| `scoreboard.hide-score-numbers` | `true` | Hide the red score numbers vanilla draws down the right edge of each line; the clean, modern look. Set to false to show the numbers. |
| `scoreboard.refresh-ticks` | `20` | The global refresh cadence for this single-board shape, in server ticks (20 = one second). Must be positive. |
| `scoreboard.world-blacklist` | `[]` | World names where the sidebar is suppressed entirely. |
{/* /generated */}

## Placeholders

{/* generated:placeholders */}
| Placeholder | Renders |
|---|---|
| `%uxmessentials_scoreboard_board%` | The board the player's sidebar is drawn from, or a dash when they are shown none. |
| `%uxmessentials_scoreboard_visible%` | Whether the player has the sidebar showing (yes/no). |
{/* /generated */}

## Notes

- **The HUD modules ship off.** A sidebar is the most visible thing a plugin can take over and many servers
  already draw one, so `scoreboard`, `tablist` and `nametags` each wait to be switched on.
- **Lines take three kinds of content:** built-in `{tokens}` such as the online count and TPS, which need no
  other plugin; any PlaceholderAPI `%...%` when PAPI is installed; and animated shapes paced by `refresh-ticks`.
- **A module can carry several formats and pick one per player by condition,** so staff, donors and everyone
  else can see different sidebars without a second module.
- **A line belongs to a viewer, not to the board.** You may author up to 128 candidate lines. Conditions and
  `hide-when-empty` are evaluated for each player, and the first 15 that survive are the ones they see. A line
  that vanishes for one player does not shift anybody else's board.
- **Give a line a stable id when the layout changes often.** Instead of a plain string, write
  `{ id = "balance", text = "<gray>Balance", hide-when-empty = true }`. The renderer then updates or moves that
  exact row instead of rebuilding the rows around it. Plain strings still work and are given an id automatically.
- **A value can sit on the right.** Write `"<gray>Balance||<gold>{coins}"` and the right half is rendered
  separately and sent as that row's number format, so it lines up against the right edge. A typed line says the
  same thing with `number-format = { type = "fixed", text = "<gold>{coins}" }`.
- **It yields to another plugin's sidebar.** If something else displays its own objective in the sidebar slot,
  uxmEssentials stops drawing; when that objective is cleared, its board comes back on the next refresh.
- **`world-blacklist` suppresses the sidebar** where it would be in the way, a minigame world for instance.

Related: [Tablist](tablist.md), [Nametags](nametags.md), [Communication](communication.md)

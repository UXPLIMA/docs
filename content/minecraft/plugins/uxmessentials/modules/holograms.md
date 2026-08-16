---
title: Holograms
order: 9060
description: Floating displays built from native display entities, with pages, click actions and per-player visibility.
---

A hologram is a floating display you place in the world: a welcome sign at spawn, a live leaderboard by the
arena, a rotating rules board, a nameplate over an NPC. They are built from the display entities Minecraft
ships rather than from a packet trick, so they survive restarts and stay in sync without a client mod.
Everything about one is edited live from `/hologram` and stored in the database.

Module `holograms` · enabled by default · `modules/holograms/config.conf`

## Commands

{/* generated:commands */}
| Command | What it does | Permission |
|---|---|---|
| `/hologram` | Create and manage native-Display holograms. | `uxmessentials.hologram.use` |
{/* /generated */}

## Permissions

{/* generated:permissions */}
| Node | Default | Grants |
|---|---|---|
| `uxmessentials.hologram.action` | everyone | /hologram action and clickcommand: change what a hologram runs when clicked. |
| `uxmessentials.hologram.appearance` | everyone | /hologram billboard, background, glow, opacity, shadow, linewidth, viewrange, alignment, seethrough, growup, item, block, head and entity: change how a hologram looks. |
| `uxmessentials.hologram.create` | everyone | /hologram create and copy. Held by default alongside the base node; negate it to leave an operator editing only the holograms that already exist. |
| `uxmessentials.hologram.delete` | everyone | /hologram delete. Held by default alongside the base node; the capability most worth negating for build staff. |
| `uxmessentials.hologram.edit` | everyone | The line and page content of a hologram (addline, setline, insertline, removeline, page, leaderboard, linknpc, unlinknpc). |
| `uxmessentials.hologram.move` | everyone | /hologram movehere, moveto, center, teleport and rotate: change where a hologram sits. |
| `uxmessentials.hologram.use` | op | /hologram to create, edit, move, list and delete native-Display holograms. |
| `uxmessentials.hologram.view` | everyone | /hologram list, info and nearby: read-only inspection. |
| `uxmessentials.hologram.visibility` | everyone | /hologram visibility, visibilitydistance, show, hide, blacklist and unblacklist: change who sees a hologram. |
| `uxmessentials.holograms.gui` | op | /hologram (no args) opens the holograms management GUI. |
| `uxmessentials.module.holograms` | op | Hot-reload / inspect the holograms module (native-Display holograms behind /hologram). |
{/* /generated */}

## Settings

{/* generated:settings */}
| Key | Default | What it does |
|---|---|---|
| `damage-indicators.enabled` | `false` |  |
| `damage-indicators.show-for-players` | `true` |  |
| `damage-indicators.show-for-mobs` | `true` |  |
| `damage-indicators.show-heal` | `true` |  |
| `damage-indicators.duration-ticks` | `20` |  |
| `damage-indicators.damage-format` | `"<red>-{amount}"` |  |
| `damage-indicators.crit-format` | `"<gold><bold>-{amount} ✦"` |  |
| `damage-indicators.heal-format` | `"<green>+{amount}"` |  |
{/* /generated */}

## Placeholders

{/* generated:placeholders */}
| Placeholder | Renders |
|---|---|
| `%uxmessentials_holograms_count%` | How many holograms are placed. |
{/* /generated */}

## Notes

- **A hologram is a stack of lines, and a line can be five things:** text, a floating item, a block, a player
  head, or a miniature entity. Text lines resolve MiniMessage and placeholders, which is what makes a live
  leaderboard possible.
- **Visibility has a mode and per-player overrides.** `ALL`, `PERMISSION <node>` or `MANUAL` sets the default
  audience; `show`, `hide` and the blacklist override it for individual players.
- **Clicks run an ordered action chain** keyed by `left`, `right` or `any`, built step by step and reordered in
  place, so one hologram can teleport, message and run a command in a fixed sequence.
- **`linknpc <npc>` turns a hologram into an NPC nameplate** that tracks the NPC, which is how a multi-line
  rank, title and status plate is built. `unlinknpc` detaches it.
- **Pages rotate content on a timer,** so a rules board can cycle without a second hologram.

Related: [NPCs](npc.md), [Menus](../menus/engine.md), [Scoreboard](scoreboard.md)

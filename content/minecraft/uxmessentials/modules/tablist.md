---
title: Tablist
order: 9260
description: The tab-list header, footer, column layout and player skins, drawn from packets.
---

The tablist module shapes the player list: the multi-line header and footer, how players are arranged into
columns, and the skin rendered next to each name. Like the rest of the HUD it is drawn from packets rather than
vanilla scoreboard teams, and it registers no commands: everything is config.

Module `tablist` · disabled by default · `modules/tablist/config.conf`

## Permissions

{/* generated:permissions */}
| Node | Default | Grants |
|---|---|---|
| `uxmessentials.module.tablist` | op | Hot-reload / inspect the tablist module (the player list header, footer and rows). |
{/* /generated */}

## Settings

{/* generated:settings */}
| Key | Default | What it does |
|---|---|---|
| `refresh-ticks` | `20` | How often every viewer is re-rendered, in server ticks (20 ticks = one second). Must be positive. This is the GLOBAL cadence; when using the `formats { … }` block below it is read from here, otherwise from `tablist.refresh-ticks`. |
| `tablist.header` | `[...]` | The tablist header lines, joined top to bottom with newlines. This example uses the built-in \{tokens} above, so it shows real values with or without PlaceholderAPI; edit it to taste. (Leaving BOTH header and footer empty would leave the player's tab untouched rather than blanking it; the header and footer are sent together.) |
| `tablist.footer` | `[...]` | The tablist footer lines, joined top to bottom with newlines. |
| `tablist.refresh-ticks` | `20` | How often every viewer is re-rendered, in server ticks (20 ticks = one second). Must be positive. |
| `tablist.world-blacklist` | `[]` | World names where the tablist is suppressed entirely. |
{/* /generated */}

## Placeholders

{/* generated:placeholders */}
| Placeholder | Renders |
|---|---|
| `%uxmessentials_tablist_format%` | The tablist format the player's tab is drawn from, or a dash when they are drawn none. |
| `%uxmessentials_tablist_shown%` | Whether the player's tab is being drawn from a format at all (yes/no). |
{/* /generated */}

## Notes

- **Skins resolve on cracked servers too.** A skin named `player:<name>` is read from that player's live profile
  while they are online, and fetched from Mojang otherwise, so an offline-mode server still renders faces.
- **Header and footer share the HUD content grammar:** built-in `{tokens}`, PlaceholderAPI placeholders when
  PAPI is present, and animations paced by the refresh setting.
- **The layout arranges players into columns**, which is how a network builds a per-server or per-rank grid
  instead of one alphabetical list.

Related: [Scoreboard](scoreboard.md), [Nametags](nametags.md), [Vanish](vanish.md)

---
title: Scoreboard, Tablist & Nametags
order: 1410
description: 'The HUD is everything a player sees layered over the world without opening
  a menu: the sidebar scoreboard on the right, the header and footer of the tab list,
  and the nametags floating above heads. uxmEssentials splits these into three modules
  — scoreboard, tablist, and nametags — that share one design and one content grammar.'
---

The important detail: all three are **packet-based**, drawn entirely by the sibling
uxmLib toolkit. That means the HUD is fully **per-player** (two players can see
different sidebars in the same world), it never touches the vanilla scoreboard
objectives that other plugins fight over, and it needs no client mod. Each module is
purely config-driven, and `scoreboard` is the only one with a command.

---

## The Three Modules

| Module | Draws | Command |
|--------|-------|---------|
| `scoreboard` | The per-player sidebar (and drives the tablist header/footer) | `/scoreboard` (`sb`) |
| `tablist` | The tab-list header, footer, column layout and player skins | — (config only) |
| `nametags` | The name shown above each player's head | — (config only) |

They interrelate but stay independent: you can run just the scoreboard, just
nametags, or all three. Disabling one wires nothing for that surface.

<Callout type="info" title="All three ship switched off">

A HUD is the most visible thing a plugin can take over, and plenty of servers
already draw one with something else. So a fresh install leaves the sidebar, the
tab list and the nametags exactly as they were. Set `enabled = true` at the top of
`modules/scoreboard/config.conf`, `modules/tablist/config.conf` or
`modules/nametags/config.conf` (each one independently), then
`/uxmess reload <module>`. Every one of them ships with an example layout, so
turning it on gives you something to look at straight away.

</Callout>

---

## Sidebar Scoreboard

The sidebar is a per-player panel driven by the `scoreboard {}` block in config:

| Key | Controls |
|-----|----------|
| `title` | The header line (MiniMessage, can animate) |
| `lines[]` | The body lines, top to bottom |
| `hide-score-numbers` | Drop the vanilla red score numbers on the right |
| `world-blacklist` | Worlds where the sidebar is suppressed |

Players toggle their own sidebar on and off with `/scoreboard` (alias `sb`), and
open a personal settings panel with `/scoreboard gui`. The two nodes are
`uxmessentials.scoreboard.use` and `uxmessentials.scoreboard.gui`.

---

## Tablist

The tab list is shaped by the `tablist {}` block:

- `header[]` / `footer[]` — the multi-line banner above and below the player list.
- `layout` — how players are arranged into the tab-list columns.
- Player **skins** render next to each name, since the tablist is drawn from packets
  rather than vanilla scoreboard teams. A skin named as `player:<name>` is read from
  that player's live profile when they are online, and otherwise fetched from Mojang
  directly, so it resolves on offline-mode (cracked) servers too. See
  [Offline Mode](../getting-started/offline-mode.md).

The tablist registers **no commands** — it's entirely config- and content-driven.

---

## Nametags

The `nametag {}` block controls the name floating above each player:

- `lines[]` — one or more lines (a rank prefix, the name, a suffix/title).
- `appearance {}` — colours and formatting.
- `visibility {}` — including **name hiding**, so you can hide names past a distance
  or for specific players (useful for events, hardcore modes, or a "who's that?"
  arena). Prefixes and suffixes let you paint rank colours above heads without
  vanilla team quirks.

Nametags also register **no commands**.

---

## Content: Tokens, Placeholders & Animation

All three modules share the same content grammar:

- **Built-in `{tokens}`** — server stats like the online count and TPS are available
  as native tokens, so a basic HUD works with **no other plugin installed**.
- **PlaceholderAPI** — any `%…%` placeholder (including uxmEssentials' own
  `%uxmessentials_*%` family and anything from other plugins) resolves in HUD lines
  when PAPI is present.
- **Animations** — titles and lines support animated shapes (scrolling, cycling
  frames), paced by a shared `refresh-ticks` setting.
- **Condition-selected formats** — a module can carry several formats and pick one
  per player by condition, so staff, VIPs and default players each see a tailored
  HUD.

<Callout type="tip" title="Start from the shipped defaults">

uxmEssentials ships a working scoreboard, tablist and nametag layout out of the
box, wired to the built-in tokens. Enable the modules and you have a HUD
immediately; edit the content blocks to make it yours.

</Callout>

---

## Cross-Server Presence

On a network, uxmEssentials' [cross-server bus](../cross-server/overview.md) carries
a lightweight presence heartbeat between backends. With it enabled, HUD content can
reflect **network-wide** presence (who's online across the whole cluster) rather than
just the local server — handy for a tablist or sidebar that shows the full network
population. On a single server this simply doesn't apply.

---

## Commands & Reloading

Only the scoreboard is interactive:

| Command | Description | Permission |
|---------|-------------|------------|
| `/scoreboard` (`sb`) | Toggle whether you see the sidebar | `uxmessentials.scoreboard.use` |
| `/scoreboard gui` | Your personal scoreboard settings panel | `uxmessentials.scoreboard.gui` |

Each module reloads independently — `/uxmess reload scoreboard`,
`/uxmess reload tablist`, `/uxmess reload nametags` — so you can iterate on HUD
content without a restart.

---

## Next Steps

- [🧩 Per-Module Config](../config/per-module.md) — the `scoreboard`, `tablist` and `nametag` content blocks
- [🔌 PlaceholderAPI](../integrations/placeholderapi.md) — the placeholders your HUD can show
- [🌐 Velocity & Redis](../cross-server/overview.md) — network-wide presence
- [🔑 Permission Reference](../permissions/reference.md) — the `uxmessentials.scoreboard.*` nodes

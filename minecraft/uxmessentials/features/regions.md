---
title: Regions (WorldGuard)
order: 1400
description: 'The regions module is an in-game manager for WorldGuard regions: browse
  a world''s regions, carve out a new cuboid, cycle its protection flags, and edit
  its member and owner rosters — all from a menu, without touching /rg or reaching
  for a permission plugin. It''s built for the operator who runs WorldGuard for protection
  but doesn''t want to memorise the flag list or type out a UUID to add a co-owner.'
---

<Callout type="danger" title="WorldGuard is a soft-dependency — the module is inert without it">

`regions` does not bundle WorldGuard and does not manage protection on its own. It is
a **front-end** over an installed WorldGuard. With WorldGuard **absent**, the module
loads but does nothing: every `/regions` command answers with a single
*"WorldGuard not installed"* line and no menu opens. Install
[WorldGuard](https://enginehub.org/worldguard) for the module to have anything to
manage.

</Callout>

<Callout type="info" title="WorldEdit is needed only for selection-based create">

`/regions create` prefers your **WorldEdit** selection when one is present. If you
don't run WorldEdit — or haven't made a selection — mark the two corners with
`/regions pos1` and `/regions pos2` instead. Everything else works with WorldGuard
alone.

</Callout>

Because it is a front end and nothing else, `regions` ships **disabled**: on a server
without WorldGuard there is simply nothing for it to manage. Set `enabled = true` in
`modules/regions/config.conf` and run `/uxmess reload regions` once WorldGuard is
installed, and set it back to `false` to switch the module off again.

---

## Browsing a World's Regions

`/regions [world]` opens a paginated list of the regions in a world — your current world
when you omit the name, or a named world when you pass one. Each entry shows the region's
**id**, its **priority**, and its **member count** at a glance.

Click a region to open its **detail panel** — the hub for that one region. From there you
jump straight to its flag editor and its member/owner roster, so you rarely type the
`<id>` sub-commands by hand once the list is open.

The list is the natural starting point: open `/regions`, find the region you care about,
and drive everything else from its panel.

---

## Creating a Region

Defining a cuboid region is two steps — pick the volume, then name it.

**Pick the volume.** If you run WorldEdit, make a normal cuboid selection with the wand;
`/regions create` reads it directly. Without WorldEdit, mark the corners yourself:

| Command | What it does |
|---------|--------------|
| `/regions pos1` | Mark the **first** corner at your current block |
| `/regions pos2` | Mark the **second** corner at your current block |

**Name it.** `/regions create <id>` builds the cuboid from your WorldEdit selection if one
is present, otherwise from the two corners you marked, and registers it with WorldGuard
under `<id>`.

Two things are rejected up front, each with a clear message rather than a half-made
region:

- an **`<id>` that already exists** in that world, so you never clobber a region by reusing
  a name;
- a **create with no volume** — no WorldEdit selection *and* not both corners marked yet.

<Callout type="tip" title="Corners live until you create">

The two `pos1` / `pos2` marks are remembered per-player until you run `create`, so you
can walk to each corner, mark it, and name the region afterwards. A WorldEdit selection,
when you have one, always takes precedence over the marked corners.

</Callout>

---

## The Flag Editor

Open the flag editor from a region's detail panel, or jump to it directly with
`/regions flags <id>`. By default it lists **every flag WorldGuard has registered**, of
every type, and each one is edited in the way that suits it:

| Flag type | Examples | Clicking it |
|---|---|---|
| **State** | `pvp`, `build`, `entry`, `use` | Cycles `UNSET → ALLOW → DENY → UNSET` |
| **Boolean** | `invincible`, `pvp`-style toggles | Cycles `true → false → unset` |
| **Text** | `greeting`, `farewell` | Opens a text prompt |
| **Number** | `heal-amount`, `min-heal` | Opens a validated number prompt |
| **Fixed choice** | `game-mode`, `weather-lock`, a region group | Opens a choice picker |
| **Complex** | sets, locations | Shown **read-only** — edit those with WorldGuard's own commands |

Each click writes the new value straight to WorldGuard, so what you see in the menu is the
region's real flag state. `UNSET` clears the override and lets the flag fall back to
WorldGuard's own default for that region.

<Callout type="tip" title="Narrow the list if the full one is too much">

Listing every registered flag is a lot of items on a server with several flag-adding
plugins. Set `flags.editable` to a non-empty list and the editor shows **exactly**
those flags, in that order — see [Configuration](#configuration).

</Callout>

---

## Members & Owners

Open the roster from a region's detail panel, or with `/regions members <id>`. It lists the
region's **owners first**, then its members, so the people with the most authority read at
the top. Click any member or owner to **remove** them from the region.

Adding people is two commands, and both are **offline-safe** — a name you pass is resolved
to a UUID even when that player has never joined while you were looking, so you can seed a
region's roster before its owners log in:

| Command | What it does |
|---------|--------------|
| `/regions addmember <id> <player>` | Add a **member** to the region |
| `/regions addowner <id> <player>` | Add an **owner** to the region |

<Callout type="note" title="Legacy entries are read-only">

A WorldGuard region can hold **name-only** entries (from before UUIDs) and **group**
entries like `g:builders`. These render in the roster **greyed out and read-only** — the
module shows them so the picture is complete, but a click won't remove them, because they
aren't UUID-backed players. Manage those with WorldGuard directly.

</Callout>

---

## Priority

`/regions priority <id> <value>` sets a region's **priority**. When two regions overlap,
WorldGuard applies the higher-priority region's flags — so a small `no-pvp` arena inside a
larger `pvp` world just needs a higher number than its parent. This is a plain admin knob,
gated behind the module's `.admin` node.

---

## Configuration

Everything lives in `plugins/uxmEssentials/modules/regions/config.conf` (HOCON):

```hocon
# Regions: a GUI to manage WorldGuard regions. /regions [world] (permission uxmessentials.regions.list) opens a
# paginated list of the regions in a world; clicking a region opens its flag editor (and, from there, its
# members/owners editor).
#
# WorldGuard is a SOFT dependency. On a server without WorldGuard this module is fully inert: /regions replies
# "WorldGuard not installed" and opens nothing, and none of WorldGuard's classes are ever loaded.
#
# The module ships DISABLED: it is a GUI over WorldGuard's regions, so there is nothing to show without WorldGuard.
# Set enabled = true on a server that runs WorldGuard.
enabled = false

# The region list GUI.
list {
  # How many regions fill one page of the /regions list before it paginates. 1..45 (one six-row chest page, with the
  # bottom row reserved for the previous/next buttons).
  page-size = 45
}

# The flag editor GUI (opened by clicking a region in the list, or with /regions flags <id>).
flags {
  # An OPTIONAL allow-list of the WorldGuard flags the editor shows, in display order. Leave it EMPTY (the default) to
  # list every registered flag, of every type: state flags (pvp, build, ...) cycle allow/deny/unset, boolean flags
  # toggle true/false/unset, text flags (greeting, farewell, ...) open a text prompt, number flags open a validated
  # number prompt, and fixed-choice flags (game-mode, weather-lock, a region-group) open a choice picker. A
  # complex flag the editor cannot map (a set, a location) is shown read-only. Set a non-empty list to restrict the
  # editor to exactly those flags; names are the flag ids WorldGuard registers (lower kebab-case), e.g.:
  #   editable = [ "pvp", "build", "greeting", "game-mode" ]
  editable = []
}
```

After editing, apply it with `/uxmess reload regions`. `editable` ships **empty**, which
means "show every registered flag"; name a list to restrict the editor to exactly those
flags, using the ids WorldGuard registers (lower kebab-case).

<Callout type="warning" title="WorldGuard must be installed at runtime">

WorldGuard is declared as a **soft-depend** in `paper-plugin.yml`, so uxmEssentials
loads cleanly whether or not it's present — but the `regions` module can only *do*
something when WorldGuard is actually on the server. With WorldGuard missing, every
command short-circuits with *"WorldGuard not installed"*. WorldEdit is a further optional
soft-depend, needed only for selection-based `/regions create`.

</Callout>

<Callout type="info" title="HOCON, not YAML">

uxmEssentials config is HOCON (`.conf`). Keys are kebab-case, comments start with `#`,
and lists use square brackets. See [Configuration](../config/overview.md) for the full
picture.

</Callout>

---

## Permissions

Region management is staff work, so every node defaults to `op` — a fresh install hands the
region tools to operators and nobody else. Grant the specific node to the staff rank that
should hold it.

| Node | Default | What it grants |
|------|---------|----------------|
| `uxmessentials.regions.list` | `op` | `/regions [world]` — browse a world's regions and open a region's detail panel |
| `uxmessentials.regions.create` | `op` | `/regions create <id>`, `/regions pos1`, `/regions pos2` — define a new cuboid region |
| `uxmessentials.regions.flags` | `op` | The flag editor (from the panel or `/regions flags <id>`) — cycle a region's state flags |
| `uxmessentials.regions.members` | `op` | The member/owner roster (from the panel or `/regions members <id>`), plus `/regions addmember` / `addowner` |
| `uxmessentials.regions.admin` | `op` | `/regions priority <id> <value>` — set a region's priority |
| `uxmessentials.module.regions` | `op` | Reload / inspect the module (`/uxmess reload regions`) |

---

## Next Steps

- [🗺️ Regions Commands](../commands/regions.md) — the full `/regions` command reference
- [🔑 Permission Reference](../permissions/reference.md) — the `uxmessentials.regions.*` nodes
- [🧩 Per-Module Config](../config/per-module.md) — where `modules/regions/config.conf` lives and how it's loaded
- [🧩 The Module System](../modules/overview.md) — turn modules on and off

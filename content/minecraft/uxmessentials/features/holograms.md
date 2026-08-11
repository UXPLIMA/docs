---
title: Holograms
order: 1370
description: 'Holograms are floating displays you place in the world: a welcome sign at
  spawn, a live leaderboard by the arena, a rotating rules board, or a nameplate
  hovering over an NPC. uxmEssentials builds them from native Display entities (the same
  text/item/block display entities Minecraft ships), not from a packet trick, so they
  survive restarts, render for everyone the server chooses, and stay perfectly in sync
  without a client mod.'
---

Everything about a hologram is edited live from `/hologram` (or the management GUI)
and stored in the database: no config file to hand-write, no reload to see a
change.

---

## Hologram Types

A single hologram is a stack of **lines**, and each line can be one of five kinds:

| Type | Subcommand | What it shows |
|------|-----------|---------------|
| Text | `addline <text>` | A MiniMessage line: colours, gradients, PAPI placeholders |
| Item | `item <material>` | A floating, slowly spinning item |
| Block | `block <blockdata>` | A small floating block (any block state) |
| Head | `head <source>` | A player head from a name, URL or texture |
| Entity | `entity <type>` | A miniature living entity display |

Text lines are the workhorse. Because they resolve **MiniMessage**, you get the
full modern formatting set (`<gradient>`, `<rainbow>`, `<hover>`, `<click>`) with no
legacy `&` codes anywhere.

<Callout type="tip" title="Mixing line types">

A hologram can freely mix types: an item on top, a bold title, two body
lines, and a block underneath. Line indices are **1-based**, so
`/hologram setline 2 <text>` edits the second line.

</Callout>

---

## Lines & Pages

Lines are managed one at a time:

| Subcommand | Effect |
|------------|--------|
| `addline <text>` | Append a line to the bottom |
| `insertline <n> <text>` | Insert at position `n` |
| `setline <n> <text>` | Rewrite line `n` |
| `removeline <n>` | Delete line `n` |

For content that changes on a schedule: a tip of the day, an event countdown:
group lines into **pages** with `page add | remove | list`. The hologram cycles
through its pages automatically on its refresh interval.

---

## Display Look (Display-Meta)

Because holograms are real Display entities, you control every visual property the
engine exposes. Each is its own subcommand:

| Aspect | Subcommand | Notes |
|--------|-----------|-------|
| Billboard | `billboard <CENTER\|FIXED\|VERTICAL\|HORIZONTAL>` | Whether the display turns to face the viewer |
| Scale | `scale <factor>` | Grow or shrink the whole hologram |
| Alignment | `alignment <LEFT\|CENTER\|RIGHT>` | Text justification |
| Glow | `glow` | Outline glow (with a colour) |
| Opacity | `opacity` | Text transparency |
| Brightness | `brightness` | Block/sky light override, so it's readable at night |
| Background | `background` | The text panel's backing colour, or none |
| See-through | `seethrough` | Render through walls |
| Line width | `linewidth` | Wrap width for long text |
| Shadow | `shadow`, `shadowradius`, `shadowstrength` | Drop-shadow for the text |
| Translation / rotation | `translation`, `rotate <yaw> [pitch]` | Fine placement |
| View range | `viewrange`, `visibilitydistance` | How far away it renders |

<Callout type="info" title="Move it, don't rebuild it">

`movehere` teleports the hologram to you, `moveto <x y z>` to exact
coordinates, and `center` snaps it to the centre of its block. `copy` clones an
existing hologram with all its meta intact.

</Callout>

---

## Live Content: Refresh & Placeholders

Text lines are re-rendered on a fixed cadence set by `refreshinterval`. On each
refresh, any **PlaceholderAPI** tokens in the line (`%uxmessentials_economy_balance%`,
`%server_online%`, and so on) are re-evaluated, so a hologram can show a live player
count, TPS, or a scoreboard-style stat that ticks up as you watch.

The `holograms.conf` file adds a small `symbols {}` block for reusable glyphs and an
opt-in `damage-indicators { }` feature (off by default) that pops short-lived damage
numbers above entities.

### Leaderboards

`leaderboard <provider> [limit]` turns a hologram into a live top-N board: top
balances, most playtime, top voters: fed by a named provider and capped at `limit`
rows. It refreshes on the same interval, so the standings stay current on their own.

---

## Who Can See It: Visibility

Every hologram has a **visibility mode** that decides its default audience:

| Mode | Command | Who sees it |
|------|---------|-------------|
| ALL | `visibility ALL` | Everyone (the default) |
| PERMISSION | `visibility PERMISSION <perm>` | Only players holding `<perm>` |
| MANUAL | `visibility MANUAL` | Nobody, until you reveal it per-player |

On top of the mode you can override individual players:

| Subcommand | Effect |
|------------|--------|
| `show <player>` | Force-show to that player |
| `hide <player>` | Force-hide from that player |
| `blacklist <player>` | Permanently exclude a player |
| `unblacklist <player>` | Remove them from the blacklist |

This is how you build per-rank signage (a VIP-only board with `PERMISSION`) or a
reward that a player only sees once you `show` it to them (`MANUAL`).

---

## Click Action Chains

Holograms are interactive. Each one carries an ordered **action chain** that runs
when a player clicks it, keyed by trigger:

- `left`: runs on a left-click
- `right`: runs on a right-click
- `any`: runs on either

You build the chain step by step with the `action` subtree, and the steps run in
order top to bottom:

```
/hologram action add right command:spawn
/hologram action add_after right message:<green>Teleporting you home…
/hologram action move_up right 2
/hologram action list right
```

`action add | add_before | add_after | set | move_up | move_down | list | remove | clear`
give you full control over the sequence; `clickcommand` remains as a quick shortcut
when you only need a single command. This is the same idea used by the
[NPC action chains](npcs.md): a click becomes a small, ordered script.

<Callout type="tip" title="A hologram is a clickable button">

Because the chain can run commands, send messages, and play sounds, a
text hologram doubles as a menu-style button: "click to open the shop", "click
to claim your daily", "click for the rules".

</Callout>

---

## Linking to an NPC

A hologram makes an excellent multi-line **nameplate** above an
[NPC](npcs.md). Link the two with `linknpc <npc>` and the hologram tracks the NPC's
position, giving you a rich name tag (rank line, title line, status line) that a
plain NPC display name can't. `unlinknpc` detaches it again.

---

## Commands

Holograms live under one root command, `/hologram`, gated by
`uxmessentials.hologram.use` (the bare-root GUI branch also needs
`uxmessentials.holograms.gui`). The full subcommand set covers creation
(`create`, `delete`, `list`, `copy`), lines and pages, every display-meta property,
visibility, leaderboards, and the action chain. See the
[Commands Overview](../commands/overview.md) for how the command surface works, and
open the in-game panel with the bare `/hologram` if you'd rather point and click.

Like every module, holograms can be reloaded on their own with
`/uxmess reload holograms` (tier node `uxmessentials.module.holograms`).

---

## Next Steps

- [🤖 NPCs](npcs.md): attach a hologram nameplate to a fake-player NPC
- [🎛️ Custom Menu Engine](../menus/engine.md): turn a hologram click into a full GUI
- [🧩 Per-Module Config](../config/per-module.md): `holograms.conf` symbols and damage indicators
- [🔑 Permission Reference](../permissions/reference.md): the `uxmessentials.hologram.*` nodes

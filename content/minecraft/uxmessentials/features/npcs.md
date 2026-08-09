---
title: NPCs
order: 1380
description: 'NPCs are the interactive characters you scatter around a server — a
  shopkeeper at spawn, a warp guide, a tutorial helper, or a fake-player standing
  at the crossroads. uxmEssentials renders them with its own packet layer (part of
  the sibling uxmLib toolkit), so an NPC is not a real world entity: it exists only
  in each viewer''s client, costs the server almost nothing, and never wanders off,
  despawns, or gets killed. No client mod is required.'
---

Every NPC is edited live from `/npc` (or the management GUI) and persisted to the
database, so your cast survives restarts intact.

---

## Creating an NPC & Choosing a Type

`create <name> [type]` spawns an NPC where you're standing. The default is a
**player** NPC (a fake player, complete with a skin), but you can pick any entity
type instead:

| Type | Example | Behaves like |
|------|---------|--------------|
| Player (default) | `create guide` | A fake player with a skin and tab entry |
| Living entity | `create villager villager` | A villager, zombie, cow, … |
| Display / block / item | `create sign block_display` | A static display entity |

Change the type later with `type <entitytype>`, and reposition without recreating
using `movehere`, `moveto <x y z [yaw pitch]>`, `teleport`, or `center`. `copy`
duplicates an existing NPC, and `fix` re-sends an NPC that a client failed to render.

---

## Skins

Player NPCs take a skin from any of four sources:

| Spec | Meaning |
|------|---------|
| `skin name:<mcname>` | Fetch the current skin of a Minecraft account |
| `skin player:<mcname>` | Same, resolved from the player profile |
| `skin url:<url>` | Load a skin from an image URL |
| `skin texture:<base64>` | Apply a raw signed texture value |
| `skin none` | Clear the skin |

Use `skinslim <bool>` to switch between the classic and slim (Alex) arm models.

<Callout type="info" title="Skins, authlib and MineSkin">

Skin data is a signed texture bound to a game profile — that signature is what
makes a skin render for other players rather than showing up blank. uxmEssentials
handles the profile plumbing for you; for `url:`-based skins it can call the
**MineSkin** service to generate a signed texture, configured with an optional
`skin { mineskin-api-key }` in `npc.conf`. A supplied key raises your rate limit
but is not required for occasional use.

A `name:` lookup asks Mojang directly rather than going through the server's own
profile completion, so it resolves the same way on an offline-mode (cracked) server
as on an online-mode one. The whole server shares one cached lookup, and
`skins { mojang-lookup }` in `config.conf` turns it off for a server with no
outbound network.

</Callout>

---

## Appearance & Behavior

An NPC's presentation is a set of independent switches:

| Aspect | Subcommand | Notes |
|--------|-----------|-------|
| Display name | `displayname <text\|none\|reset>` | MiniMessage; `none` hides the tag, `reset` puts the NPC id back |
| Equipment | `equip <slot> <material>\|clear\|list` | Hand, off-hand, armour slots |
| Pose | `pose <pose>` | Standing, sitting, sleeping, … |
| Scale | `scale <0.0625-16>` | Shrink to a gnome or tower over players |
| Glow | `glow <bool> [color]` | Coloured outline |
| Look at player | `lookatplayer <bool>` | Head tracks nearby players |
| Collidable | `collidable <bool>` | Whether players can walk through it |
| Tab visibility | `showintab <bool>` | Show/hide the fake player in the tab list |
| Mirror | `mirror <bool>` | Mirror the viewer's own skin |
| States | `state <on_fire\|invisible\|silent> <bool>` | Toggle entity flags |
| Render distance | `viewdistance`, `turndistance` | How far it renders / tracks |

`data set | clear | list` stores arbitrary key/value data on an NPC for use by its
action chain.

---

## Click Action Chains & Cooldown

Like [holograms](holograms.md), each NPC carries an ordered **action chain** that
fires when a player interacts with it. Build it step by step:

```
/npc action add command:warp shop
/npc action add_after message:<yellow>Welcome to the shop!
/npc action list
```

The `action add | add_before | add_after | set | move_up | move_down | list | remove | clear`
subtree edits the sequence, and `command <value…>` sets a quick single command.
Pair it with `cooldown <duration>` to rate-limit interactions per player (there's
also a global `click-cooldown-millis` in `npc.conf`) so an NPC can't be spam-clicked.

<Callout type="tip" title="NPCs are buttons that walk">

An NPC's chain runs the same kind of steps a menu button does — run a command,
send a message, open a GUI. A shopkeeper NPC is just an action chain that
opens your shop menu.

</Callout>

---

## Multi-Line Nameplates

A single `displayname` is one line. For a proper rank/title/status plate, attach a
[hologram](holograms.md) as the NPC's nameplate: create the hologram, then run
`/hologram linknpc <npc>`. The hologram tracks the NPC and gives you unlimited
formatted lines above its head.

Clear the NPC's own name first with `/npc displayname <npc> none` (or the display-name
field in `/npc edit`, typing `-`), so the hologram is the only thing above its head. A
cleared name renders nothing at all: the NPC keeps its id for commands and the tab
entry, the client is simply told not to draw the nametag. Use `reset` instead of `none`
to put the id back above its head.

---

## Player-Owned NPCs

NPCs can be owned by ordinary players, not just staff — useful for a "hire an NPC"
perk or player shops. `npc.conf` controls the model:

- `default-limit` — how many NPCs a player may own (`-1` = unlimited).
- `blocked-commands` — commands an owned NPC's action chain may **not** run, so a
  player can't wire an NPC to a staff command.

`uxmessentials.npc.admin` gates the full `/npc` surface; a reserved
`uxmessentials.npc.use` node is set aside for a future per-NPC interaction gate.

---

## Commands

All NPC management is under one root, `/npc` (no aliases), gated by
`uxmessentials.npc.admin`; the bare-root GUI branch also needs
`uxmessentials.npc.gui`. See the [Commands Overview](../commands/overview.md) for how
Brigadier tab-completion and permission filtering work, and reload the module on its
own with `/uxmess reload npc` (tier `uxmessentials.module.npc`).

---

## Next Steps

- [🪧 Holograms](holograms.md) — floating displays and NPC nameplates
- [🎛️ Custom Menu Engine](../menus/engine.md) — the GUIs an NPC click opens
- [🧩 Per-Module Config](../config/per-module.md) — `npc.conf` limits, cooldowns, MineSkin key
- [🔑 Permission Reference](../permissions/reference.md) — the `uxmessentials.npc.*` nodes

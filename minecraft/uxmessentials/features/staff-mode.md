---
title: Staff Mode
order: 1310
description: Staff Mode is a dedicated moderation state, not a single toggle. When
  a staff member enters it with /staffmode, their survival inventory is swapped out
  for a hotbar of moderation gadgets, they go invisible, and — depending on config
  — they gain flight and night vision. Leaving staff mode restores their normal inventory
  and state exactly as it was.
---

The whole feature lives in the **`staff`** module. It is a **staff-mode-only** module:
it exists purely to give moderators a self-contained on-duty toolkit.

---

## How Staff Mode Works

`/staffmode` flips you between "playing" and "on duty":

```
Entering staff mode:
  1. Your real inventory is saved to the database
  2. It is replaced by the gadget hotbar
  3. Vanish / flight / night vision apply (per config)

Leaving staff mode:
  1. The gadget hotbar is cleared
  2. Your saved inventory is restored, untouched
```

Because your survival items are stored server-side while you're on duty, they survive
relogs and restarts and can't be lost by fumbling the gadget bar.

<Callout type="info" title="Loadouts are DB-backed">

The inventory saved when you enter staff mode is persisted in the database, so it
is safe across restarts and world rollbacks — you always get your real items back.

</Callout>

---

## The Gadget Hotbar

Entering staff mode gives you a hotbar of interaction gadgets. Each can be turned off
in config, so you ship only the tools your team should carry.

| Gadget | What it does |
|--------|--------------|
| **Vanish** | Toggle your invisibility on and off |
| **Examine** | Inspect a player's inventory / ender chest |
| **Freeze** | Pin a player in place for questioning |
| **Compass** | Teleport / navigate to players |
| **Follow** | Shadow a target player's movement |

<Callout type="note" title="Gadget set is config-driven">

The gadgets above are the ones exposed by the module's `gadgets { }` block
(`vanish`, `examine`, `freeze`, `compass`, `follow`). Set any to `false` to drop
it from the hotbar.

</Callout>

---

## Commands

| Command | What it does | Permission |
|---------|--------------|------------|
| `/staffmode [player]` | Enter / leave staff mode (loadout swap + gadgets + vanish) | `uxmessentials.staff.mode` |
| `/staffchat <message>` | Send to the staff-only chat channel (alias `sc`) | `uxmessentials.staff.chat` |
| `/stafflist` | Open the online-staff GUI (vanish-aware; click a head to teleport) | `uxmessentials.staff.list` |

**Staff chat** is a private channel for the team. Who *receives* it is set by the
`staff-chat.receive-node` config key, so you can point it at your existing staff
permission group. **`/stafflist`** shows who's on duty — it is vanish-aware (so hidden
staff still appear to other staff) and each head is clickable to teleport straight to
that person.

---

## Alerts & Flight

- **Flight** — `flight-on-enter` grants creative flight the moment you go on duty, so you can move freely while moderating.
- **Alerts** — the module surfaces staff-relevant notifications to on-duty moderators, keeping the team aware of events worth a look.

---

## How This Differs From `/vanish`

The [`vanish`](vanish.md) module owns `/vanish` (`uxmessentials.vanish.use`) — a
standalone, PremiumVanish-class invisibility toggle with its own buffs, fake join/quit
messages, and layered see/use levels. Staff mode is a whole on-duty *mode* on top of
that same vanish:

| | `/vanish` (vanish) | `/staffmode` (staff) |
|--|--------------------|----------------------|
| Invisibility | Yes | Yes (on enter) |
| Swaps your inventory | No | Yes — gadget loadout |
| Restores inventory on exit | n/a | Yes |
| Gadget hotbar | No | Yes |
| Flight / night vision | Yes — per `vanish` config (on by default) | Optional per staff config |
| Intended for | Quick hide | Full on-duty moderation |

Use `/vanish` to slip out of sight for a moment; use `/staffmode` to actually go on
duty. Staff mode drops you into this same vanish state on enter, so the two share one
invisibility flag. (Note `/stafflist` here is the staff-mode roster GUI; the presence
module's separate `/staff` command simply *lists* online staff to anyone allowed to see
it.)

---

## Permissions

| Node | Default | Grants |
|------|---------|--------|
| `uxmessentials.staff.mode` | `op` | Enter / leave staff mode |
| `uxmessentials.staff.chat` | `op` | Send in staff chat |
| `uxmessentials.staff.list` | `op` | Open the staff-list GUI |
| `uxmessentials.module.staff` | `op` | Reload / inspect the module |

---

## Key Settings

```hocon
# modules/staff/config.conf
enabled = true
vanish-on-enter = true
flight-on-enter = true
night-vision-on-enter = true

gadgets {
  vanish  = true
  examine = true
  freeze  = true
  compass = true
  follow  = true
}

staff-chat { receive-node = "..." }   # permission that receives staff chat
```

---

## Tips & Gotchas

<Callout type="warning" title="Staff mode ≠ vanish alone">

Leaving staff mode restores your saved inventory. Don't rely on `/vanish` (which
doesn't touch your inventory) when you mean to go fully on duty, and don't stack
the two carelessly — enter staff mode to moderate, exit it to play.

</Callout>

- The gadget hotbar replaces whatever you were holding; that's why the real inventory is saved first. Always exit staff mode (don't just log out mid-session) to be sure your items come back cleanly.
- Point `staff-chat.receive-node` at your existing staff group so new moderators get the channel automatically when they're added to the group.
- Turn off gadgets your team shouldn't have (e.g. leave `follow` off if you don't want silent shadowing) — the hotbar only shows enabled gadgets.

---

## Next Steps

- [📟 Staff & Presence Commands](../commands/staff-presence.md) - `/staffmode`, `/staffchat`, `/vanish` and more
- [🫥 Vanish](vanish.md) - The standalone invisibility module staff mode drops you into
- [🔐 Permission Reference](../permissions/reference.md) - Staff-mode nodes
- [⚙️ Per-Module Config](../config/per-module.md) - Loadout, gadgets, and staff chat
- [🛡️ Moderation](moderation.md) - The punishment toolkit staff use on duty

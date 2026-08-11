---
title: Inventory Rollback
order: 1360
description: 'The invrollback module gives staff the one thing a survival server almost
  always ends up wanting a separate plugin for: a way to hand a player back an inventory
  they lost (to a lag death, a bad restart, a bug) without a manual /give guessing game.
  It snapshots a player''s inventory at the moments things go wrong (a death, a logout),
  keeps a bounded history of those snapshots in the database, and lets staff browse and
  restore them from a GUI with /invrestore <player>. It''s the AxInventoryRestore
  feature set, built into the same module system as everything else and configured from
  one HOCON file, modules/invrollback/config.conf.'
---

<Callout type="info" title="Ships switched off, and starts capturing the moment you turn it on">

Every death and every logout writes a snapshot to the database, which is a real
ongoing cost and only worth paying on a server whose staff actually restore
inventories. So the module ships **disabled**. Set `enabled = true` in
`modules/invrollback/config.conf` and run `/uxmess reload invrollback`, and capture
begins immediately with sensible defaults, so a snapshot exists the first time you
need one. Narrow *when* it captures with the `capture` block below.

One thing to know before you decide: snapshots only exist from the moment you turn
it on. It cannot recover an inventory that was lost while the module was off.

</Callout>

Every snapshot is **DB-backed and never stored in PDC**: the same hard invariant the
economy and vaults ledgers hold. The authority is the stored row, not the live
inventory, so a snapshot **survives a world rollback**: roll the world back to last
night and the snapshot you took this morning is still there to restore from. The
snapshots live in the `inv_snapshots` table, which ships as part of the persistence
baseline, so the module runs no migrations of its own.

---

## What a snapshot captures

A snapshot freezes the player's **full inventory** at the moment it's taken:

- the **main inventory**: hotbar and storage,
- the **armour** they're wearing,
- the **offhand** slot,
- and, when `include-enderchest` is on, the **ender chest** alongside it,
- plus **where the player was standing**, so staff can go back to the scene.

Each item is serialised whole (components, enchantments, custom names and all) so a
restored item is byte-for-byte the one that was lost, not a re-rolled stand-in. Empty
slots store nothing, so a near-empty inventory stays compact in the table.

Every capture is tagged with a **cause** (`DEATH`, `LOGOUT`, or `RESTORE`) which the
restore GUI shows so staff can tell a death snapshot apart from a routine logout one.

---

## When a snapshot is taken

Two triggers are under your control, and a third happens automatically as a safety net.

### On death

```hocon
capture { on-death = true }
```

With it on, the player's inventory is read **the instant they die, before the items
drop**, and frozen into a `DEATH` snapshot. The capture is a pure observer; it runs at
the lowest priority and never alters the death, the drops, or the death message; it only
reads. This is the trigger you'll restore from most often.

### On logout

```hocon
capture { on-logout = true }
```

With it on, the player's inventory is frozen into a `LOGOUT` snapshot when they leave
the server: a cheap "last known good" state that covers a crash, a kick, or a dupe that
only shows up after they've logged off.

### The pre-restore safety snapshot

There's a third cause you never configure: **`RESTORE`**. Immediately before a restore
overwrites a player's live inventory, the module snapshots their **current** inventory
first, tagged `RESTORE`. So a restore is itself undoable: pick the wrong snapshot and
you can restore the `RESTORE` copy to put things back exactly as they were. This safety
copy is only taken when the chosen snapshot actually resolves; a dead click on an
already-pruned snapshot writes nothing.

<Callout type="note" title="Captures never block the server">

The live inventory is read on the region tick thread (where reading it is legal),
serialised there, and the database write is then handed to a background thread. A
capture never blocks a region thread and is Folia-safe from line one.

</Callout>

---

## Restoring an inventory: `/invrestore <player>`

Staff holding **`uxmessentials.invrollback.restore`** run **`/invrestore <player>`** to
open the restore GUI for a target. The whole flow is three steps:

1. **The snapshot list.** A paginated window lists every snapshot the target holds,
   **newest first**, one icon each. The icon's material tracks the cause: a
   **skeleton skull** for a death, an **ender pearl** for a logout, a **clock** for a
   pre-restore safety copy, and its label names the cause and the capture time, with a
   click-to-preview hint. If the target has no snapshots, you get a "no snapshots" line
   instead of an empty window.
2. **The preview.** Click a snapshot to open a **read-only** preview: a chest window
   whose top five rows mirror the snapshot's inventory exactly as it was, with a lime
   **restore button** in the bottom row. Every other click is cancelled, so you can look
   without touching; nothing you do in the preview changes anything until you press the
   button.
3. **The restore.** Press the button and the target's live inventory (main, armour,
   offhand, and the ender chest when the snapshot carried one) is overwritten with the
   snapshot's contents, after the pre-restore safety snapshot is taken first. You get a
   confirmation line naming the player and the cause you restored.

<Callout type="warning" title="The target must be online">

A restore is applied to a **live** inventory: it's never written to the player's
saved file on disk. So the target has to be **online** for the restore to land; aim
`/invrestore` at an offline or unknown name and you get a "not online" line and
nothing changes. Their snapshots persist regardless, so the restore succeeds the
moment they rejoin: open the GUI again once they're back.

</Callout>

The target lookup, the snapshot listing, and both database touches during a restore all
run off the tick thread, so `/invrestore` never blocks the server.

---

## Two more things you can do with a snapshot

Both act on **one snapshot chosen by its list index** (`1` is the newest), and both work
on an **offline** target, because they read stored data and act on *you*, not on the
target's live session.

### Export to shulker boxes

```
/invrestore export <player> <index>
```

Packs the snapshot's items into named shulker boxes and hands them to you, so a lost
inventory can be inspected or given back by hand without touching the target's live
inventory. Every store is exported (hotbar, storage, armour, offhand and the ender
chest), packed 27 stacks to a box, so a full inventory produces as many boxes as it
needs. An item that is *itself* a shulker box is handed over as-is rather than nested,
because vanilla forbids a box inside a box. Boxes that will not fit are dropped at your
feet.

Gated on `uxmessentials.invrollback.export`.

### Teleport to the scene

```
/invrestore tp <player> <index>
```

Teleports you to where that snapshot was captured, which is the fast way to find a death
site or check what a logout looked like. Gated on
`uxmessentials.invrollback.teleport`.

<Callout type="note" title="Older snapshots have no location">

Location capture arrived after the module shipped, so a snapshot taken before it,
or one whose world is no longer loaded, answers with a "no location" line instead of
teleporting.

</Callout>

---

## Retention

Left alone, a snapshot-per-death-and-logout would grow the table without bound, so
retention keeps it in check two ways, and either can be switched off independently.

```hocon
retention {
  max-per-player = 10
  max-age-days   = 30
}
```

| Key | Default | What it does |
|-----|---------|--------------|
| `max-per-player` | `10` | The most snapshots kept per player. After **every** capture the player's snapshots past this count are pruned **oldest-first**, so a player's history is trimmed the moment it overflows; you always keep their most recent `10`. `0` disables the count cap. |
| `max-age-days` | `30` | The oldest a snapshot may get. A scheduled sweep removes any snapshot older than this. `0` disables the age cap. |

The **count cap** is enforced immediately on every capture, so between sweeps only the
age half can accrue. On top of that, an **hourly sweep** runs the retention policy in the
background: it re-applies the count cap across every player (catching anyone left over
the cap after you *lower* `max-per-player` and reload) and removes anything past
`max-age-days`. The first sweep runs a few minutes after the server starts so it never
collides with startup, then every hour after. Set **both** limits to `0` and the sweep
never schedules at all.

---

## The `config.conf`

The complete `modules/invrollback/config.conf`, with the shipped defaults:

```hocon
enabled = true

# When to take a snapshot.
capture {
  on-death = true    # snapshot the inventory when a player dies (read before the items drop)
  on-logout = true   # snapshot the inventory when a player leaves the server
}

# Whether a snapshot also stores the player's ender chest alongside the main inventory, armor and offhand.
include-enderchest = true

# How long snapshots are kept, so the table stays bounded. After each capture the player's snapshots past
# max-per-player are pruned oldest-first; a scheduled hourly sweep removes any snapshot older than max-age-days
# (and trims any player left over the count cap after a reload). Set either limit to 0 to disable it.
retention {
  max-per-player = 10
  max-age-days = 30
}
```

Config is loaded once on enable and swapped atomically on `/uxmess reload invrollback`,
so a capture handled mid-reload always sees one coherent snapshot. Delete a line and it
falls back to the shipped default above.

---

## Command

| Command | What it does | Permission |
|---------|--------------|------------|
| `/invrestore <player>` | Open the inventory-snapshot restore GUI for a player | `uxmessentials.invrollback.restore` |
| `/invrestore export <player> <index>` | Pack that snapshot into shulker boxes and give them to you | `uxmessentials.invrollback.export` |
| `/invrestore tp <player> <index>` | Teleport to where that snapshot was captured | `uxmessentials.invrollback.teleport` |

The `<player>` argument suggests online players, but accepts any name the server has seen
before. `/invrestore` itself is visible to anyone holding **any** of the three nodes, and
each verb then gates on its own.

Only the **restore** needs the target online, since it writes their live inventory.
Listing, exporting and teleporting all work while they are offline.

---

## Permissions

Three action nodes, plus the per-module reload gate. All default to `op`, so a fresh
install hands the restore tools to operators and nobody else: grant the specific node to
the staff rank that should have it. Splitting them lets you hand out the read-only verbs
(export, teleport) without the one that overwrites a live inventory.

| Node | Default | What it grants |
|------|---------|----------------|
| `uxmessentials.invrollback.restore` | `op` | `/invrestore <player>`: open the snapshot GUI and restore from it |
| `uxmessentials.invrollback.export` | `op` | `/invrestore export <player> <index>`: pack a snapshot into shulker boxes |
| `uxmessentials.invrollback.teleport` | `op` | `/invrestore tp <player> <index>`: teleport to the capture location |
| `uxmessentials.module.invrollback` | `op` | Reload / inspect the module (`/uxmess reload invrollback`) |

---

## Next Steps

- [🔑 Permission Reference](../permissions/reference.md): the full node list
- [🧩 Per-Module Config](../config/per-module.md): where `modules/invrollback/config.conf` lives and how it's loaded
- [🗄️ Vaults](vaults.md): DB-backed player-owned item storage, the same "survives a rollback" guarantee
- [🛡️ Moderation](moderation.md): bans, mutes and warnings for the staff who hold the restore node

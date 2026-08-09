---
title: Villagers
order: 1350
description: 'The villagers module hands you the villager-management knobs a survival
  or economy server usually reaches for a small pile of single-purpose plugins to
  get: trades that never lock out, a restock timer that replaces the vanilla work-station
  cycle, a staff trade editor, click-to-trade access, a villager saver, villager pickup,
  follow, and leashing. Each is an independent switch in one HOCON file, modules/villagers/config.conf.'
---

<Callout type="info" title="The module ships off; inside it, the common tools are pre-armed">

Every feature here rewrites how villager trading works, and a server that wants
vanilla villagers should get vanilla villagers, so the module ships **disabled**.
Set `enabled = true` at the top of the file and run `/uxmess reload villagers` to
turn it on. Once on, the six tools most servers want are already armed: infinite
trading, the restock timer, the staff trade manager, click-to-trade, protection and
follow. The four that visibly change gameplay stay **off** until you say otherwise:
instant restock, the blanket disable-trades switch, the villager bucket, and leash.
A listener whose switch is off is an inert early return, so you pay nothing for what
you leave off.

</Callout>

The context **persists nothing relational**. The per-villager last-restock stamp, the
disable flag, the follow-owner mark, and a manager-edited villager's custom trade set all
live in the villager's own **PDC** on the entity — so the module owns no database table
and runs no migrations, and a manager's edits survive a chunk reload or restart because
they ride on the villager itself.

---

## The ten features at a glance

| Feature | Config block | Ships | What it does | Gated by |
|---------|--------------|-------|--------------|----------|
| **Infinite trading** | `infinite-trading` | **on** | A villager's trades never lock out from use | *(no permission)* |
| **Restock timer** | `restock` | **on** | A scheduled sweep restocks loaded villagers on an interval | *(no permission)* |
| **Instant restock** | `instant-restock` | off | The trade a player just used restocks at once | *(no permission)* |
| **Disable trades** | `disable-trades` | off | Every villager refuses to open its trade window | *(no permission)* |
| **Trade manager** | `trade-manager` | **on** | `/villager manager` — a staff GUI to edit a villager's trades | `uxmessentials.villagers.manager` |
| **Click-to-trade** | `click-to-trade` | **on** | Right-click a villager to open its trade window directly | `uxmessentials.villagers.trade` |
| **Protection (saver)** | `protect` | **on** | A villager that can't die and won't despawn | `uxmessentials.villagers.protect` |
| **Villager-in-a-bucket** | `bucket` | off | Sneak-pick a villager into an item and place it back | `uxmessentials.villagers.bucket` |
| **Follow** | `follow` | **on** | `/villager follow` — the villager pathfinds after you | `uxmessentials.villagers.follow` |
| **Leash** | `leash` | off | Right-click a villager with a lead to leash it | `uxmessentials.villagers.leash` |

The first four are entity-mechanics only — they act on their own with no command and no
permission, gated purely by their config switch. The last six carry a command or a
right-click gesture behind one of the `uxmessentials.villagers.*` nodes.

---

## The `/villager` command

`/villager` registers whenever the module is on, gated on
**`uxmessentials.villagers.use`**. Each subcommand then carries its own node *and* needs
its feature switched on, so the verbs you see are the ones you turned on.

| Command | What it does | Permission |
|---------|--------------|------------|
| `/villager manager` | Open the trade manager for the villager you're looking at | `uxmessentials.villagers.manager` |
| `/villager protect` | Toggle whether that villager is protected from death and despawn | `uxmessentials.villagers.protect` |
| `/villager follow` | Toggle whether that villager follows you | `uxmessentials.villagers.follow` |

Every subcommand acts on **the villager you are looking at** — or, if you aren't aiming
directly at one, the **nearest villager within five blocks**. A sender who isn't a player,
or who has no villager in reach, gets a feedback line instead of an action.

---

## Trade availability

Four switches change how a villager's trades behave. None takes a permission; each is on
or off by its config block alone.

### Infinite trading

```hocon
infinite-trading { enabled = true }
```

With it on, a villager's trades **never lock out**. On every completed trade the plugin
resets the villager's recipe uses and stops the trade counting toward its limit, so no
trade ever greys out. It ships **on** as a common convenience; set `enabled = false` to
restore vanilla trade limits.

### Restock timer

```hocon
restock {
  enabled          = true
  interval-seconds = 600
}
```

Instead of relying on the vanilla work-station cycle, a scheduled sweep restocks a
villager's trades once its last restock is older than `interval-seconds`. Only **loaded**
villagers are swept, and the last-restock instant is stamped in the villager's PDC and
compared against the interval.

| Key | Default | What it does |
|-----|---------|--------------|
| `enabled` | `true` | Master switch for the restock sweep. Off leaves restocking to the vanilla work-station cycle. |
| `interval-seconds` | `600` | How long a villager's trades stay fresh between restocks, in seconds. Clamped to at least `1`. |

### Instant restock

```hocon
instant-restock { enabled = false }
```

The trade a player just used **restocks immediately**, with no cooldown. It composes with
the restock timer — the used trade is available again at once regardless of the timer, so
it effectively wins for the recipe a player just took.

### Disable trades

```hocon
disable-trades { enabled = false }
```

With it on, **every** villager refuses to open its trade window: the right-click is
cancelled and the player is shown the `villagers.trades-disabled` line. The same listener
also honours a **per-villager** disable flag the trade manager sets, so an individual
villager can be turned off even while this global switch is off — which is why the listener
always registers when the module is on (with the global switch off and no flag set, it
cancels nothing).

---

## Trade manager

```hocon
trade-manager { enabled = true }
```

With it on, staff holding **`uxmessentials.villagers.manager`** run **`/villager manager`**
to open the villager they're looking at and edit its trades. Edits apply to the live
merchant and are PDC-serialised onto the villager, so they survive a chunk reload or
restart (a load-time reapply listener restores them when the villager comes back). It
ships **on** as a common staff tool; set `enabled = false` to drop `/villager manager`.

The window is a **six-row chest**:

- The **top five rows** are recipe rows. Each has two editable **buy** slots, a decorative
  arrow, an editable **sell** slot, and a **remove** button. Editing a trade means dragging
  its buy/sell stacks into the row — *the stack amounts are the trade amounts*. Filling an
  empty row **adds** a trade; clearing a row (the remove button, or emptying it) **drops**
  that trade.
- The **sixth row** is a control bar: a **disable toggle** (flips this villager's
  per-villager trade-disable flag) and a **help** item.

A librarian's deeper trade list is safe: any trade beyond the five visible rows is carried
through untouched, so the editor never truncates it. The window is a definition surface —
its stacks are trade templates, not deposited items — and the villager's recipes are
replaced wholesale on close, never appended, so nothing dupes.

---

## Click-to-trade

```hocon
click-to-trade { enabled = true }
```

With it on, a player holding **`uxmessentials.villagers.trade`** who right-clicks a villager
opens its trade window **directly**, even where the vanilla gate would refuse it — a
professionless villager, or one already busy with another trader. A **disabled** villager
(globally, or by its per-villager flag) still never opens. It ships **on**; set
`enabled = false` to leave villager access to the vanilla gate.

---

## Protection (villager saver)

```hocon
protect {
  enabled        = true
  all            = false
  from-zombies   = true
  from-lightning = true
  from-damage    = true
  no-despawn     = true
}
```

With it on, a **protected** villager never dies to the deaths a settlement loses villagers
to, and is kept loaded so it never despawns. Mark an individual villager with
**`/villager protect`** (permission **`uxmessentials.villagers.protect`**), or set
`all = true` to shield **every** villager.

| Key | Default | What it does |
|-----|---------|--------------|
| `enabled` | `true` | Master switch for the saver. On out of the box, though only villagers marked with `/villager protect` are shielded until `all = true`. |
| `all` | `false` | Shield every villager, not only those marked with `/villager protect`. |
| `from-zombies` | `true` | Cancel a zombie infecting the villager into a zombie villager. |
| `from-lightning` | `true` | Cancel a lightning strike's damage and the villager-to-witch transform it triggers. |
| `from-damage` | `true` | Cancel other lethal damage: suffocation, fire, drowning, a mob's or player's blow. |
| `no-despawn` | `true` | Keep the villager loaded (mark it persistent) so it never despawns when no player is nearby. |

<Callout type="note" title="The four gates ship on">

The per-threat gates default to `true`, so a protected villager is shielded from
**everything** until you narrow it — turn a gate off to let that one end through (for
example `from-zombies = false` to protect a villager yet still allow zombie
conversions).

</Callout>

`/villager protect` toggles the mark on the villager you look at and confirms with
`villagers.protect.enabled` / `villagers.protect.disabled`.

---

## Villager-in-a-bucket

```hocon
bucket { enabled = false }
```

With it on, a player holding **`uxmessentials.villagers.bucket`** **sneak-right-clicks** a
villager to pick it up into a captured-villager item that stores the villager's
**profession, type, level, and trades**. Right-clicking a **block** with that item places
the villager back, spawning it with the same trades it had when captured (not a re-rolled
vanilla set). A gameplay change, so it ships off.

Both directions cancel the vanilla interaction they'd otherwise trigger — the trade window
on pickup, the spawn-egg spawn on placement — and the captured item is consumed on
placement (except in creative). The item carries the `villagers.bucket.name` display name.

---

## Follow

```hocon
follow {
  enabled = true
  speed   = 1.0
  range   = 16
}
```

With it on, a player holding **`uxmessentials.villagers.follow`** runs **`/villager follow`**
to make the villager they're looking at pathfind after them. The villager keeps up while it
stays within `range` blocks of its owner **in the same world**, stops beyond that, and
resumes when the owner returns. A second `/villager follow` un-follows it. The owner is
stamped in the villager's PDC, so the pairing is durable. It ships **on** as a common
tool; set `enabled = false` to drop `/villager follow`.

| Key | Default | What it does |
|-----|---------|--------------|
| `enabled` | `true` | Master switch for follow. |
| `speed` | `1.0` | The villager's pathfinder speed multiplier while following (`1.0` = normal). A non-positive value falls back to `1.0`. |
| `range` | `16` | How far, in blocks, the owner may stray before the villager stops following. Clamped to at least `1`. |

The follow toggle confirms with `villagers.follow.started` / `villagers.follow.stopped`.

---

## Leash

```hocon
leash { enabled = false }
```

With it on, a player holding **`uxmessentials.villagers.leash`** right-clicks a villager
with a **lead** in hand to leash it — something vanilla never allows. The lead is consumed,
and the trade window that click would otherwise open is suppressed. Leashing is silent,
like vanilla leashing — there's no confirmation line. A gameplay change, so it ships off.

---

## The `config.conf`

The complete `modules/villagers/config.conf`, with the shipped defaults:

```hocon
# Villagers — villager trade management. The trade-availability mechanics (never-locking trades, a restock timer that
# replaces the vanilla work-station cycle, immediate restock of a used trade, and a switch that refuses trading
# altogether) plus the staff trade manager and click-to-trade access.
#
# The module ships DISABLED: what a villager trades and how often it restocks is an economy decision, so a fresh
# install leaves it to you. Set enabled = true and the common villager tools come on with it (infinite trading, the
# restock timer, the staff trade manager, click-to-trade, protection, and follow); the niche or disruptive ones stay
# OFF (instant restock, the blanket disable-trades switch, the villager bucket, and leash). Flip any single feature
# below.
enabled = false

# Infinite trading: a villager's trades never lock out from use. On every trade the plugin resets the villager's
# recipe uses and stops the trade from counting toward its limit, so no trade ever greys out. Ships on as a common
# convenience; set enabled = false to restore vanilla trade limits.
infinite-trading {
  enabled = true
}

# Restock timer: instead of relying on the vanilla work-station cycle, a scheduled sweep restocks a villager's trades
# once its last restock is older than interval-seconds. Only loaded villagers are swept. interval-seconds is clamped
# to at least one. Ships on; set enabled = false to leave restocking to the vanilla work-station cycle.
restock {
  enabled = true
  interval-seconds = 600
}

# Instant restock: the trade a player just used restocks immediately, with no cooldown. Composes with the restock
# timer — the used trade is available again at once regardless of the timer.
instant-restock {
  enabled = false
}

# Disable trades: with enabled = true, every villager refuses to open its trade GUI (the right-click is cancelled and
# the player is told). The trade manager can also disable an individual villager, which is honoured even while this
# global switch is off.
disable-trades {
  enabled = false
}

# Trade manager: with enabled = true, staff holding uxmessentials.villagers.manager run /villager manager to open the
# villager they are looking at (or the nearest within reach) and edit its trades: change a recipe's buy/sell items and
# amounts, add a recipe, remove one, or toggle that villager's disable flag. Edits apply to the live villager and are
# saved onto the villager so they survive a chunk reload / restart. Ships on as a common staff tool; set enabled =
# false to drop /villager manager.
trade-manager {
  enabled = true
}

# Click-to-trade: with enabled = true, a player holding uxmessentials.villagers.trade who right-clicks a villager opens
# its trade window directly, even where the vanilla gate would refuse it (a professionless villager, one already busy
# with another trader). A disabled villager (globally or per-villager) still never opens. Ships on as a common
# convenience; set enabled = false to leave villager access to the vanilla gate.
click-to-trade {
  enabled = true
}

# Protection ("villager saver"): with enabled = true, a protected villager never dies to the deaths a settlement loses
# villagers to and is kept loaded so it never despawns. Mark an individual villager with /villager protect (permission
# uxmessentials.villagers.protect), or set all = true to shield every villager. The four gates below choose which
# threats are cancelled; they ship on, so turning the feature on shields everything until you narrow it. The feature
# itself ships on out of the box, though only villagers marked with /villager protect are shielded until all = true.
protect {
  enabled = true
  # Shield every villager, not only those marked with /villager protect.
  all = false
  # Cancel a zombie infecting the villager into a zombie villager.
  from-zombies = true
  # Cancel a lightning strike's damage and the villager-to-witch transform it triggers.
  from-lightning = true
  # Cancel other lethal damage: suffocation, fire, drowning, a mob's or player's blow.
  from-damage = true
  # Keep the villager loaded (mark it persistent) so it never despawns when no player is nearby.
  no-despawn = true
}

# Villager-in-a-bucket: with enabled = true, a player holding uxmessentials.villagers.bucket sneak-right-clicks a
# villager to pick it up into a captured-villager item that stores the villager's profession, type, level, and trades.
# Right-clicking a block with that item places the villager back, restoring the same trades. A gameplay change, so it
# ships off.
bucket {
  enabled = false
}

# Follow: with enabled = true, a player holding uxmessentials.villagers.follow runs /villager follow to make the
# villager they are looking at (or the nearest within reach) pathfind after them. The villager keeps up while it stays
# within range blocks of its owner in the same world, stops beyond that, and resumes when the owner returns; a second
# /villager follow un-follows it. speed is the villager's pathfinder speed multiplier (1.0 = normal); range is clamped
# to at least one block. Ships on as a common tool; set enabled = false to drop /villager follow.
follow {
  enabled = true
  speed = 1.0
  range = 16
}

# Leash: with enabled = true, a player holding uxmessentials.villagers.leash right-clicks a villager with a lead in
# hand to leash it — something vanilla never allows. The lead is consumed and the trade window that click would open
# is suppressed. A gameplay change, so it ships off.
leash {
  enabled = false
}
```

Config is loaded once on enable and swapped atomically on `/uxmess reload villagers`, so a
trade handled mid-reload always sees one coherent snapshot. Delete a line and it falls back
to the shipped default above.

---

## Permissions

Every node defaults to `op`, so a fresh install hands the villager **tools** to operators
and nobody else — grant the specific node to the staff or donor rank that should have it.

The four trade-availability switches take no permission at all; they're config-only. That
is the practical difference on a fresh install: infinite trading and the restock timer
change gameplay for **everyone** right away, while the trade manager, click-to-trade,
protection and follow are enabled but reachable only by operators until you grant their
nodes.

| Node | Default | What it grants |
|------|---------|----------------|
| `uxmessentials.villagers.use` | `op` | `/villager` — the root command's base node; each subcommand gates further below |
| `uxmessentials.villagers.manager` | `op` | `/villager manager` — open and edit the trades of the villager you're looking at |
| `uxmessentials.villagers.trade` | `op` | Open a villager's trade window directly on right-click, when click-to-trade is enabled |
| `uxmessentials.villagers.protect` | `op` | `/villager protect` — toggle whether the villager you're looking at is protected from death and despawn |
| `uxmessentials.villagers.bucket` | `op` | Sneak-right-click a villager to pick it up into a captured-villager item, and place it back later |
| `uxmessentials.villagers.follow` | `op` | `/villager follow` — toggle whether the villager you're looking at pathfinds after you |
| `uxmessentials.villagers.leash` | `op` | Right-click a villager with a lead to leash it, when leashing is enabled |
| `uxmessentials.module.villagers` | `op` | Reload / inspect the module (`/uxmess reload villagers`) |

---

## Next Steps

- [🔑 Permission Reference](../permissions/reference.md) — the full `uxmessentials.villagers.*` node list
- [🧩 Per-Module Config](../config/per-module.md) — where `modules/villagers/config.conf` lives and how it's loaded
- [💬 Messages & Languages](../config/messages.md) — the `villagers.*` lines (the disabled-trade refusal, the protect/follow confirmations, the manager labels, the bucket item name)
- [🛒 Trade](trade.md) — player-to-player trading (a separate module from villager trade management)

---
title: Vanish
order: 1320
description: Vanish makes a player invisible to everyone who is not allowed to see
  hidden players, and it does the job properly — the vanished player drops off the
  tab list, their join and quit lines can be faked so nobody notices they merely went
  invisible, and they move through the world untouched by damage, hunger, and mobs.
  It is a PremiumVanish-class feature with layered see/use levels, so a senior admin
  can hide from a junior moderator while still watching over them.
---

Vanish is its own **`vanish`** module. It is the single authority over vanish state:
messaging (`/msg` resolution), the nametag renderer, and staff mode all read this one
vanish flag, so there is never a second copy to keep in sync. It replaces the plain
vanish that used to live inside the `presence` and `staff` modules.

The module ships **enabled**. Turn it off with `enabled = false` in
`modules/vanish/config.conf`, at which point the messaging and nametag vanish gates
degrade to "no one is hidden" and staff-mode vanish becomes a no-op.

---

## Commands

Everything lives under one command, `/vanish` (alias `/v`).

| Command | What it does | Permission |
|---------|--------------|------------|
| `/vanish` | Toggle your own vanish on or off | `uxmessentials.vanish.use` |
| `/vanish on` / `/vanish off` | Set your vanish absolutely (no guessing which way a toggle went) | `uxmessentials.vanish.use` |
| `/vanish <player>` | Toggle another player's vanish (console-usable) | `uxmessentials.vanish.others` |
| `/vanish list` | List the hidden players you are permitted to see | `uxmessentials.vanish.list` |
| `/vanish pickup [on\|off]` | Flip whether you pick up items while vanished | `uxmessentials.vanish.use` |
| `/vanish -s` (also `on -s` / `off -s`) | Vanish or reappear **silently** — no fake join/quit broadcast | `uxmessentials.vanish.silent` |

A few notes:

- **`/vanish list` is scoped to your see level.** It never leaks a player hidden
  *above* your see level — you only ever learn about the vanished players you could
  already see. A vanished caller appears in their own list (that reads as "you are
  vanished").
- **`/vanish pickup` is remembered per player** and defaults to the `pickup-items`
  config value. Bare `/vanish pickup` toggles; `on` / `off` set it absolutely.
- **The `-s` flag** only applies to vanishing and reappearing (`/vanish`, `on`, `off`).
  It suppresses the fake connection broadcast for a truly silent transition.

---

## Layered See / Use Levels

Vanish uses the PremiumVanish see/use model. Every vanished player has a **use level**
(how hidden they are), and every viewer has a **see level** (how far up they can see).
The rule is one line:

> A viewer sees a vanished player **if and only if the viewer's see level is at least
> the target's use level.** A higher-or-equal see level overrides a lower-or-equal use
> level; a higher use level overrides a lower see level.

Levels come from permissions:

| Permission a player holds | Resolves to |
|---------------------------|-------------|
| `uxmessentials.vanish.use.level<N>` | Use level `N` (the **highest** numbered node they hold wins) |
| `uxmessentials.vanish.see.level<N>` | See level `N` (highest wins) |
| plain `uxmessentials.vanish.use` (no number) | Use level **1** |
| plain `uxmessentials.vanish.see` (no number) | See level **1** |
| *no `.see` node at all* | See level **0** — sees nobody who is vanished |
| op or a wildcard grant | A very high level — sees every vanished player, and hides from every non-op viewer |

<Callout type="info" title="With layered permissions off, it's just a flat toggle">

If you never grant a numbered `.level<N>` node, every vanished player is use level 1
and every `.see` holder is see level 1, so `1 >= 1` reveals and `0 >= 1` (no see
node) hides. That is exactly the classic "staff see each other, players see nobody"
behaviour — the levels are there only if you want tiers.

</Callout>

### Worked example: two staff tiers

Grant your ranks like this in your permission plugin:

```
# Moderators — plain nodes = level 1
uxmessentials.vanish.use
uxmessentials.vanish.see

# Admins — level 2
uxmessentials.vanish.use.level2
uxmessentials.vanish.see.level2
```

Now:

- An **admin** vanishes at use level 2. A **moderator** (see level 1) does **not** see
  them — `1 >= 2` is false — so an admin can hide even from staff below them.
- A **moderator** vanishes at use level 1. An **admin** (see level 2) *does* see them —
  `2 >= 1` — and can `/tp` to them (that needs `uxmessentials.vanish.see`).
- Two admins see each other (`2 >= 2`); two moderators see each other (`1 >= 1`).
- A **regular player** holds no `.see` node, so their see level is 0 and every vanished
  player stays invisible to them.

---

## What a Vanished Player Experiences

Each of these is an independent toggle in `config.conf`. They all default on (except
pickup) so a vanished admin moves through the world unseen and untouched out of the box.

| Toggle | What it does |
|--------|--------------|
| `silent-chests` | Silences the open animation and sound when a vanished player opens a **chest, shulker box, ender chest, or barrel** — the lidded containers whose open would otherwise broadcast to every nearby player and give the vanished player away. (Furnaces, hoppers, anvils and the like animate nothing, so they need no help.) |
| `pickup-items` | The **default** for whether a vanished player picks up items. Ships **off** so a vanished player doesn't silently vacuum drops; each player flips their own preference with `/vanish pickup`. |
| `night-vision` | Grants permanent night vision while vanished, with no tint or particles, so a hidden staffer can see in the dark. Removed on reappear. |
| `allow-flight` | Allows flight while vanished. On reappear the allowance is restored to your **game-mode default** — creative and spectator keep flying; survival and adventure lose the granted flight. |
| `no-hunger` | Stops hunger draining while vanished. A deliberate eat still tops the food bar back up; only the drain is blocked. |
| `no-damage` | Makes a vanished player invulnerable — cancels all incoming damage while hidden. |
| `mob-target` | Stops mobs targeting a vanished player, and drops any target a mob already had on them, so nothing paths to or attacks a hidden player. |

<Callout type="note" title="How silent containers work">

When `silent-chests` is on, opening a lidded container as a vanished player shows a
**detached copy** of its contents (which carries no block, so no animation plays);
the copy is written back to the real container when you close it. It's the same live
inventory-mirror pattern the `/invsee` and offline-container views use. The copy is a
point-in-time snapshot, so if another player edits the real chest while you have the
mirror open, your write-back on close wins — a negligible window for a quick peek.

</Callout>

---

## Fake Join / Quit Messages

To sell the illusion, a vanishing player can broadcast a fake **"left the game"** line,
and a fake **"joined the game"** line on reappear, so no viewer notices they merely
turned invisible. While vanished, the player's *real* connection lines (an actual
disconnect or reconnect) are suppressed too.

```hocon
fake-join-quit = true
fake-quit-message = "<yellow>{player} left the game"
fake-join-message = "<yellow>{player} joined the game"
fake-quit-message-staff = ""
fake-join-message-staff = ""
```

| Key | What it does |
|-----|--------------|
| `fake-join-quit` | Master switch for the whole illusion. Off, and a vanish/reappear broadcasts nothing (the player just winks out). |
| `fake-quit-message` / `fake-join-message` | The fake lines, as MiniMessage. `{player}` is replaced with the player's name. They mirror the vanilla connection format by default — change them to match your server's real join/quit style so the illusion is seamless. |
| `fake-quit-message-staff` / `fake-join-message-staff` | The lines shown **instead** to viewers who *can* see the vanishing player (staff). Left **blank** by default, so staff are sent nothing and simply watch the player wink out rather than getting a misleading "left the game". |

**The `-s` flag skips the fake broadcast entirely.** `/vanish -s` (gated by
`uxmessentials.vanish.silent`) is a truly silent vanish or reappear — nobody, staff
included, gets a line.

---

## Action Bar

```hocon
action-bar = true
```

With `action-bar` on, a vanished player sees a persistent **"You are vanished"** action
bar, refreshed once a second so it never fades. Its text is the `vanish.actionbar` entry
in your [message catalog](../config/messages.md), so it is per-locale. Turn it off to
hide the indicator entirely.

---

## Join Vanished

```hocon
join-vanished = true
```

A player who holds `uxmessentials.vanish.persist` can rejoin **already vanished**: on
join they are re-hidden before anyone sees them and their real join line is suppressed,
so a staff member who relogs stays invisible instead of announcing themselves. Without
the persist node a player reappears normally on join. (A player is dropped from the
vanish store on quit; the persist permission is what re-derives it on the next join.)

---

## Cross-Server Vanish

```hocon
cross-server = false
```

On a network, `cross-server` syncs vanish state across every backend: someone vanished
on `survival-1` arrives already hidden on `survival-2`, and `/vanish list` shows the
hidden players across the whole network (still scoped to the caller's see level). A
server hop is **not** read as an unvanish — only a genuine `/vanish` flip crosses the
bus.

<Callout type="warning" title="Needs the network bus">

Cross-server vanish requires the network bus in `network.conf` to be enabled (a
Velocity proxy or a Redis bus). It ships **off**, and is completely inert — with no
change to same-server vanish — when the bus is absent. See
[Cross-Server: Velocity & Redis](../cross-server/overview.md).

</Callout>

---

## SuperVanish & PremiumVanish

SuperVanish and PremiumVanish hide a player from the world, but they cannot hide them from
**us**: without this, a player they have hidden still shows up in our tab list, still carries a
nametag, still counts for `/list`, and can still be reached by `/msg`. Reading their state
closes that gap.

```hocon
foreign {
  read-supervanish = true
  level = 1
}
```

With `read-supervanish` on (the default) every one of our surfaces treats a player they have
hidden as vanished too. `level` is the [see level](#layered-see-use-levels) they are folded in
at, so the default `1` means anyone holding `uxmessentials.vanish.see` still sees them and
nobody else does. Raise it to hide them from your lower staff tiers as well.

**Nothing is written back.** Our `/vanish` still owns our own state, and this never vanishes or
reveals a player in the other plugin. `/vanish list` lists the players *we* vanished, and the
action bar indicator belongs to our own vanish, not theirs.

The whole block is inert when neither plugin is installed, which is the ordinary case.

---

## The `config.conf`

The complete `modules/vanish/config.conf`, with the shipped defaults:

```hocon
enabled = true

# What a vanished player experiences in the world
silent-chests = true
pickup-items  = false
night-vision  = true
allow-flight  = true
no-hunger     = true
no-damage     = true
mob-target    = true

# Connection illusions
fake-join-quit          = true
fake-quit-message       = "<yellow>{player} left the game"
fake-join-message       = "<yellow>{player} joined the game"
fake-quit-message-staff = ""
fake-join-message-staff = ""

# Indicator, persistence, network
action-bar    = true
join-vanished = true
cross-server  = false

# Another plugin's vanish (SuperVanish / PremiumVanish)
foreign {
  read-supervanish = true
  level            = 1
}
```

Config is loaded once on enable and swapped atomically on `/uxmess reload vanish`, so a
command or listener that reads it mid-reload always sees one coherent snapshot. Delete a
line and it falls back to the shipped default above, never to `false`.

---

## Permissions

| Node | Default | What it grants |
|------|---------|----------------|
| `uxmessentials.vanish.use` | `op` | `/vanish` (and `on`/`off`, `/vanish pickup`) — become invisible |
| `uxmessentials.vanish.others` | `op` | `/vanish <player>` — toggle another player's vanish |
| `uxmessentials.vanish.list` | `op` | `/vanish list` — the hidden players you may see |
| `uxmessentials.vanish.see` | `op` | See vanished players (staff-among-staff) and `/tp` to them |
| `uxmessentials.vanish.silent` | `op` | `/vanish -s` — vanish or reappear with no fake broadcast |
| `uxmessentials.vanish.persist` | `op` | Rejoin already vanished across a relog |
| `uxmessentials.module.vanish` | `op` | Reload / inspect the module (`/uxmess reload vanish`) |

**Layered level families** (optional, granted per rank — no boolean default):

| Node family | Meaning | Reduction |
|-------------|---------|-----------|
| `uxmessentials.vanish.use.level<N>` | Vanish at use level `N` | highest `<N>` wins |
| `uxmessentials.vanish.see.level<N>` | See vanished players up to level `N` | highest `<N>` wins |

Because their value space is open, the numbered nodes are not enumerated in
`paper-plugin.yml` — grant the specific `level<N>` instances your ranks need. See
[Numbered and Tiered Nodes](../permissions/reference.md#numbered-and-tiered-nodes).

---

## Next Steps

- [🔑 Permission Reference](../permissions/reference.md) — the full `uxmessentials.vanish.*` node list
- [🕵️ Staff Mode](staff-mode.md) — the on-duty toolkit that drops you into this same vanish
- [💬 Messaging](../commands/chat-messaging.md) — how `/msg` resolution respects vanish
- [🌐 Cross-Server: Velocity & Redis](../cross-server/overview.md) — the bus that `cross-server` rides on
- [🧩 Per-Module Config](../config/per-module.md) — where `modules/vanish/config.conf` lives and how it's loaded

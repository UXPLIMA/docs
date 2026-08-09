---
title: Built-in Menu Guide
order: 1080
description: 'Almost every GUI you open in uxmEssentials — the home grid, the warp
  browser, the kit list, the vault selector, the /eco and /mod admin panels, the module
  hub behind /uxmess gui — is drawn by one shared menu engine. There is no separate
  "GUI system" bolted onto each feature: the same code that loads your own menus/.conf
  files loads and renders the built-in menus too.'
---

That has two practical consequences worth knowing before you touch anything:

- **They all behave the same way.** Pagination, click gestures, refresh-on-change,
  Bedrock form fallback, sounds — a habit you learn on one menu carries to every menu.
- **They are all themeable.** Because the built-ins run on the engine, the same
  vocabulary you use for a [custom menu](engine.md) — materials, MiniMessage names and
  lore, [actions and requirements](actions-requirements.md) — is what shapes them. You
  are never stuck with the shipped look.

---

## The menus you already have

These open from commands (or from clicking through another menu). You do not create or
enable them — they ship with their module and appear as soon as that module is on.

| Menu | Opens with | What it does |
|---|---|---|
| **Home grid** | `/home` | A slot-based grid of your homes. Click a slot to teleport; empty slots create; icons, renames, invites and deletes all happen inside the grid. |
| **Warp browser** | `/warp`, `/warp list` | Every warp you are allowed to use, one icon each. Click to teleport (pays the cost / prompts for a password if set). |
| **Warp editor** | `/warp editor [name]` | The staff-side editor for a warp: cost, permission gates, arrival effects, welcome message, icon. |
| **Kit browser** | `/kit`, `/kit list` | Claimable kits with their cooldown state shown live. Click a ready kit to claim it; click a locked one to see why. |
| **Vault selector** | `/vault` (with several) | A picker across your vaults when you own more than one; each tile shows its name and icon. Click to open that vault. |
| **Player-warp list** | `/pwarp` | Manage and browse player-owned warps. |
| **Economy admin** | `/eco` | The staff economy hub — give / take / set balances, history and logs, notes and backups — routed to the matching sub-panels. |
| **Moderation panel** | `/mod` | The staff moderation hub; many bare commands (`/ban`, `/mute`, …) also open a picker → confirm flow built on the engine. |
| **Module hub** | `/uxmess gui` | The management hub: a tile per module leading into that module's own settings panels. |
| **Per-module settings** | `/tpsettings`, `/msgsettings`, `/presencesettings`, `/scoreboard gui`, … | The personal and admin settings panels each module exposes. |

### What is on the module hub

`/uxmess gui` draws one tile per module that has a screen you can open from nothing, and
hides any tile you lack the permission for, so two staff members with different ranks see
different hubs. Clicking a tile opens exactly the same screen the module's own command
opens, under the same permission node:

| Tile | Same as typing |
|---|---|
| Worlds, Homes, Warps, Kits, Vaults, Player-warps | `/worlds`, `/home`, `/warp`, `/kit`, `/vault`, `/pwarp` |
| Economy, Moderation, Item world, Communication | `/eco`, `/mod`, `/itemworld`, the communication admin panel |
| Teleport, Presence, Messaging (settings + mailbox) | `/tpsettings`, `/presencesettings`, `/msgsettings`, `/mail` |
| Holograms, NPCs, Scoreboard, Custom menus | `/hologram list`, `/npc list`, `/scoreboard gui`, `/menu editor` |
| Survival, Poses, Ranks, Vote, Regions, Staff, Discord | `/survival`, `/poses`, `/ranks`, `/vote`, `/regions`, `/stafflist`, the Discord status panel |

A few modules are deliberately absent:

- **`tablist`, `nametags`, `commandcontrol`, `servertweaks`, `vanish`** have no GUI of
  their own; they are configured entirely in their `.conf` files.
- **`playerstate`, `invrollback`, `villagers`, `trade`, `security`** do have GUIs, but
  each one opens **onto a subject you name first**: a player (`/invsee Bob`,
  `/invrestore Bob`), the villager you clicked, the partner who accepted your trade, or
  your own PIN prompt. There is nothing for a hub tile to open.

<Callout type="info" title="These are `management` GUIs, not custom menus">

The built-in panels live in each module's `modules/<module>/gui/` folder as their own
`.conf` files. They are loaded by the always-on `management` framework and styled with
the same engine grammar as your `menus/*.conf` files — but they are wired to
type-safe feature handlers, so a click there does real work (charging a balance,
issuing a ban) that a plain custom menu could not do on its own.

</Callout>

---

## What every built-in menu shares

Because they all run on the one engine, the following is true of the home grid, the kit
browser, the `/eco` panels and every other built-in alike.

### Click gestures

Menus react differently to different clicks. A left-click usually performs the primary
action; a right-click, shift-click or middle-click may do something secondary (preview
instead of claim, delete instead of open). Where a menu uses more than one gesture on the
same tile, its lore says so.

### Live refresh

Tiles that show a changing value — a kit's remaining cooldown, your balance in the
economy panel, a home's occupancy — re-render on their own while the menu is open. You do
not need to close and reopen to see the current number.

### Pagination

Lists longer than one page (many warps, a long transaction history) page with the arrow
tiles. The page controls sit on pinned slots so they never move as the contents scroll.

### Bedrock players get a native form

A player joining through Floodgate/Geyser does not see a chest GUI — the same menu is
rendered as a **native Bedrock form**. Storage-style menus that show real item stacks
(the vault, the inventory viewer) stay as a chest even on Bedrock, because a form cannot
hold item stacks. See [Bedrock Forms](bedrock.md).

---

## Re-theming a built-in menu

You theme a built-in the same way you author a custom one — you just edit the file the
module already ships instead of creating a new one.

1. Find the panel under `plugins/uxmEssentials/modules/<module>/gui/<menu>.conf`.
2. Change what you want: an item's `material`, its `name` and `lore` (verbatim
   MiniMessage), its `slot`, or add `decor { glow = true }`.
3. Reload just that module: `/uxmess reload <module>`.

<Callout type="warning" title="Change the look, keep the wiring">

In a built-in panel, the item **ids** and their `click` bindings are what connect a
tile to real behaviour (charging a balance, teleporting home). Re-skin freely —
material, name, lore, slot, glow — but do not rename or delete the bound ids unless
you mean to remove that button. If a file fails to load after an edit, only that one
panel refuses; the log names the file and line.

</Callout>

Delete a key to fall back to its shipped default; delete the whole file to regenerate it
from defaults on next load.

---

## Windows that hold real items

A handful of built-ins are not just rows of buttons: they contain real item stacks that
move. The trade window, `/invsee` and `/endersee`, the villager trade manager and the
inventory-rollback snapshot preview all show items you (or the player you are inspecting)
actually own.

These are spec files like every other built-in, with one addition. The file hands a block
of slots over to the module with a `content {}` block, and from there on the module owns
what happens inside it:

``` hocon
content {
  "playerstate:invsee" {
    slots    = ["0-40"]
    editable = true
  }
}
```

Everything outside that block is ordinary chrome: a backdrop, a label, a button. Inside
it, the module decides what is drawn and which movements are legal, because that is where
the rules that stop an item being duplicated live.

| Window | File | Region | Holds | Editable |
|---|---|---|---|---|
| Trade | `modules/trade/gui/trade.conf` | `trade:offer` | what you are offering | yes |
| Trade | `modules/trade/gui/trade.conf` | `trade:mirror` | what the other player is offering | no |
| Cross-server trade | `modules/trade/gui/trade-cross.conf` | `trade:cross-offer` | your side of a trade across servers | yes |
| Inventory viewer | `modules/playerstate/gui/invsee.conf` | `playerstate:invsee` | the target's inventory, armour and offhand | with `uxmessentials.invsee.modify` |
| Ender viewer | `modules/playerstate/gui/endersee.conf` | `playerstate:endersee` | the target's ender chest | yes |
| Villager trade manager | `modules/villagers/gui/trade-manager.conf` | `villagers:trades` | a villager's buy and sell stacks | yes |
| Snapshot preview | `modules/invrollback/gui/snapshot-preview.conf` | `invrollback:snapshot` | a stored inventory snapshot | no, it is a preview |

### What you can change

Move the region wherever you like, and reshape it: the slot list is read straight out of
your file, and nothing in the module assumes where the slots sit. Restyle the chrome
around it as freely as any other panel.

Three things are not yours to change:

- **The region id.** It is what connects the block of slots to the module behind it.
  Rename it and the module no longer recognises its own window.
- **The slot count, where the window mirrors something fixed.** An inventory viewer needs
  exactly 41 slots and an ender viewer 27, because that is how many the thing it mirrors
  has; the snapshot preview needs 45; the two trade regions must be the same size as each
  other, since one is the mirror of the other. A file that does not add up is refused by
  name, with the count it needed, instead of opening a window whose slots do not line up.
- **The slot order, where it means something.** In the villager trade manager the slots
  come in threes, in the order buy, buy, sell, one triple per trade. Five triples is five
  editable trades; a villager with more trades keeps the rest untouched behind the window.
  Declare more triples for more editable trades, but keep the count a multiple of three
  and keep each triple in that order. There is one `villagers:remove-N` action per triple,
  numbered from 1 in region order, so a remove button that moves rows is renumbered rather
  than guessed from where it sits.

<Callout type="info" title="Read-only means read-only">

A region marked `editable = false` (the trade mirror, the snapshot preview) refuses
every movement: clicks, drags, shift-clicks, hotbar swaps and double-clicks alike. You
cannot make it editable from the file, and that is deliberate: those stacks are a view
of something that belongs to someone else.

</Callout>

---

## Next Steps

- [Custom Menu Engine](engine.md) — build your own menu in `menus/<name>.conf`.
- [Actions & Requirements](actions-requirements.md) — the click and visibility vocabulary
  every menu, built-in or custom, is made of.
- [Bedrock Forms](bedrock.md) — how a menu renders for Floodgate players.
- [Per-Module Config](../config/per-module.md) — where each module's `gui/` panels live.

---
title: Items & World
order: 1290
description: The Item & World module (itemworld) is uxmEssentials' toolbox of item,
  block, entity and environment utilities — roughly the entire EssentialsX "everything
  else" surface, around 65 commands, in one module. It is the largest module in the
  plugin.
---

Because it is so broad, it is built to be **carved up**: seven independent
sub-feature groups you can enable or disable individually, per-command switches
inside each group, and per-type permission opt-outs on the sharpest verbs. Two
further sub-features — the held-item editor (`/itemedit`) and in-inventory shulker
editing — sit alongside the seven groups with switches of their own.

---

## The Seven Sub-Feature Groups

Every command belongs to one of seven groups. A disabled group takes all of its
commands offline at once.

| Group | What it covers | Representative commands |
|-------|----------------|-------------------------|
| **item-utils** | Give / edit / inspect held items | `/give`, `/item`, `/itemname`, `/itemlore`, `/enchant`, `/repair`, `/hat`, `/skull`, `/itemdb` |
| **workstations** | Portable crafting / utility blocks | `/anvil`, `/workbench`, `/enderchest`, `/grindstone`, `/loom`, `/smithingtable`, `/furnace` |
| **cleanup** | Tidy inventories and drops | `/disposal`, `/condense`, `/enderclear` |
| **powertool** | Bind commands to a held item | `/powertool`, `/powertoollist`, `/powertooltoggle` |
| **mob-entity** | Spawn / count / purge entities | `/spawnmob`, `/spawner`, `/kill`, `/butcher`, `/killall`, `/remove`, `/entitycount` |
| **time-weather** | Change time and weather | `/time`, `/weather`, `/day`, `/night`, `/sun`, `/rain`, `/thunder` |
| **admin-fun** | The joke / spectacle verbs | `/lightning`, `/fireball`, `/kittycannon`, `/nuke`, `/tree`, `/beezooka` |

<Callout type="note" title="Grouping follows the module's own group names">

The seven group ids above (`item-utils`, `workstations`, `cleanup`, `powertool`,
`mob-entity`, `time-weather`, `admin-fun`) are the switches in `itemworld.conf`.
The per-command split between them mirrors those names.

</Callout>

---

## What's Useful For What

**Building & item work**

| I want to… | Use |
|------------|-----|
| Give myself or a player an item | `/give`, `/giveall`, `/item`, `/more` |
| Rename / re-lore / re-flag a held item | `/itemname`, `/itemlore`, `/itemflag` |
| Edit a held item end to end (name, lore, enchants, flags, attributes) | `/itemedit` |
| Edit a shulker box's contents without placing it | sneak-right-click it in your inventory |
| Enchant, repair or make unbreakable | `/enchant`, `/disenchant`, `/repair`, `/repairall`, `/unbreakable` |
| Wear an item as a hat | `/hat` |
| Look up an item, its recipe, or its id | `/itemdb`, `/iteminfo`, `/recipe`, `/showitem` |

**Portable stations & cleanup**

| I want to… | Use |
|------------|-----|
| Craft / repair / smith on the go | `/workbench`, `/anvil`, `/smithingtable`, `/grindstone` |
| Open an ender chest or furnace | `/enderchest`, `/furnace` |
| Throw away or compact items | `/disposal`, `/condense` |

**Entities & environment**

| I want to… | Use |
|------------|-----|
| Spawn mobs or a spawner | `/spawnmob`, `/spawner` |
| Clear mobs from an area | `/butcher`, `/killall`, `/remove` |
| Count nearby entities | `/entitycount` |
| Set time or weather | `/time`, `/weather`, `/day`, `/night`, `/sun`, `/rain` |

**Power tools**

Bind a command to whatever you're holding: `/powertool <command>` makes the held
item run that command when you use it. `/powertoollist` shows your bindings and
`/powertooltoggle` switches them on and off.

---

## The Held-Item Editor & In-Inventory Shulker Editing

Two sub-features round out the item side of the module. Each carries its own switch in
`itemworld.conf` and its own permission, independent of the seven command groups.

### `/itemedit` — the all-in-one held-item editor

`/itemedit` edits whatever is in your main hand, in place. It gathers the jobs the
separate `/itemname`, `/itemlore`, `/enchant`, `/itemflag` and `/unbreakable` verbs do
into one command and adds attributes, durability and custom model data on top. Names and
lore accept MiniMessage, and a rename strips the default italic so the styling you type
is the styling that shows.

| Area | Subcommands |
|------|-------------|
| Name | `rename <name>`, `resetname` |
| Lore | `lore add <line>`, `lore set <index> <line>`, `lore insert <index> <line>`, `lore remove <index>`, `lore clear` |
| Enchants | `enchant <enchant> <level>` (a flag lets the level pass the vanilla ceiling), `enchant <enchant> remove` |
| Flags | `flag <ItemFlag> [on\|off]` — hide enchants, hide attributes, and the rest |
| Attributes | `attribute add <attribute> <amount> <slot>`, `attribute remove <attribute>` |
| Durability | `durability <value>`, `repair`, `unbreakable [on\|off]` |
| Model | `custom-model-data <int>` |

**A bare `/itemedit` opens a panel** instead: a live preview slot shows the held item and
a row of controls covers every edit the subcommands expose — rename and reset, lore add /
remove / clear, enchants through a picker, item-flag toggles, unbreakable, custom model
data, durability and repair. Both routes apply the same changes and report the same
feedback, so use whichever suits: the panel for a quick tweak, the subcommands for a
script or a macro.

The panel captures the item type when it opens and closes gracefully if your hand empties
or the item changes, so it can never write to the wrong item. Re-skin it in
`modules/itemworld/gui/itemedit.conf`.

Gate it with `item-edit { enabled = true }` and the permission
`uxmessentials.itemworld.itemedit` (default `op`).

### Editing a shulker box from the inventory

Sneak-right-click a shulker box **item** in your inventory and its 27 slots open as an
editable menu — drop items in, take items out, rearrange them — and everything is written
back into the box when you close the menu. It's the AxShulkers-style "peek inside without
placing it" convenience, kept safe on two counts:

- The source box is **locked in place while the menu is open**, so it can't be moved or
  dropped from another slot mid-edit — there is no dupe window.
- **Nesting is refused**: you can't drop a shulker box inside a shulker box.

Set `require-sneak = false` to open on any right-click instead of a sneak-right-click.
Gate it with `shulkers { enabled = true, require-sneak = true }` and the permission
`uxmessentials.itemworld.shulker` (default `op`).

---

## Enabling & Disabling

Everything is controlled from `modules/itemworld/config.conf`. Disable a whole group,
or leave a group on and switch off individual commands within it.

```hocon
# modules/itemworld/config.conf
enabled = true

groups {
  item-utils   { enabled = true }
  workstations { enabled = true }
  cleanup      { enabled = true }
  powertool    { enabled = true }
  mob-entity   { enabled = true }
  time-weather { enabled = true }
  admin-fun    { enabled = false }   # e.g. keep the joke verbs off a serious server
}

# the held-item editor and in-inventory shulker editing
item-edit { enabled = true }
shulkers  { enabled = true, require-sneak = true }   # require-sneak = false -> any right-click opens

# safety caps
give-cap = 64
more-cap = 64
max-enchant-level = 255
spawnmob-cap = 50
purge-max-radius = 200
give-audit-threshold = 64   # audit-log any /give above this amount

audit {
  # per-verb audit switches, e.g.
  # nuke = true
}
```

<Callout type="info" title="Two layers of gating">

A command is available only if **its group is enabled** *and* its own per-command
switch is on. On top of that, the command's permission node must be granted. A
disabled group registers none of its commands at all.

</Callout>

---

## Per-Type Opt-Out Nodes

Three commands accept a *type*, and each type is individually permissible. The parent
wildcards default to `true` (everything allowed), so you deny by granting the specific
node `false`:

| Node pattern | Restricts |
|--------------|-----------|
| `uxmessentials.itemworld.give.<material>` | Which materials `/give` may hand out |
| `uxmessentials.itemworld.enchant.<enchant>` | Which enchantments `/enchant` may apply |
| `uxmessentials.itemworld.spawnmob.<mob>` | Which mobs `/spawnmob` may spawn |

For example, set `uxmessentials.itemworld.give.bedrock = false` on a helper rank to
let them give most items but never hand out bedrock.

---

## Audit Logging

The abusable, destructive and admin-fun verbs are recorded to the audit log. That
covers area purges (`/killall`, `/remove`, `/butcher`), spectacle verbs
(`/nuke`, `/lightning`, `/fireball`, …), and large `/give` amounts once they cross
`give-audit-threshold`. The `audit { }` block lets you turn individual verbs' logging
on or off. Caps like `purge-max-radius` and `spawnmob-cap` bound how much damage a
single command can do.

---

## Permissions

Each command has its own node under `uxmessentials.itemworld.*` (see the command
reference for the full list). Most are staff-default (`op`); the read-only inspection
verbs (`/itemdb`, `/iteminfo`, `/recipe`) are self-service. Workstations add a
`.workstation.others` node to open a station *for* someone else. The held-item editor
uses `uxmessentials.itemworld.itemedit`, and opening a shulker box from the inventory
uses `uxmessentials.itemworld.shulker` — both default `op`. The module's reload tier is
`uxmessentials.module.itemworld`.

---

## Tips & Gotchas

<Callout type="warning" title="Whole-world purges and Folia">

`/killall` and `/remove` at world scope, and `/gc`'s entity count, do a
whole-world entity scan. This is a known residual limitation under Folia and is
best used with a radius rather than server-wide on very large worlds.

</Callout>

- **Turn `admin-fun` off** on survival/economy servers — those verbs (`/nuke`, `/kittycannon`, `/antioch`) are spectacle tools, not gameplay.
- Per-type opt-outs are the clean way to hand `/give` to junior staff without the risk of spawn eggs, bedrock, or command blocks.
- The `time-weather` group also ships short aliases (`/day`, `/night`, `/sun`, `/rain`) gated by `.time.alias` / `.weather.alias`.

---

## Next Steps

- [📟 Items, Blocks & World Commands](../commands/items-world.md) - Every verb, grouped
- [🔐 Permission Reference](../permissions/reference.md) - Per-command and per-type nodes
- [⚙️ Per-Module Config](../config/per-module.md) - The `itemworld` groups & caps

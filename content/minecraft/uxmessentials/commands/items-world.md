---
title: Items, Blocks & World
order: 1010
---

## Item & World

The **Item & World** module is the largest in uxmEssentials: roughly 65 verbs covering item editing, virtual workstations, inventory cleanup, powertools, mob/entity control, time/weather shortcuts, and admin fun. Internally it splits into **seven sub-feature groups** that can be toggled independently, and on top of that every single command can be switched off in `itemworld.conf`. Everything is Brigadier, so all of it is renameable and re-aliasable in [`commands/commands.conf`](../config/commands-conf.md).

<Callout type="warning" title="These verbs are gated twice, and some are logged">

A command runs only when **both** its sub-feature group and its own `enabled` flag are on in `itemworld.conf`. The abusable, purge, and admin-fun verbs (`/nuke`, `/killall`, `/remove`, `/butcher`, `/kittycannon`, `/antioch`, and friends) are **audit-logged** every time they fire, so there is always a record of who blew up what.

</Callout>

<Callout type="note" title="Per-type opt-out nodes">

`/give`, `/enchant`, and `/spawnmob` layer a per-type sub-node on top of the base permission: `uxmessentials.itemworld.give.<material>`, `uxmessentials.itemworld.enchant.<enchant>`, and `uxmessentials.itemworld.spawnmob.<mob>`. The wildcard parents default to `true`, so grant is the norm; set a specific one to `false` to block, say, `/give` of bedrock without touching the rest of the command.

</Callout>

---

## Item utilities

Editing, generating, and inspecting held items. Item names and lore accept MiniMessage formatting.

| Command | Aliases | Permission |
|---------|---------|------------|
| `/give <player> <item> [amount]` | `i` | `uxmessentials.give.use` |
| `/giveall <item> [amount]` | - | `uxmessentials.giveall.use` |
| `/item <item> [amount]` | - | `uxmessentials.item.use` |
| `/itemname [name]` | `iname` | `uxmessentials.itemname.use` |
| `/itemlore <set\|add> <text>` · `/itemlore clear` | `lore` | `uxmessentials.itemlore.use` |
| `/itemflag <flag> <on\|off>` | `iflag` | `uxmessentials.itemflag.use` |
| `/skull [player]` | - | `uxmessentials.skull.use` |
| `/firework …` | - | `uxmessentials.firework.use` |
| `/potion <effect> [duration] [amplifier]` | - | `uxmessentials.potion.use` |
| `/book` | - | `uxmessentials.book.use` |
| `/more` | - | `uxmessentials.more.use` |
| `/itemamount <n>` | `amount` | `uxmessentials.itemamount.use` |
| `/itemdamage <n>` | `durability` | `uxmessentials.itemdamage.use` |
| `/repair` · `/repairall` | - | `uxmessentials.repair.itemworld` |
| `/enchant <enchant> [level]` | - | `uxmessentials.enchant.use` |
| `/disenchant [all\|<enchant>]` | - | `uxmessentials.disenchant.use` |
| `/hat` | - | `uxmessentials.hat.use` |
| `/itemdb [item]` | `idb` | `uxmessentials.itemdb.use` |
| `/iteminfo` | - | `uxmessentials.iteminfo.use` |
| `/recipe [item]` | - | `uxmessentials.recipe.use` |
| `/showitem` | - | `uxmessentials.showitem.use` |
| `/unbreakable [true\|false]` | - | `uxmessentials.unbreakable.use` |
| `/itemmodel <id>\|clear` | `custommodeldata` | `uxmessentials.itemmodel.use` |
| `/editsign` | - | `uxmessentials.editsign.use` |
| `/copyinv <player>` | - | `uxmessentials.copyinv.use` |
| `/endercopy <player>` | - | `uxmessentials.endercopy.use` |

### The held-item editor: `/itemedit`

`/itemedit` is a single command that edits the item in your main hand in place: the jobs `/itemname`, `/itemlore`, `/enchant`, `/itemflag` and `/unbreakable` do, plus attributes, durability and custom model data, under one verb. Names and lore lines accept MiniMessage, and a rename strips the default italic so your formatting is what shows. A bare `/itemedit` opens a GUI panel that does the same edits by clicking, with a live preview of the held item. It sits in its own `item-edit` sub-feature (`item-edit { enabled }` in `itemworld.conf`) behind `uxmessentials.itemworld.itemedit` (default `op`).

| Subcommand | What it does |
|------------|--------------|
| `rename <name>` | Set the display name (MiniMessage). |
| `resetname` | Clear the display name back to the item's default. |
| `lore add <line>` | Append a lore line. |
| `lore set <index> <line>` | Replace the line at `<index>`. |
| `lore insert <index> <line>` | Insert a line at `<index>`, pushing the rest down. |
| `lore remove <index>` | Delete the line at `<index>`. |
| `lore clear` | Remove all lore. |
| `enchant <enchant> <level>` | Add an enchantment; a flag lets `<level>` exceed the vanilla maximum. |
| `enchant <enchant> remove` | Take an enchantment off. |
| `flag <ItemFlag> [on\|off]` | Toggle an item flag: `HIDE_ENCHANTS`, `HIDE_ATTRIBUTES`, and the rest. |
| `attribute add <attribute> <amount> <slot>` | Add an attribute modifier bound to an equipment slot. |
| `attribute remove <attribute>` | Remove an attribute modifier. |
| `durability <value>` | Set the item's remaining durability. |
| `repair` | Repair the held item to full. |
| `unbreakable [on\|off]` | Toggle the unbreakable flag. |
| `custom-model-data <int>` | Set the custom model data value used by resource packs. |

<Callout type="tip" title="Editing a shulker without placing it">

Sneak-right-click a shulker box **item** in your inventory to open its 27 contents and edit them in place: no command, no placing the box. See the [feature guide](../features/item-world.md#the-held-item-editor-in-inventory-shulker-editing) for the safety rules and the `shulkers { }` switch.

</Callout>

---

## Workstations

Open a crafting UI anywhere, no block required. Add `uxmessentials.workstation.others` to open one **for** another player. `/furnace` also accepts an optional `[player]`.

| Command | Aliases | Permission |
|---------|---------|------------|
| `/workbench` | `craft` | `uxmessentials.workstation.workbench` |
| `/anvil` | - | `uxmessentials.workstation.anvil` |
| `/enderchest` | `echest` | `uxmessentials.workstation.enderchest` |
| `/grindstone` | - | `uxmessentials.workstation.grindstone` |
| `/cartography` | - | `uxmessentials.workstation.cartography` |
| `/loom` | - | `uxmessentials.workstation.loom` |
| `/smithingtable` | - | `uxmessentials.workstation.smithingtable` |
| `/stonecutter` | - | `uxmessentials.workstation.stonecutter` |
| `/furnace [player]` | - | `uxmessentials.workstation.furnace` |

---

## Cleanup

Quick inventory housekeeping.

| Command | Aliases | Permission |
|---------|---------|------------|
| `/disposal` | `trash` | `uxmessentials.disposal.use` |
| `/condense` | `compact` | `uxmessentials.condense.use` |
| `/enderclear [player]` | `clearec` | `uxmessentials.enderclear.use` |

---

## Powertool

Bind a command to the item in your hand, then trigger it by using that item.

| Command | Aliases | Permission |
|---------|---------|------------|
| `/powertool [command…]` | `pt` | `uxmessentials.powertool.use` |
| `/powertoollist` | - | `uxmessentials.powertool.use` |
| `/powertooltoggle` | - | `uxmessentials.powertool.toggle` |

---

## Mob & entity

Spawning and purging entities. The purge verbs are audit-logged.

| Command | Permission |
|---------|------------|
| `/spawnmob <type> [amount]` | `uxmessentials.spawnmob.use` |
| `/spawner <type>` | `uxmessentials.spawner.use` |
| `/kill [player\|entity]` | `uxmessentials.kill.use` |
| `/butcher [radius]` | `uxmessentials.butcher.use` |
| `/killall [type]` | `uxmessentials.killall.use` |
| `/remove <type> [radius]` | `uxmessentials.remove.use` |
| `/entitycount [radius]` | `uxmessentials.entitycount.use` |
| `/unlimited` | `uxmessentials.unlimited.use` |

---

## Time & weather

Shortcuts on top of the vanilla time/weather controls. The `/day`, `/night`, `/sun`, `/rain`, `/thunder` aliases resolve to the alias-tier nodes below.

| Command | Permission |
|---------|------------|
| `/time <set\|add> <value>` | `uxmessentials.time.use` |
| `/weather <clear\|rain\|thunder> [duration]` | `uxmessentials.weather.use` |
| `/day` · `/night` | `uxmessentials.time.alias` |
| `/sun` · `/rain` · `/thunder` | `uxmessentials.weather.alias` |

---

## Admin fun

The joke/chaos verbs. **All of these are audit-logged.**

| Command | Aliases | Permission |
|---------|---------|------------|
| `/lightning [player]` | `smite` | `uxmessentials.lightning.use` |
| `/fireball` | `grenade` | `uxmessentials.fireball.use` |
| `/kittycannon` | - | `uxmessentials.kittycannon.use` |
| `/antioch` | - | `uxmessentials.antioch.use` |
| `/beezooka` | `beecannon` | `uxmessentials.beezooka.use` |
| `/break` | - | `uxmessentials.break.use` |
| `/tree <type>` | - | `uxmessentials.tree.use` |
| `/bigtree <type>` | `largetree` | `uxmessentials.tree.use` |
| `/nuke [player]` | - | `uxmessentials.nuke.use` |

---

## Management GUI

| Command | Permission |
|---------|------------|
| `/itemworld [gui]` | `uxmessentials.itemworld.gui` |

The module also answers to the reload/inspect tier `uxmessentials.module.itemworld`, used by `/uxmess reload itemworld`.

---

## Examples

```
/give Steve diamond 16       # give Steve 16 diamonds
/i golden_apple 5            # give yourself 5 (alias of /item)
/itemname <gold>Excalibur    # MiniMessage-formatted item name
/itemedit rename <gradient:#ff5555:#ffaa00>Blade   # all-in-one held-item editor
/itemedit lore add <gray>Forged in the deep        # append a lore line
/enchant sharpness 5         # enchant the held item
/anvil                       # open a virtual anvil, no block needed
/condense                    # compact your inventory into blocks
/powertool /warp spawn       # bind a warp to the held item
/spawnmob zombie 3           # spawn three zombies
/killall arrow               # purge dropped arrows (logged)
/day                         # set the time to day
```

---

## Next Steps

- [🧱 Item & World Feature Guide](../features/item-world.md): sub-feature groups, config flags, and audit log
- [🔐 Permissions Reference](../permissions/reference.md): base nodes plus the per-type opt-out tiers
- [⚙️ Renaming Commands](../config/commands-conf.md): rename or disable any of these verbs

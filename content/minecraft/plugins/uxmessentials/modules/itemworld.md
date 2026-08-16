---
title: Items & World
order: 9090
description: 'The item, block, entity and environment toolbox: roughly 65 commands in seven switchable groups.'
---

Items & World is the toolbox: give and edit held items, portable workstations, inventory cleanup, powertools,
mob and entity verbs, time and weather aliases, and the admin-fun spectacle commands. It is the largest module
in the plugin, so it is built to be carved up: seven groups you switch on or off independently, per-command
switches inside each, and per-type opt-outs on the sharpest verbs.

Module `itemworld` · enabled by default · `modules/itemworld/config.conf`

## Commands

{/* generated:commands */}
| Command | What it does | Permission |
|---|---|---|
| `/antioch` (`/grenade`) | Throw a primed TNT grenade (audit-logged). | `uxmessentials.antioch.use` |
| `/anvil` | Open a virtual anvil. | `uxmessentials.workstation.anvil` |
| `/beezooka` (`/beecannon`) | Launch an angry bee (audit-logged). | `uxmessentials.beezooka.use` |
| `/bigtree` (`/largetree`) | Generate a tree of the given type where you are looking (audit-logged). | `uxmessentials.tree.use` |
| `/book` | Unlock a written book for editing. | `uxmessentials.book.use` |
| `/break` | Instantly break the block you are looking at (audit-logged). | `uxmessentials.break.use` |
| `/butcher` | Purge nearby mobs (audit-logged). | `uxmessentials.butcher.use` |
| `/cartography` | Open a virtual cartography table. | `uxmessentials.workstation.cartography` |
| `/condense` (`/compact`) | Recipe-stack inventory items. | `uxmessentials.condense.use` |
| `/copyinv` | Copy a player's inventory into yours. | `uxmessentials.copyinv.use` |
| `/day` | /day / /night quick time aliases. | `uxmessentials.time.alias` |
| `/disenchant` | Remove all or one enchantment from the held item. | `uxmessentials.disenchant.use` |
| `/disposal` (`/trash`) | Open a throwaway GUI. | `uxmessentials.disposal.use` |
| `/editsign` | Edit the sign you are looking at (respects build access). | `uxmessentials.editsign.use` |
| `/enchant` | Enchant the held item (level clamped at the boundary). | `uxmessentials.enchant.use` |
| `/enderchest` (`/echest`) | Open your ender chest. | `uxmessentials.workstation.enderchest` |
| `/enderclear` (`/clearec`) | Clear an ender chest. | `uxmessentials.enderclear.use` |
| `/endercopy` | Copy a player's ender chest into yours. | `uxmessentials.endercopy.use` |
| `/entitycount` | Tally nearby entities by type for lag diagnosis. | `uxmessentials.entitycount.use` |
| `/fireball` | Launch a fireball (audit-logged). | `uxmessentials.fireball.use` |
| `/firework` | Style or power the held firework rocket. | `uxmessentials.firework.use` |
| `/furnace` | Open a virtual furnace. | `uxmessentials.workstation.furnace` |
| `/give` (`/i`) | Give an item to a player; bulk gives are audited. | `uxmessentials.give.use` |
| `/giveall` | Give an item to every online player; bulk gives are audited per recipient. | `uxmessentials.giveall.use` |
| `/grindstone` | Open a virtual grindstone. | `uxmessentials.workstation.grindstone` |
| `/hat` | Wear the held item as a helmet (itemworld-owned; playerstate defers it). | `uxmessentials.hat.use` |
| `/item` | Give an item to yourself. | `uxmessentials.item.use` |
| `/itemamount` (`/amount`) | Set the held stack amount, clamped to the give cap. | `uxmessentials.itemamount.use` |
| `/itemdamage` (`/durability`) | Set the held item's durability damage. | `uxmessentials.itemdamage.use` |
| `/itemdb` (`/idb`) | Look up an item's id / data. | `uxmessentials.itemdb.use` |
| `/itemedit` | Edit the held item's name, lore and meta (item-edit.enabled). | `uxmessentials.itemworld.itemedit` |
| `/itemflag` (`/iflag`) | Toggle an item meta flag. | `uxmessentials.itemflag.use` |
| `/iteminfo` | Inspect the metadata of the item in your hand. | `uxmessentials.iteminfo.use` |
| `/itemlore` (`/lore`) | Edit the held item's lore. | `uxmessentials.itemlore.use` |
| `/itemmodel` (`/custommodeldata`) | Set or clear the held item's custom model data. | `uxmessentials.itemmodel.use` |
| `/itemname` (`/iname`) | Rename the held item. | `uxmessentials.itemname.use` |
| `/itemworld` | Open the itemworld utilities hub. | `uxmessentials.itemworld.gui` |
| `/kill` | Kill a target (audit-logged). | `uxmessentials.kill.use` |
| `/killall` | Purge entities world-wide (audit-logged). | `uxmessentials.killall.use` |
| `/kittycannon` | Launch an exploding cat (audit-logged). | `uxmessentials.kittycannon.use` |
| `/lightning` (`/smite`) | Strike lightning (audit-logged). | `uxmessentials.lightning.use` |
| `/loom` | Open a virtual loom. | `uxmessentials.workstation.loom` |
| `/more` | Fill the held stack to max (itemworld-owned; playerstate defers it). | `uxmessentials.more.use` |
| `/night` | /day / /night quick time aliases. | `uxmessentials.time.alias` |
| `/nuke` | Rain lightning over an area (audit-logged). | `uxmessentials.nuke.use` |
| `/potion` | Add a potion effect to the held potion. | `uxmessentials.potion.use` |
| `/powertool` (`/pt`) | Bind a command to the held item. | `uxmessentials.powertool.use` |
| `/powertoollist` | Bind a command to the held item. | `uxmessentials.powertool.use` |
| `/powertooltoggle` | Enable/disable your powertool bindings. | `uxmessentials.powertool.toggle` |
| `/rain` | /sun / /rain / /thunder quick weather aliases. | `uxmessentials.weather.alias` |
| `/recipe` | Show an item's crafting recipe. | `uxmessentials.recipe.use` |
| `/remove` | Remove entities by type (audit-logged). | `uxmessentials.remove.use` |
| `/repair` | /repair and /repairall in the itemworld surface (itemworld-owned; playerstate defers them). | `uxmessentials.repair.itemworld` |
| `/repairall` | /repair and /repairall in the itemworld surface (itemworld-owned; playerstate defers them). | `uxmessentials.repair.itemworld` |
| `/showitem` | Broadcast the held item to chat for everyone online. | `uxmessentials.showitem.use` |
| `/skull` | Get a player-head skull. | `uxmessentials.skull.use` |
| `/smithingtable` | Open a virtual smithing table. | `uxmessentials.workstation.smithingtable` |
| `/spawner` | Set a spawner's mob type (audit-logged). | `uxmessentials.spawner.use` |
| `/spawnmob` | Spawn mobs (audit-logged). | `uxmessentials.spawnmob.use` |
| `/stonecutter` | Open a virtual stonecutter. | `uxmessentials.workstation.stonecutter` |
| `/sun` | /sun / /rain / /thunder quick weather aliases. | `uxmessentials.weather.alias` |
| `/thunder` | /sun / /rain / /thunder quick weather aliases. | `uxmessentials.weather.alias` |
| `/time` | Per-world time. | `uxmessentials.time.use` |
| `/tree` | Generate a tree of the given type where you are looking (audit-logged). | `uxmessentials.tree.use` |
| `/unbreakable` | Toggle or set the held item's unbreakable flag. | `uxmessentials.unbreakable.use` |
| `/unlimited` | Toggle unlimited placement of held blocks. | `uxmessentials.unlimited.use` |
| `/weather` | /weather \<clear\|rain\|thunder> [duration]. | `uxmessentials.weather.use` |
| `/workbench` (`/craft`) | Open a virtual crafting table. | `uxmessentials.workstation.workbench` |
{/* /generated */}

## Permissions

{/* generated:permissions */}
| Node | Default | Grants |
|---|---|---|
| `uxmessentials.antioch.use` | op | /antioch (alias /grenade): throw a primed TNT grenade (audit-logged). |
| `uxmessentials.beezooka.use` | op | /beezooka (alias /beecannon): launch an angry bee (audit-logged). |
| `uxmessentials.book.use` | op | /book: unlock a written book for editing. |
| `uxmessentials.break.use` | op | /break: instantly break the block you are looking at (audit-logged). |
| `uxmessentials.butcher.use` | op | /butcher [radius]: purge nearby mobs (audit-logged). |
| `uxmessentials.condense.use` | op | /condense (alias /compact) [all]: recipe-stack inventory items. |
| `uxmessentials.copyinv.use` | op | /copyinv \<player>: copy a player's inventory into yours. |
| `uxmessentials.disenchant.use` | op | /disenchant [all\|\<enchant>]: remove all or one enchantment from the held item. |
| `uxmessentials.disposal.use` | op | /disposal (alias /trash): open a throwaway GUI. |
| `uxmessentials.editsign.use` | op | /editsign: edit the sign you are looking at (respects build access). |
| `uxmessentials.enchant.use` | op | /enchant \<enchant> [level]: enchant the held item (level clamped at the boundary). |
| `uxmessentials.enderclear.use` | op | /enderclear (alias /clearec) [player]: clear an ender chest. |
| `uxmessentials.endercopy.use` | op | /endercopy \<player>: copy a player's ender chest into yours. |
| `uxmessentials.entitycount.use` | op | /entitycount [radius]: tally nearby entities by type for lag diagnosis. |
| `uxmessentials.fireball.use` | op | /fireball: launch a fireball (audit-logged). |
| `uxmessentials.firework.use` | op | /firework \<color\|clear\|power>: style or power the held firework rocket. |
| `uxmessentials.give.use` | op | /give \<player> \<item> [amount] (alias /i): give an item to a player; bulk gives are audited. |
| `uxmessentials.giveall.use` | op | /giveall \<item> [amount]: give an item to every online player; bulk gives are audited per recipient. |
| `uxmessentials.hat.use` | op | /hat: wear the held item as a helmet (itemworld-owned; playerstate defers it). |
| `uxmessentials.item.use` | op | /item \<item> [amount]: give an item to yourself. |
| `uxmessentials.itemamount.use` | op | /itemamount \<amount> (/amount): set the held stack amount, clamped to the give cap. |
| `uxmessentials.itemdamage.use` | op | /itemdamage \<damage> (/durability): set the held item's durability damage. |
| `uxmessentials.itemdb.use` | op | /itemdb [item]: look up an item's id / data. |
| `uxmessentials.itemflag.use` | op | /itemflag \<flag> \<on\|off>: toggle an item meta flag. |
| `uxmessentials.iteminfo.use` | op | /iteminfo: inspect the metadata of the item in your hand. |
| `uxmessentials.itemlore.use` | op | /itemlore \<set\|add\|clear> [text]: edit the held item's lore. |
| `uxmessentials.itemmodel.use` | op | /itemmodel \<id\|clear> (alias /custommodeldata): set or clear the held item's custom model data. |
| `uxmessentials.itemname.use` | op | /itemname \<name>: rename the held item. |
| `uxmessentials.itemworld.enchant.<enchantment>` | op | Apply one specific enchantment through /enchant, when per-enchantment gating is switched on. |
| `uxmessentials.itemworld.give.<item>` | op | Give one specific item through /give, when per-item gating is switched on. |
| `uxmessentials.itemworld.gui` | op | Open the itemworld utilities hub (/itemworld gui and on the /uxmess gui hub). |
| `uxmessentials.itemworld.itemedit` | op | /itemedit \<rename\|resetname\|lore\|enchant\|unenchant\|flag\|attribute\|durability\|repair\|unbreakable\|custommodeldata>: edit the held item's name, lore and meta (item-edit.enabled). |
| `uxmessentials.itemworld.itemedit.attribute` | everyone | /itemedit attribute add\|remove: edit attribute modifiers. Defaults on for an editor-node holder; the verb most worth withholding on a survival server. |
| `uxmessentials.itemworld.itemedit.durability` | everyone | /itemedit durability and repair. Defaults on for an editor-node holder; negate it to withhold just this verb. |
| `uxmessentials.itemworld.itemedit.enchant` | everyone | /itemedit enchant and unenchant. Defaults on for an editor-node holder; negate it to withhold just this verb. |
| `uxmessentials.itemworld.itemedit.flag` | everyone | /itemedit flag: toggle an item flag such as HIDE_ENCHANTS. Defaults on for an editor-node holder. |
| `uxmessentials.itemworld.itemedit.gui` | everyone | A bare /itemedit, which opens the click-driven editor. Defaults on for an editor-node holder; negate it to leave only the typed verbs. |
| `uxmessentials.itemworld.itemedit.lore` | everyone | /itemedit lore add\|set\|insert\|remove\|clear. Defaults on for an editor-node holder; negate it to withhold just this verb. |
| `uxmessentials.itemworld.itemedit.model` | everyone | /itemedit custommodeldata: set or clear the resource-pack model selector. Defaults on for an editor-node holder. |
| `uxmessentials.itemworld.itemedit.rename` | everyone | /itemedit rename and resetname. Defaults on for anyone who already holds the editor node; negate it to withhold just this verb. |
| `uxmessentials.itemworld.itemedit.unbreakable` | everyone | /itemedit unbreakable: mark the held item unbreakable. Defaults on for an editor-node holder. |
| `uxmessentials.itemworld.shulker` | op | Right-click a shulker box in the inventory to open its contents in place (shulkers.enabled). |
| `uxmessentials.itemworld.spawnmob.<mob>` | op | Spawn one specific mob through /spawnmob, when per-mob gating is switched on. |
| `uxmessentials.kill.use` | op | /kill [player\|entity]: kill a target (audit-logged). |
| `uxmessentials.killall.use` | op | /killall [type]: purge entities world-wide (audit-logged). |
| `uxmessentials.kittycannon.use` | op | /kittycannon: launch an exploding cat (audit-logged). |
| `uxmessentials.lightning.use` | op | /lightning (alias /smite) [player]: strike lightning (audit-logged). |
| `uxmessentials.module.itemworld` | op | Hot-reload / inspect the itemworld module and its sub-feature groups. |
| `uxmessentials.more.use` | op | /more: fill the held stack to max (itemworld-owned; playerstate defers it). |
| `uxmessentials.nuke.use` | op | /nuke [player]: rain lightning over an area (audit-logged). |
| `uxmessentials.potion.use` | op | /potion \<effect> [duration] [amplifier]: add a potion effect to the held potion. |
| `uxmessentials.powertool.toggle` | op | /powertooltoggle: enable/disable your powertool bindings. |
| `uxmessentials.powertool.use` | op | /powertool \<command> (alias /pt): bind a command to the held item. |
| `uxmessentials.recipe.use` | op | /recipe [item]: show an item's crafting recipe. |
| `uxmessentials.remove.use` | op | /remove \<type> [radius]: remove entities by type (audit-logged). |
| `uxmessentials.repair.itemworld` | op | /repair and /repairall in the itemworld surface (itemworld-owned; playerstate defers them). |
| `uxmessentials.showitem.use` | everyone | /showitem: broadcast the held item to chat for everyone online. |
| `uxmessentials.skull.use` | op | /skull [player]: get a player-head skull. |
| `uxmessentials.spawner.use` | op | /spawner \<type>: set a spawner's mob type (audit-logged). |
| `uxmessentials.spawnmob.use` | op | /spawnmob \<type> [amount]: spawn mobs (audit-logged). |
| `uxmessentials.time.alias` | op | /day / /night quick time aliases. |
| `uxmessentials.time.use` | op | /time \<set\|add> \<value>: per-world time. |
| `uxmessentials.tree.use` | op | /tree \<type>: generate a tree of the given type where you are looking (audit-logged). |
| `uxmessentials.unbreakable.use` | op | /unbreakable [true\|false]: toggle or set the held item's unbreakable flag. |
| `uxmessentials.unlimited.use` | op | /unlimited: toggle unlimited placement of held blocks. |
| `uxmessentials.weather.alias` | op | /sun / /rain / /thunder quick weather aliases. |
| `uxmessentials.weather.use` | op | /weather \<clear\|rain\|thunder> [duration]. |
| `uxmessentials.workstation.anvil` | op | /anvil: open a virtual anvil. |
| `uxmessentials.workstation.cartography` | op | /cartography: open a virtual cartography table. |
| `uxmessentials.workstation.enderchest` | op | /enderchest (alias /echest): open your ender chest. |
| `uxmessentials.workstation.furnace` | op | /furnace: open a virtual furnace. |
| `uxmessentials.workstation.grindstone` | op | /grindstone: open a virtual grindstone. |
| `uxmessentials.workstation.loom` | op | /loom: open a virtual loom. |
| `uxmessentials.workstation.others` | op | Open any virtual workstation on another player with the [player] target form. |
| `uxmessentials.workstation.smithingtable` | op | /smithingtable: open a virtual smithing table. |
| `uxmessentials.workstation.stonecutter` | op | /stonecutter: open a virtual stonecutter. |
| `uxmessentials.workstation.workbench` | op | /workbench (alias /craft): open a virtual crafting table. |
{/* /generated */}

## Settings

{/* generated:settings */}
| Key | Default | What it does |
|---|---|---|
| `give-cap` | `2304` | max items handed out by /give or /item at once |
| `more-cap` | `2304` | max stack fill for /more (defaults to give-cap) |
| `max-enchant-level` | `32767` | ceiling for /enchant |
| `spawnmob-cap` | `64` | max mobs per /spawnmob |
| `purge-max-radius` | `256` | max radius for /butcher, /killall, /remove |
| `give-audit-threshold` | `64` | audit-log /give above this amount |
| `item-edit.enabled` | `true` |  |
| `item-edit.allow-over-max-enchants` | `false` | let /itemedit enchant exceed an enchantment's vanilla max level |
| `shulkers.enabled` | `true` |  |
| `shulkers.require-sneak` | `true` | on: sneak-right-click opens (plain right-click still places the box); off: any right-click opens |
| `groups.item-utils.enabled` | `true` |  |
| `groups.workstations.enabled` | `true` |  |
| `groups.cleanup.enabled` | `true` |  |
| `groups.powertool.enabled` | `true` |  |
| `groups.mob-entity.enabled` | `true` |  |
| `groups.time-weather.enabled` | `true` |  |
| `groups.admin-fun.enabled` | `true` |  |
| `audit.give` | `true` |  |
| `audit.spawnmob` | `true` |  |
| `audit.spawner` | `true` |  |
| `audit.kill` | `true` |  |
| `audit.butcher` | `true` |  |
| `audit.killall` | `true` |  |
| `audit.remove` | `true` |  |
| `audit.lightning` | `true` |  |
| `audit.fireball` | `true` |  |
| `audit.kittycannon` | `true` |  |
| `audit.break` | `true` |  |
| `audit.tree` | `true` |  |
{/* /generated */}

## Placeholders

{/* generated:placeholders */}
| Placeholder | Renders |
|---|---|
| `%uxmessentials_itemworld_powertool%` | The commands bound to the item in the player's hand, comma separated. |
| `%uxmessentials_itemworld_powertool_bound%` | Whether the held item runs anything on click (yes/no). |
| `%uxmessentials_itemworld_powertool_count%` | How many commands the held item is bound to. |
| `%uxmessentials_itemworld_powertool_enabled%` | Whether the player currently lets their powertool bindings fire (yes/no). |
| `%uxmessentials_itemworld_unlimited%` | Whether the player is placing blocks without consuming them (yes/no). |
{/* /generated */}

## Notes

- **The seven groups are `item-utils`, `workstations`, `cleanup`, `powertool`, `mob-entity`, `time-weather` and
  `admin-fun`.** Disabling a group takes all of its commands offline at once.
- **`admin-fun` is off by default,** and belongs off on a survival or economy server: `/nuke`, `/kittycannon`
  and friends are spectacle, not gameplay.
- **Per-type nodes are how you hand `/give` to junior staff safely.** The wildcards default to allowed, so you
  deny by setting a specific node false, for instance `uxmessentials.itemworld.give.bedrock`. The same shape
  covers `/enchant` and `/spawnmob`.
- **The caps bound how much one command can do:** `give-cap`, `spawnmob-cap`, `max-enchant-level` and
  `purge-max-radius`. Large gives and every destructive or spectacle verb land in the audit log.
- **In-inventory shulker editing needs a sneak-right-click by default.** Set `shulkers.require-sneak = false`
  to open on any right-click.
- **Whole-world purges scan every entity.** Under Folia prefer a radius to a server-wide `/killall` or
  `/remove` on a large world.

Related: [Player State](playerstate.md), [Worlds](worlds.md), [Vaults](vaults.md)

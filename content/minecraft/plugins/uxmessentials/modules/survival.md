---
title: Survival
order: 9250
description: Tree-feller, veinminer, auto-pickup, auto-smelt, auto-sell, one-player sleep and the rest, each its own switch.
---

Survival is a set of opt-in gameplay conveniences most servers reach for a separate plugin (or three) to get:
tree-feller, veinminer, fast leaf decay, farmland protection, farm assist, an anvil unlocker, one-player sleep,
head drops, auto-pickup, auto-smelt, auto-sell and auto-tool. Each mechanic is independent, so you switch on
only what your server wants.

Module `survival` · disabled by default · `modules/survival/config.conf`

## Commands

{/* generated:commands */}
| Command | What it does | Permission |
|---|---|---|
| `/autopickup` | Toggle auto-pickup: mined drops go straight to your inventory. | `uxmessentials.survival.autopickup.toggle` |
| `/autosell` | Toggle auto-sell: priced drops are sold for coin as you mine. | `uxmessentials.survival.autosell.toggle` |
| `/autosmelt` | Toggle auto-smelt: ores are smelted as you mine them. | `uxmessentials.survival.autosmelt.toggle` |
| `/autotool` | Toggle auto-tool: the best tool swaps to hand as you mine. | `uxmessentials.survival.autotool.toggle` |
| `/farmprotect` | Toggle farmland protection: stop yourself trampling crops. | `uxmessentials.survival.farmprotect.toggle` |
| `/survival` | Open your survival panel: toggle tree-feller, veinminer, auto-pickup and the rest. | `uxmessentials.survival.gui` |
| `/treefeller` | Toggle tree-feller: fell a whole tree by breaking one log. | `uxmessentials.survival.treefeller.toggle` |
| `/veinminer` | Toggle veinminer: break a whole ore vein by mining one block. | `uxmessentials.survival.veinminer.toggle` |
{/* /generated */}

## Permissions

{/* generated:permissions */}
| Node | Default | Grants |
|---|---|---|
| `uxmessentials.module.survival` | op | Hot-reload / inspect the survival module (opt-in gameplay mechanics). |
| `uxmessentials.survival.autopickup` | everyone | Auto-pickup acts for you: mined drops go straight to your inventory. |
| `uxmessentials.survival.autopickup.toggle` | everyone | /autopickup: switch your personal auto-pickup on or off. |
| `uxmessentials.survival.autosell` | everyone | Auto-sell acts for you: priced drops are sold for coin as you mine. |
| `uxmessentials.survival.autosell.toggle` | everyone | /autosell: switch your personal auto-sell on or off. |
| `uxmessentials.survival.autosmelt` | everyone | Auto-smelt acts for you: ores are smelted as you mine them. |
| `uxmessentials.survival.autosmelt.toggle` | everyone | /autosmelt: switch your personal auto-smelt on or off. |
| `uxmessentials.survival.autotool` | everyone | Auto-tool acts for you: the best tool swaps to hand as you mine. |
| `uxmessentials.survival.autotool.toggle` | everyone | /autotool: switch your personal auto-tool on or off. |
| `uxmessentials.survival.farmassist` | everyone | Right-click a mature crop to harvest and replant it, spending one seed. |
| `uxmessentials.survival.farmprotect` | everyone | Farmland protection acts for you: you will not trample your crops. |
| `uxmessentials.survival.farmprotect.toggle` | everyone | /farmprotect: switch your personal farmland protection on or off. |
| `uxmessentials.survival.gui` | everyone | /survival: open your personal survival mechanics settings panel. |
| `uxmessentials.survival.treefeller` | everyone | Tree-feller acts for you: break one log to fell the whole trunk. |
| `uxmessentials.survival.treefeller.toggle` | everyone | /treefeller: switch your personal tree-feller on or off. |
| `uxmessentials.survival.veinminer` | everyone | Veinminer acts for you: break one block to mine the connected vein. |
| `uxmessentials.survival.veinminer.toggle` | everyone | /veinminer: switch your personal veinminer on or off. |
{/* /generated */}

## Settings

{/* generated:settings */}
| Key | Default | What it does |
|---|---|---|
| `tree-feller.enabled` | `true` |  |
| `tree-feller.require-axe` | `true` | Require an axe in hand to fell. With this off, breaking a log by any means fells the tree. |
| `tree-feller.durability-drain` | `true` | Drain the axe's durability as the tree comes down. |
| `tree-feller.durability-multiplier` | `1` | Durability points spent per extra log felled (the log you broke by hand is not counted). 1 mirrors mining each log normally; raise it to make felling wear the axe faster. |
| `tree-feller.max-blocks` | `64` | The most logs a single fell brings down, counting the one you broke. A safety cap so an enormous custom tree cannot lag the server. |
| `tree-feller.hunger-cost` | `0.0` | Food exhaustion added per extra log felled (0 disables the hunger cost). 0.0 by default so felling costs no more hunger than mining the logs by hand would. |
| `tree-feller.sneak-required` | `false` | Require the player to be sneaking to fell. This doubles as the "shift to fell" switch: with it on, a normal break chops a single log and a shift-break fells the tree; with it off (the default) any log break fells. |
| `tree-feller.replant-saplings` | `true` | Replant a matching sapling at the base after felling, when the block below is soil. Nothing is replanted for a species without a plantable sapling (the nether stems). |
| `veinminer.enabled` | `true` |  |
| `veinminer.blocks` | `[...]` | The blocks a vein-break triggers on. A break of any of these mines the connected run of the same block. Use the Bukkit material names; unknown names are skipped with a warning at startup. |
| `veinminer.max-blocks` | `64` | The most blocks a single vein-break mines, counting the one you broke. A safety cap for very large veins. |
| `veinminer.tool-durability` | `true` | Drain the mining tool's durability as the vein breaks. |
| `veinminer.hunger-cost` | `0.0` | Food exhaustion added per extra block mined (0 disables the hunger cost). |
| `veinminer.sneak-required` | `true` | Require the player to be sneaking to vein. The "shift to vein" switch: with it on (the default) a plain break takes a single ore and a shift-break takes the vein; with it off any break of a listed block veins it. |
| `leaf-decay.enabled` | `true` |  |
| `leaf-decay.radius` | `4` | How far from the broken log to look for leaves to decay, as the half-width of a cube. Larger values clear a bigger canopy but cost more to sweep; keep it modest. |
| `leaf-decay.delay-ticks` | `4` | Ticks to wait after the log breaks before decaying the leaves (20 ticks = 1 second). A small delay lets the cascade read as a gentle ripple rather than the whole canopy vanishing at once. |
| `farmprotect.enabled` | `true` |  |
| `farmassist.enabled` | `true` |  |
| `farmassist.crops` | `[...]` | The crops farm-assist acts on. Only crops that grow through a block age and replant from a single seed item are supported; unknown or seedless crops are skipped with a warning at startup. |
| `anvilunlocker.enabled` | `true` |  |
| `anvilunlocker.remove-level-limit` | `true` | Lift the "Too Expensive!" level ceiling (vanilla stops a survival combine once it costs 40 levels) so any combine, however many prior-work levels it has stacked up, produces a result. |
| `anvilunlocker.remove-cost-limit` | `false` | Also zero the level price of the combine, so the unlocked repair is free rather than charging the full (and possibly enormous) XP cost. Off by default, so an unlocked combine still costs its levels. |
| `oneplayersleep.enabled` | `true` |  |
| `oneplayersleep.required-percent` | `50` | The percentage of eligible players that must be sleeping to skip the night, rounded up to a whole player. Only used when required-count is 0. |
| `oneplayersleep.required-count` | `1` | A fixed number of sleeping players that skips the night. When above 0 this takes precedence over required-percent entirely; the default of 1 is the classic one-player sleep. Set to 0 to fall back to the percentage instead. |
| `headdrop.enabled` | `true` |  |
| `headdrop.player-head-on-pvp` | `true` | A player killed by another player drops their player head at full odds. PvE and environmental deaths never drop a player head. |
| `headdrop.mob-chance` | `0.0` | The default chance a slain mob drops its head, as a percentage. 0 means mobs drop no heads unless a per-mob override below raises their chance. |
| `autopickup.enabled` | `true` |  |
| `autopickup.transfer-xp` | `false` | Grant the block's dropped experience straight to the player instead of spawning XP orbs on the ground. Off by default, so mining still drops the usual orbs. |
| `autosmelt.enabled` | `true` |  |
| `autosmelt.smelt.RAW_IRON` | `IRON_INGOT` |  |
| `autosmelt.smelt.RAW_GOLD` | `GOLD_INGOT` |  |
| `autosmelt.smelt.RAW_COPPER` | `COPPER_INGOT` |  |
| `autosmelt.smelt.ANCIENT_DEBRIS` | `NETHERITE_SCRAP` |  |
| `autosell.enabled` | `true` |  |
| `autosell.notify.mode` | `"actionbar"` | Where the receipt goes: actionbar (the default; it overwrites itself and leaves the chat log clean), chat, or off for the silent behaviour. |
| `autosell.notify.interval-seconds` | `3` | Sales are pooled for this many seconds and reported as one line ("12x Iron Ingot, 3x Coal for $102"), so a vein does not print a receipt per block. Set 0 to report every sale on its own; raise it for chat mode. |
| `autosell.prices.COAL` | `2` |  |
| `autosell.prices.IRON_INGOT` | `8` |  |
| `autosell.prices.COPPER_INGOT` | `4` |  |
| `autosell.prices.GOLD_INGOT` | `12` |  |
| `autosell.prices.REDSTONE` | `1` |  |
| `autosell.prices.LAPIS_LAZULI` | `2` |  |
| `autosell.prices.DIAMOND` | `80` |  |
| `autosell.prices.EMERALD` | `60` |  |
| `autosell.prices.QUARTZ` | `3` |  |
| `autosell.prices.NETHERITE_SCRAP` | `120` |  |
| `autotool.enabled` | `true` |  |
{/* /generated */}

## Placeholders

{/* generated:placeholders */}
| Placeholder | Renders |
|---|---|
| `%uxmessentials_survival_autopickup%` | Whether the player has auto-pickup switched on (yes/no), the mechanic that sends drops straight to the inventory. |
| `%uxmessentials_survival_autopickup_enabled%` | Whether this server runs auto-pickup at all (yes/no), the mechanic that sends drops straight to the inventory. |
| `%uxmessentials_survival_autosell%` | Whether the player has auto-sell switched on (yes/no), the mechanic that sells what is mined. |
| `%uxmessentials_survival_autosell_enabled%` | Whether this server runs auto-sell at all (yes/no), the mechanic that sells what is mined. |
| `%uxmessentials_survival_autosmelt%` | Whether the player has auto-smelt switched on (yes/no), the mechanic that smelts what is mined. |
| `%uxmessentials_survival_autosmelt_enabled%` | Whether this server runs auto-smelt at all (yes/no), the mechanic that smelts what is mined. |
| `%uxmessentials_survival_autotool%` | Whether the player has auto-tool switched on (yes/no), the mechanic that swaps to the right tool. |
| `%uxmessentials_survival_autotool_enabled%` | Whether this server runs auto-tool at all (yes/no), the mechanic that swaps to the right tool. |
| `%uxmessentials_survival_farmprotect%` | Whether the player has farm protection switched on (yes/no), the mechanic that stops trampling crops. |
| `%uxmessentials_survival_farmprotect_enabled%` | Whether this server runs farm protection at all (yes/no), the mechanic that stops trampling crops. |
| `%uxmessentials_survival_treefeller%` | Whether the player has tree-feller switched on (yes/no), the mechanic that fells a whole tree in one break. |
| `%uxmessentials_survival_treefeller_enabled%` | Whether this server runs tree-feller at all (yes/no), the mechanic that fells a whole tree in one break. |
| `%uxmessentials_survival_veinminer%` | Whether the player has veinminer switched on (yes/no), the mechanic that follows an ore vein. |
| `%uxmessentials_survival_veinminer_enabled%` | Whether this server runs veinminer at all (yes/no), the mechanic that follows an ore vein. |
{/* /generated */}

## Notes

- **The module ships off because every mechanic changes how vanilla blocks break, drop or replant,** which is a
  decision about your server's gameplay rather than an essentials default.
- **A mechanic answers to up to three layers:** the config switch, the player's own `/command` toggle (on by
  default, remembered per player), and a permission node where one applies. All three must allow it.
- **Auto-sell runs after auto-smelt,** so it prices the smelted result, and it needs an economy provider and a
  price list before anything is sold.
- **One-player sleep is evaluated per world.** AFK players marked sleep-ignored by the presence module and
  spectators neither count toward the threshold nor block it.
- **`required-percent` only applies when `required-count` is 0,** so set one or the other rather than both.

Related: [Economy](economy.md), [Villagers](villagers.md), [Presence](presence.md)

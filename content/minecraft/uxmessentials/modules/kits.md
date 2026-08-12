---
title: Kits
order: 9100
description: Item bundles players claim on a cooldown, built and edited from a menu.
---

A kit is a named bundle of items, and optionally commands, that a player claims with `/kit`. The bare command
opens the browse menu, where a player sees the kits they may claim, what each one costs and how long is left on
its cooldown. Kits are built and edited in game, so adding one needs no restart.

Module `kits` · enabled by default · `modules/kits/config.conf`

## Commands

{/* generated:commands */}
| Command | What it does | Permission |
|---|---|---|
| `/kit` (`/k`, `/kits`) | Claim a kit and /kit list to list the kits you may claim. | `uxmessentials.kit.use` |
{/* /generated */}

## Permissions

{/* generated:permissions */}
| Node | Default | Grants |
|---|---|---|
| `uxmessentials.kit.<kit>` | everyone | Claim one kit. |
| `uxmessentials.kit.cooldown.<kit>.<seconds>` | everyone | The wait between claims of one kit, in seconds; the shortest tier held wins. |
| `uxmessentials.kit.cooldown.bypass` | op | Skip kit cooldowns and re-claim one-time kits. |
| `uxmessentials.kit.edit` | op | /kit create, /kit del, /kit editor to define, remove and edit kit contents. |
| `uxmessentials.kit.others` | op | /kit \<name> \<player> to give a kit to another player. |
| `uxmessentials.kit.preview` | everyone | /kit show \<name> to preview a kit's contents without claiming it. |
| `uxmessentials.kit.reset` | op | /kit reset \<player> [kit] to clear a player's claim/cooldown stamps. |
| `uxmessentials.kit.use` | everyone | /kit \<name> to claim a kit and /kit list to list the kits you may claim. |
| `uxmessentials.module.kits` | op | Hot-reload / inspect the kits module (kit definitions, cooldowns and claims). |
| `uxmessentials.oversizedstacks` | op | Receive kit items in stacks larger than the material normally allows. |
{/* /generated */}

## Settings

{/* generated:settings */}
| Key | Default | What it does |
|---|---|---|
| `list-display` | `"gui"` | how /kit list presents the list: "gui" opens the browse menu, "chat" prints the chat list |
| `showkit-display` | `"gui"` | how /kit show previews a kit: "gui" opens a read-only preview, "chat" prints the contents |
{/* /generated */}

## Placeholders

{/* generated:placeholders */}
| Placeholder | Renders |
|---|---|
| `%uxmessentials_kit_available_<kit>%` | Whether the player may claim one kit right now (yes/no). |
| `%uxmessentials_kit_claims_left_<kit>%` | How many claims of one kit the player has left; the infinity marker when it repeats. |
| `%uxmessentials_kit_cooldown_<kit>%` | The wait left before the player may claim one kit again. |
| `%uxmessentials_kit_cooldown_<kit>_formatted%` | The same remaining kit wait, under the spelling a config may prefer. |
| `%uxmessentials_kit_cost_<kit>%` | What one kit charges to claim, or free when it charges nothing. |
| `%uxmessentials_kit_has_<kit>%` | Whether the player holds one kit's permission (yes/no). |
| `%uxmessentials_kits_list%` | The ids of the kits the player may claim, comma separated. |
{/* /generated */}

## Notes

- **A claim runs three checks in order:** the per-kit permission gate, then the cooldown, then whether the items
  fit. Failing the first hides the kit, failing the second reports the time left, and failing the third refuses
  the claim without stamping a cooldown or charging anything.
- **`uxmessentials.kit.<id>` is how a kit is rank-locked.** Create a `vip` kit, grant `uxmessentials.kit.vip` to
  the group, and nobody else sees it. Combined with a price, that is a buy-to-unlock kit.
- **Cooldowns can differ per rank.** A kit carries a base cooldown, and
  `uxmessentials.cooldown.kit.<seconds>` grants a shorter one; `uxmessentials.cooldown.bypass.kit` removes it.
- **`/kit reset <player> [kit]` clears both the claim record and the cooldown**, which is also how a starter kit
  is re-issued for testing.
- **Claim and cooldown stamps live on the player, not in the database.** This is the one place uxmEssentials
  keeps state in PDC on purpose: a rolled-back world can reset a kit cooldown, and that is acceptable where
  losing a balance would not be.
- Kits imported with `/uxmess import essentialsx` land in the same catalogue as hand-built ones.

Related: [Economy](economy.md), [Menus](../menus/engine.md), [Ranks](../features/ranks.md)

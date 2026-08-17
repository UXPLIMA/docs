---
title: Commands
order: 260
description: One root command, two player subcommands, and the admin tree.
icon: terminal
---

One command: `/blacksmith`. Aliases `/bsmith`, `/uxmblacksmith`, `/smith`.

## Players

| Command | Does |
|---|---|
| `/blacksmith` | Open the main menu |
| `/blacksmith perks` | Open the perk tree |
| `/blacksmith mastery` | Open the mastery screen |

`perks` also answers to `perk`, `tree` and `talents`; `mastery` also answers to `masteries`.

Any unrecognised subcommand opens the main menu rather than printing an error, so a player who types
`/blacksmith help` gets somewhere useful.

All three are player-only. There are no permission nodes on them, if a player can run the command,
they can open the menu. Gating happens at the slot and category level.

## Admin

Everything below needs `uxmblacksmith.admin`.

| Command | Does |
|---|---|
| `/blacksmith reload` | Re-read every config file and clear the burn cache |
| `/blacksmith namedebug` | Print the held item's name and lore as the plugin sees them |
| `/blacksmith giveboost <player> <boost>` | Give a boost item; the player must be online |
| `/blacksmith editor <id>` | Open the requirement editor for the held item |
| `/blacksmith editor list` | List saved requirement profile ids |

`namedebug` is the tool for a `requiredName` that will not match. It prints exactly what the plugin
reads off the item, which is usually not what the config author expected.

### Progression

| Command | Does |
|---|---|
| `/blacksmith admin profile <player>` | Level, XP, perk points, craft counts |
| `/blacksmith admin xp add <player> <amount>` | Grant global XP |
| `/blacksmith admin level set <player> <level>` | Set the blacksmith level |
| `/blacksmith admin perkpoints <add\|set> <player> <amount>` | Adjust perk points |
| `/blacksmith admin mastery <addxp\|setlevel> <player> <mastery> <amount>` | Adjust one mastery |

`profile` also answers to `inspect`; `perkpoints` also answers to `points`.

Setting a level does not grant the perk points that reaching it would have. Grant those separately if
that is what you meant.

### The queue

| Command | Does |
|---|---|
| `/blacksmith admin queue list [player]` | Running crafts, optionally for one player |
| `/blacksmith admin queue gui` | The same as a menu |
| `/blacksmith admin queue cancel <player> <slot> [refund\|no-refund]` | Cancel a craft |
| `/blacksmith admin queue complete <player> <slot>` | Finish it now |

`cancel` refunds by default. `no-refund` takes the materials. `complete` also answers to
`forcecomplete`.

A refund the player cannot receive right now is queued and delivered on their next login.

### Analytics

| Command | Does |
|---|---|
| `/blacksmith admin analytics <today\|week\|month\|total>` | Counts and top trades |
| `/blacksmith admin analytics gui [period]` | The same as a menu; defaults to `today` |
| `/blacksmith admin history <player> [limit]` | One player's events, newest first |
| `/blacksmith admin export <csv\|json> [today]` | Write today's events to a file |

`history` defaults to 10 entries and caps at 50. Dates are UTC. See
[Analytics](features/analytics.md).

### Binary items

Saving a real `ItemStack` so a trade can use it as a reward or a requirement.

| Command | Does |
|---|---|
| `/blacksmith binary save <name>` | Save the held item under a key |
| `/blacksmith binary give <player> <item_name> [amount]` | Hand out a saved item |
| `/blacksmith binary list` | List saved keys |
| `/blacksmith binary info <item_name>` | Details of one saved item |
| `/blacksmith binary delete <item_name>` | Delete one |
| `/blacksmith binary clear confirm` | Delete all |
| `/blacksmith binary reload` | Re-read `binary_files/` from disk |

Reference a saved item in a trade with `material: SAVE` and
`customItemData: { customItemID: <name> }`, or the shorthand `material: 'SAVE:<name>'`.

Every subcommand needs the `SAVE` hook enabled: it is on by default. Without it the plugin answers
`binaryNotEnabled`.

<Callout type="danger" title="binary clear is not reversible">

`/blacksmith binary clear` without `confirm` only prints a prompt. With it, every saved item is
deleted and every trade referencing one stops loading. Back up `binary_files/` first: there is no
undo beyond that copy.

</Callout>

## Tab completion

Everything completes, including saved item names, boost ids, mastery keys and online players.
Non-admins see only the three player subcommands: the admin branches are not suggested and not
reachable.

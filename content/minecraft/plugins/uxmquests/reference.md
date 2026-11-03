---
title: "Reference"
order: 60
description: "Every command, permission, placeholder and file of uxmQuests."
---

Command names below are the defaults; `commands.conf` renames, aliases or disables any of them and needs a
restart. The admin command also answers to `/uxmquests`, `/uxmq` and `/questsadmin`; the player command to
`/quests`, `/quest` and `/q`. A player command refuses with a message when the permission is missing; an admin
command is hidden.

Contents: [Player commands](#player-commands) · [Admin commands](#admin-commands) · [Permissions](#permissions) ·
[Placeholders](#placeholders)

## Player commands

| Command | Permission | What it does |
|---|---|---|
| `/quests` | `uxmquests.use` | Opens the quest window, or writes the board in chat when there is no window. |
| `/quests start <group> <quest>` | `uxmquests.use` | Starts a quest a catalogue offered. |
| `/quests claim <group> <quest>` | `uxmquests.use` | Takes the reward of a finished quest, for a `manual` group. |
| `/quests reroll <group> <quest>` | `uxmquests.reroll` | Swaps a quest for another one from the same pool, within the rerolls the player has. |
| `/quests pass` | `uxmquests.use` | Lists every pass and where the player stands. |
| `/quests pass claim <pass>` | `uxmquests.use` | Takes every pass reward reached and not yet taken. |
| `/quests lang` | `uxmquests.language` | Shows the language the player reads and what else the server has. |
| `/quests lang set <language>` | `uxmquests.language` | Chooses the language. |
| `/quests lang reset` | `uxmquests.language` | Goes back to the client or default language. |

## Admin commands

| Command | Permission | What it does |
|---|---|---|
| `/uxmquests` | `uxmquests.admin.version` | Prints the usage line. |
| `/uxmquests version` | `uxmquests.admin.version` | Shows the running version. |
| `/uxmquests reload` | `uxmquests.admin.reload` | Reads the configuration, groups, passes, windows, messages and theme again. |
| `/uxmquests doctor` | `uxmquests.admin.doctor` | Checks the storage and the windows. |
| `/uxmquests types` | `uxmquests.admin.inspect` | Lists every task type a group file may write. |
| `/uxmquests problems` | `uxmquests.admin.inspect` | Lists what the last load of the files could not read. |
| `/uxmquests totals <player>` | `uxmquests.admin.inspect` | Shows how many quests a player has ever finished. |
| `/uxmquests pass top <pass> <season>` | `uxmquests.admin.inspect` | Shows the top of a pass; season `0` is all time. |
| `/uxmquests trigger <player> <trigger> <amount>` | `uxmquests.admin.trigger` | Advances `trigger` tasks named `<trigger>`. |
| `/uxmquests finish <player> <group> <quest>` | `uxmquests.admin.finish` | Finishes one quest of a player's board; the reward is paid the ordinary way. |
| `/uxmquests reset board <player> <group>` | `uxmquests.admin.reset` | Throws a board away so the next look draws a new one. |
| `/uxmquests reset totals <player>` | `uxmquests.admin.reset` | Forgets everything a player has finished. |
| `/uxmquests pass xp <player> <amount>` | `uxmquests.admin.pass` | Gives pass experience, with every booster applied. |
| `/uxmquests pass booster <player\|server> <multiplier> <time>` | `uxmquests.admin.pass` | Starts a timed booster. |
| `/uxmquests lang set-player <player> <language>` | `uxmquests.admin.language` | Sets the language another player reads. |
| `/uxmquests lang reset-player <player>` | `uxmquests.admin.language` | Clears another player's choice. |

A `<time>` is a span such as `30m`, `12h` or `7d`.

## Permissions

| Permission | Default | Meaning |
|---|---|---|
| `uxmquests.use` | everyone | Open the quest window, read boards, start and claim quests, read passes. |
| `uxmquests.language` | everyone | Choose the language the player reads. |
| `uxmquests.reroll` | everyone | Swap a quest. |
| `uxmquests.count.<n>` and `uxmquests.count.<group>.<n>` | op for the `count.*` node | A board of `n` quests, in every group or in one. The highest number wins. |
| `uxmquests.rerolls.<n>` | op for the `rerolls.*` node | `n` rerolls per board. The highest number wins. |
| `uxmquests.bypass` | op | Counts progress the anti-glitch rules would refuse. The node is `anti-glitch.bypass-permission`. |
| `uxmquests.admin.language` | op | Set or clear another player's language. |
| `uxmquests.admin.finish` | op | Finish a quest for a player. |
| `uxmquests.admin.reset` | op | Reset a board or the totals. |
| `uxmquests.admin.inspect` | op | `types`, `problems`, `totals`, `pass top`. |
| `uxmquests.admin.trigger` | op | `trigger`. |
| `uxmquests.admin.pass` | op | `pass xp` and `pass booster`. |
| `uxmquests.admin.doctor` | op | `doctor`. |
| `uxmquests.admin.version` | op | `version` and the bare admin command. |
| `uxmquests.admin.reload` | op | `reload`. |
| `uxmquests.admin.*` | op | Declared as "everything an operator does". It lists no child nodes, so grant the specific nodes above or use the permission plugin's own wildcard. |

The premium track of a pass is the permission named by `premium` in the pass file, for example
`uxmquests.pass.season.premium` in the shipped `season.conf`. The prefixes of the count and rerolls
permissions are `ranks.board-size` and `ranks.rerolls` in `config.conf`.

## Placeholders

The plugin registers no PlaceholderAPI placeholders. It reads them: a `placeholder` task compares any
PlaceholderAPI value, and PlaceholderAPI is applied to reward lines after the plugin's own tokens when it is
installed. The message tags `<quest>`, `<group>`, `<done>`, `<needed>`, `<percent>`, `<pass>`, `<tier>`,
`<tiers>`, `<xp>` and `<time>` are filled inside the sentences the plugin sends and are not PlaceholderAPI
placeholders.

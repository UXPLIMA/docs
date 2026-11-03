---
title: "Reference"
order: 20
description: "Every command, permission, placeholder and file of uxmSkills."
---

## Commands

Names and aliases are the defaults from `commands.conf`, which renames or disables each of them.

| Command | Aliases | Subcommands |
|---|---|---|
| `/skills` | `/skill`, `/sk` | `skill`, `levels`, `sources`, `abilities`, `stats`, `rate`, `top`, `total`, `average`, `rank`, `choose`, `buy`, `refund`, `forget` |
| `/uxmskills` | `/skillsadmin` | `reload`, `doctor`, `version`, `give`, `take`, `set`, `setlevel`, `addlevel`, `reset`, `resetall`, `wipe`, `boost`, `boostall`, `boostoff`, `backup`, `restore`, `inspect`, `diagnose`, `import`, `statgive`, `stattake`, `markitem`, `curves`, `markitem skill`, `markitem stat`, `recalculate`, `lang`, `lang set`, `lang reset`, `lang set-player`, `lang reset-player` |
| `/skillsedit` | `/seedit` | `open`, `files`, `new`, `delete`, `keys`, `show`, `set`, `read`, `write` |

## Permissions

| Permission | What it allows | Default |
|---|---|---|
| `uxmskills.use` | Open the windows and see your own skills | true |
| `uxmskills.top` | See the leaderboards | true |
| `uxmskills.language` | Choose the language you read this plugin in | true |
| `uxmskills.admin.language` | Set or clear the language another player reads | op |
| `uxmskills.skill.mining` | Earn in mining | true |
| `uxmskills.skill.excavation` | Earn in excavation | true |
| `uxmskills.skill.woodcutting` | Earn in woodcutting | true |
| `uxmskills.skill.farming` | Earn in farming | true |
| `uxmskills.skill.fishing` | Earn in fishing | true |
| `uxmskills.skill.combat` | Earn in combat | true |
| `uxmskills.skill.archery` | Earn in archery | true |
| `uxmskills.skill.defence` | Earn in defence | true |
| `uxmskills.skill.agility` | Earn in agility | true |
| `uxmskills.skill.alchemy` | Earn in alchemy | true |
| `uxmskills.skill.enchanting` | Earn in enchanting | true |
| `uxmskills.hidden` | Keep this account off every leaderboard | false |
| `uxmskills.admin.doctor` | Ask how this plugin is | op |
| `uxmskills.admin.version` | Say which build this is | op |
| `uxmskills.admin.reload` | Read every file again | op |
| `uxmskills.admin.experience` | Give, take, set and level experience | op |
| `uxmskills.admin.reset` | Clear a player's skills | op |
| `uxmskills.admin.wipe` | Clear every skill of every player | op |
| `uxmskills.admin.boost` | Boost a player, the server, or an item somebody holds | op |
| `uxmskills.admin.backup` | Write and read backups | op |
| `uxmskills.admin.inspect` | Read what a player holds | op |
| `uxmskills.admin.diagnose` | Write a report to disk | op |
| `uxmskills.admin.import` | Bring another plugin's numbers across | op |
| `uxmskills.admin.stats` | Add and remove stat modifiers | op |
| `uxmskills.admin.edit` | Edit a content file or a line of text from inside the game | op |
| `uxmskills.admin.*` | Every operator command | op |

## Placeholders

The plugin registers a PlaceholderAPI expansion when PlaceholderAPI is installed.

## Files

The plugin writes these into `plugins/uxmSkills/` on first start and reads them again on reload:

- `abilities/anvil-saving.conf`
- `abilities/arcane-saving.conf`
- `abilities/arrow-retrieval.conf`
- `abilities/auto-replant.conf`
- `abilities/bleed.conf`
- `abilities/critical-strike.conf`
- `abilities/daze.conf`
- `abilities/deep-lungs.conf`
- `abilities/double-drop.conf`
- `abilities/frugal-brewer.conf`
- `abilities/harvest-yield.conf`
- `abilities/herd-master.conf`
- `abilities/life-steal.conf`
- `abilities/pierce.conf`
- `abilities/potion-master.conf`
- `abilities/quick-bite.conf`
- `abilities/second-wind.conf`
- `abilities/shield-wall.conf`
- `abilities/soft-landing.conf`
- `abilities/soil-yield.conf`
- `abilities/swift.conf`
- `abilities/thick-skin.conf`
- `abilities/timber-yield.conf`
- `abilities/tool-saving.conf`
- `abilities/treasure-hunter.conf`
- `abilities/tree-feller.conf`
- `abilities/vein-miner.conf`
- `abilities/wide-dig.conf`
- `anti-farm.conf`
- `boosts.conf`
- `commands.conf`
- `config.conf`
- `economy.conf`
- `editor/new-abilities.conf`
- `editor/new-skills.conf`
- `editor/new-sources.conf`
- `editor/new-stats.conf`
- `menus/abilities.conf`
- `menus/antifarm.conf`
- `menus/editor-files.conf`
- `menus/editor-keys.conf`
- `menus/editor-languages.conf`
- `menus/editor-lines.conf`
- `menus/editor.conf`
- `menus/levels.conf`
- `menus/main.conf`
- `menus/skill.conf`
- `menus/sources.conf`
- `menus/stats.conf`
- `menus/top.conf`
- `messages/messages_en.conf`
- `messages/messages_tr.conf`
- `resources/mana.conf`
- `resources/stamina.conf`
- `skills/agility.conf`
- `skills/alchemy.conf`
- `skills/archery.conf`
- `skills/combat.conf`
- `skills/defence.conf`
- `skills/enchanting.conf`
- `skills/excavation.conf`
- `skills/farming.conf`
- `skills/fishing.conf`
- `skills/mining.conf`
- `skills/woodcutting.conf`
- `sources/agility.conf`
- `sources/alchemy.conf`
- `sources/archery.conf`
- `sources/combat.conf`
- `sources/defence.conf`
- `sources/enchanting.conf`
- `sources/excavation.conf`
- `sources/farming.conf`
- `sources/fishing.conf`
- `sources/mining.conf`
- `sources/woodcutting.conf`
- `stats/dexterity.conf`
- `stats/endurance.conf`
- `stats/fortune.conf`
- `stats/strength.conf`
- `stats/toughness.conf`
- `stats/wisdom.conf`
- `text-input.conf`
- `theme.conf`

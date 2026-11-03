---
title: "Guide"
order: 10
description: "How uxmSkills works, how to set it up and what each file holds."
---

uxmSkills gives players skills that level as they play: mining, excavation, woodcutting, farming, fishing, combat, archery, defence, agility, alchemy and enchanting ship as examples. Levels pay stat points, open abilities, and can drop loot, while an anti-farm layer keeps repeated work from paying in full forever.

## Install

1. Put the jar in `plugins/`.
2. Start the server once. The plugin writes its files into `plugins/uxmSkills/`.
3. Edit the files under `skills/`, `sources/`, `abilities/` and `stats/`, then run `/uxmskills reload`.

PlaceholderAPI, an economy and the claim plugins are optional.

## Files

| File | What it holds |
|---|---|
| `config.conf` | the shared curve, categories, storage, experience, claims, leaderboard, progress bar, notifications, level up, capstone, specialisation, death, activator, backups, points |
| `anti-farm.conf`, `economy.conf` | anti-farm and the economy, each under its own block name; a block an earlier release kept in `config.conf` moves here on the first start |
| `skills/<id>.conf` | one skill per file; the file name is the id |
| `sources/<id>.conf` | how a skill is earned: the price list |
| `abilities/<id>.conf` | one ability per file, tuned here, behaviour written in the plugin |
| `stats/<id>.conf` | the stats levels pay into |
| `resources/mana.conf`, `stamina.conf` | resources abilities spend |
| `boosts.conf` | permission multipliers and scheduled boosts |
| `editor/`, `menus/`, `commands.conf`, `messages/`, `text-input.conf`, `theme.conf` | editor templates, windows, commands, words, colours |

Every shipped file is commented in full; this page is the map.

A name, description or badge a player reads takes one of three shapes: a line of the messages files,
`name = "@content.skills.mining.display.name"`, drawn in each reader's language; a block per language,
`name { en = "Mining", tr = "Madencilik" }`; or one text everybody reads. The shipped files use the first, so a
new language is one more `messages_<lang>.conf` and not an edit to every skill; their words are under `content`
at the end of each messages file.

## A skill

| Key | Meaning |
|---|---|
| `enabled` | `false` takes the skill out of every menu, placeholder and board; stored experience stays |
| `display` | `name` and `description` per language, `icon`, `slot`, `colour` |
| `maximum-level` | the cap; lowering it never demotes anyone |
| `curve` | the cost of each level: a preset, a formula, or a list of values; left out, the shared `curve` in `config.conf` applies |
| `stat-points` | points of each stat one level pays, each an expression over `level` |
| `abilities` | the ability files this skill opens |
| `sources` | the file under `sources/` that says how it is earned |
| `earn-conditions` | conditions a player must pass to earn at all, such as `"money exp >= 250"` |
| `capstone`, `gates`, `loot` | what finishing pays once, what the skill holds back until earned, and extra drops |

A player also needs `uxmskills.skill.<id>` to earn in a skill; every shipped skill's node is on for everyone by default.

## Formulas

Every number can be a formula: `100 * level ^ 1.6`. The grammar is `+ - * / %`, `^` (right associative, so `2 ^ 3 ^ 2` is 512), brackets, unary minus, named variables and the functions `min`, `max`, `floor`, `ceil`, `round`, `abs`, `sqrt`, `pow`, `ln`, `log`, `exp` and `clamp`. There is no assignment, condition or loop. A broken formula is refused at load with the position of the character that broke it, and an answer that is not a finite number is refused when evaluated.

A curve written as a list repeats its last value: `[100, 200, 400]` costs 400 for every level after the third. A formula may use its own variables when the file declares them beside it.

## Sources

A source file has a `default` block merged into every entry and a `sources` block of entries, each with its own `experience` and `money`.

| Key | Meaning |
|---|---|
| `type` | `block-break`, `block-interact`, `block-collect`, `entity-kill`, `entity-damage`, `damage-taken`, `damage-blocked`, `fishing`, `brewing`, `enchanting`, `anvil-use`, `grindstone-use`, `item-consume`, `potion-splash`, `movement-pulse`, `ability-use`, `discovery` |
| `blocks` | the materials (or `entities`, `items`, `motions`, `abilities`, by type); `*` matches any run of characters and `@name` names a category |
| `states`, `after-states` | block states that must hold, now or one tick later |
| `tool` | the item filter: materials, excluded, categories, name, lore, custom model data, potion |

A misspelt type is refused at start. Categories are declared in `config.conf` under `categories`, or at the top of a source file.

## Abilities and points

An ability file sets `kind` (which built-in behaviour it is), `unlock-level`, `level-interval`, `maximum-level`, `base-value` and `value-per-level`, or `level-formula` and `value-formula` in their place, plus `conflicts`. A skill lists the abilities it opens, so moving an ability between skills is two lines in two skill files.

With `points.enabled`, abilities are bought with skill points: `/skills buy <ability>`, and `/skills refund <skill>` takes them back. `specialisation` decides how many skills a player may earn in at once (`default = 0` is no limit, nodes raise it), chosen with `/skills choose <skill>` and dropped with `/skills forget <skill>`.

## Experience and anti-farm

| `experience` key | Meaning |
|---|---|
| `global-multiplier`, `per-skill` | multipliers applied before anything else |
| `maximum-multiplier` | the most the whole stack of multipliers may reach |
| `cancelled-events-pay` | whether an event another plugin cancelled still pays; off by default |
| `movement-pulse-ms` | how often movement sources pay |

Permission multipliers and scheduled boosts are in `boosts.conf`.

`anti-farm` marks placed blocks in chunk data so they pay nothing when broken, and decays the pay for work repeated in one place, with two terms that pull back up; `ceiling` caps the whole stack. `enabled = false` turns every term off. `/skills rate` explains a player's current rate.

## Commands

Player commands need `uxmskills.use`, and the boards `uxmskills.top`.

| Command | What it does |
|---|---|
| `/skills` | open the skills window |
| `/skills skill <skill>`, `levels <skill>`, `sources <skill>`, `abilities <skill>` | one skill, what each level pays, how it is earned, what it opens |
| `/skills stats` | your stats |
| `/skills rate` | why your rate is what it is |
| `/skills top <board> [period]`, `total`, `average`, `rank <skill>` | the boards |
| `/skills choose <skill>`, `forget <skill>` | start or stop earning in a skill |
| `/skills buy <ability>`, `refund <skill>` | spend or take back skill points |

| Command | Permission | What it does |
|---|---|---|
| `/uxmskills give`, `take`, `set <player> <skill> <amount>` | `uxmskills.admin.experience` | change experience |
| `/uxmskills setlevel <player> <skill> <level>`, `addlevel <player> <skill> <levels>` | `uxmskills.admin.experience` | change levels |
| `/uxmskills reset <player> <skill>`, `resetall <player>` | `uxmskills.admin.reset` | clear one or every skill of a player |
| `/uxmskills wipe <confirm>` | `uxmskills.admin.wipe` | clear every skill of every player |
| `/uxmskills boost <player> <multiplier> <minutes>`, `boostall <multiplier> <minutes>`, `boostoff` | `uxmskills.admin.boost` | boosts |
| `/uxmskills markitem <multiplier>`, `markitemskill <skill> <multiplier>`, `markitemstat <stat> <amount>` | `uxmskills.admin.boost` | make the held item a multiplier or a stat modifier |
| `/uxmskills statgive <player> <stat> <name> <amount> <minutes>`, `stattake <player> <stat> <name>` | `uxmskills.admin.stats` | timed stat modifiers |
| `/uxmskills backup`, `restore <file>` | `uxmskills.admin.backup` | write or read a backup |
| `/uxmskills inspect <player>` | `uxmskills.admin.inspect` | what a player holds |
| `/uxmskills diagnose` | `uxmskills.admin.diagnose` | write a report to disk |
| `/uxmskills import <folder>` | `uxmskills.admin.import` | bring another plugin's numbers across |
| `/uxmskills curves`, `recalculate`, `reload` | `uxmskills.admin.reload` | print the curves, rewrite stored levels after a curve change, read every file again |
| `/uxmskills doctor`, `version` | `uxmskills.admin.doctor`, `.version` | the usual operator commands |
| `/skillsedit open`, `files`, `new`, `delete`, `keys`, `show`, `set`, `read`, `write` | `uxmskills.admin.edit` | edit content files and message lines in game |

The names and aliases come from `commands.conf`. The [reference](reference.md) lists the default aliases and every permission.

## Placeholders

Every value is plain text: counts are whole numbers and rates carry two decimals, without a percent sign. A key that exists but has no value answers the empty marker; an unknown key is left as written.

| Placeholder | Value |
|---|---|
| `%uxmskills_<skill>_level%`, `_experience%`, `_maximum%` | the level, the total experience, the cap |
| `%uxmskills_<skill>_into%`, `_next%`, `_cost%`, `_percent%`, `_bar%` | progress through the level |
| `%uxmskills_<skill>_multiplier%`, `_badge%`, `_position%` | the multiplier, the badge, the board place |
| `%uxmskills_total_level%`, `%uxmskills_rate%`, `%uxmskills_rate_reason%` | the total level, the anti-farm rate, and the term pulling it down most |
| `%uxmskills_ability_<id>%` | an ability's value for the player |
| `%uxmskills_stat_<id>%`, `%uxmskills_stat_<id>_points%` | a stat's value and its points |
| `%uxmskills_resource_<id>%`, `_maximum%`, `_percent%`, `_bar%` | mana, stamina and the like |
| `%uxmskills_trait_<key>%` | a trait's value |

## Troubleshooting

- **A reload is refused.** A formula, a source type or a key is wrong; the console names the file and the position.
- **A player earns nothing in a skill.** They lack `uxmskills.skill.<id>`, fail its `earn-conditions`, or have not chosen it under specialisation.
- **Pay falls during long sessions.** That is anti-farm; `/skills rate` says which term.
- **Levels look wrong after a curve change.** Run `/uxmskills recalculate`.

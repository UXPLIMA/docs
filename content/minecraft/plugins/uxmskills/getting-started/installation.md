---
title: Installation
order: 102
description: Requirements, optional plugins, the files, updating and checking the install.
icon: download
---

This manual describes uxmSkills after 0.1.0: German, Spanish, French, Portuguese and Russian arrive with the next
release, and 0.1.0 itself reads English and Turkish.

## Requirements

| Requirement | Detail |
|---|---|
| Java | 25 or newer |
| Server | Paper 26.2 or newer, or Folia |

uxmLib is shaded into the jar, so nothing else is required. The shipped files need no economy plugin.

## Optional plugins

| Plugin | Adds |
|---|---|
| PlaceholderAPI | The [placeholders](../placeholders/reference.md) |
| Vault, VaultUnlocked, Treasury, PlayerPoints, EcoBits | Costs and requirements in money; see [economy.conf](../config/economy-conf.md) |
| Claim plugins (nineteen) | What land is worth; see [Experience and boosts](../features/experience.md#land) |
| Oraxen, Nexo, ItemsAdder, CraftEngine | Custom blocks and items in sources |
| MythicMobs, LevelledMobs, RoseStacker | Custom and stacked mobs in sources |
| CustomCrops, CustomFishing, PyroFishingPro, InfiniteFishing | Custom crops and catches in sources |
| mcMMO | `%mcmmo_...%` values in conditions |
| Floodgate, Geyser | Forms for Bedrock players |
| uxmLang | One language choice for every UXM plugin |

## Installing

1. Drop `uxmSkills-<version>.jar` into `plugins/` and remove any older jar.
2. Start the server once. The plugin writes its files into `plugins/uxmSkills/`.
3. Edit the files under `skills/`, `sources/`, `abilities/` and `stats/`, then run `/uxmskills reload`.

## The files

```
plugins/uxmSkills/
├── config.conf          the shared curve, categories, storage, experience, claims, leaderboard, progress bar,
│                        notifications, level up, capstone, specialisation, death, activator, backups, points,
│                        editor, placeholders, menu
├── anti-farm.conf       the anti-farm layer
├── economy.conf         money backends for costs and requirements
├── boosts.conf          permission multipliers and the boost calendar
├── skills/              one skill per file: eleven ship
├── sources/             how each skill is earned
├── abilities/           one ability per file: twenty eight ship
├── stats/               six stats
├── resources/           mana.conf, stamina.conf
├── editor/              templates for new files made in game
├── menus/               thirteen windows
├── commands.conf        names, aliases, the skill shortcuts
├── text-input.conf      how typing is asked for
├── messages/            messages_<language>.conf: en, tr, de, es, fr, pt, ru
├── theme.conf           this plugin's colours on top of plugins/uxmTheme/theme.conf
├── backups/             data backups, and config copies before an update added keys
├── reports/             /uxmskills diagnose
└── data/skills.db       the SQLite database
```

Every shipped file is commented in full. A block an earlier release kept in `config.conf` moves into `anti-farm.conf`
or `economy.conf` on the first start.

## Checking the install

`/uxmskills doctor` checks storage, the skills, the currencies, the placeholders, the claim plugins and the windows.
`/uxmskills diagnose` writes a report to `reports/` to hand to support.

## Troubleshooting

- **A reload is refused.** A formula, a source type or a key is wrong; the console names the file and the position.
- **A player earns nothing in a skill.** They lack `uxmskills.skill.<id>`, fail its `earn-conditions`, or have not
  chosen it under specialisation.
- **Pay falls during long sessions.** That is anti-farm; `/skills rate` says which term.
- **Levels look wrong after a curve change.** Run `/uxmskills recalculate`.

Related: [Concepts](concepts.md), [config.conf](../config/config-conf.md)

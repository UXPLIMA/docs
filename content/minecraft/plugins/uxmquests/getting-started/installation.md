---
title: Installation
order: 202
description: Requirements, the database drivers, the files, updating and checking the install.
icon: download
---

This manual describes uxmQuests after 0.1.0: German, Spanish, French, Portuguese and Russian arrive with the next
release, and 0.1.0 itself reads English and Turkish.

## Requirements

| Requirement | Detail |
|---|---|
| Java | 25 or newer |
| Server | Paper 26.2 or newer, or Folia |

uxmLib is shaded into the jar, so nothing else is required.

## Optional plugins

| Plugin | Adds |
|---|---|
| PlaceholderAPI | `%placeholders%` in reward lines and in `placeholder` tasks |
| Vault | `[give-money]` and `[take-money]` in rewards |
| CoreProtect | The third [anti-glitch](../features/anti-glitch.md) level |
| mcMMO | A skill level written as `%mcmmo_level_<skill>%` in a line |
| Floodgate, Geyser | Bedrock players get forms where a window would be a chest |
| uxmShop | The `shop-buy` and `shop-sell` task types |
| uxmLang | One language choice for every UXM plugin |

## Installing

1. Drop `uxmQuests-<version>.jar` into `plugins/` and remove any older jar.
2. Start the server. The first start fetches the MariaDB and PostgreSQL drivers from Maven Central into the server's
   `libraries/` folder, once, so the server needs to reach Maven Central even when it only uses SQLite.
3. Edit `groups/daily.conf`: the shipped quests pay nothing until you write their `rewards`.
4. Run `/uxmquests reload`.

## The files

```
plugins/uxmQuests/
├── config.conf          language, storage, rewards, battlepass, anti-glitch, feedback, objectives, ranks, menu
├── commands.conf        the name, aliases and switch of every command
├── groups/              one group of quests per file: daily.conf, catalogue.conf
├── passes/              one battle pass per file: season.conf
├── menus/               quests.conf, board.conf, passes.conf, pass.conf
├── messages/            messages_<language>.conf: en, tr, de, es, fr, pt, ru
├── languages.conf       the language each player chose, written by the plugin
├── theme.conf           this plugin's colours on top of plugins/uxmTheme/theme.conf
├── backups/             config.conf as it was before an update added keys
└── data/quests.db       the SQLite database
```

The plugin writes a file only when it is missing and never overwrites one that exists.

## Updating

Replace the jar and restart. When a release adds a key to `config.conf`, the start adds it with its comment and keeps
every value you wrote; the file as it was goes into `backups/`. A database file an older version kept in the plugin
folder moves into `data/` by itself. The tables are upgraded on start; nothing is done by hand.

## Checking the install

`/uxmquests doctor` checks the storage (the backend answers) and the windows (how many are registered), one line each.
`/uxmquests problems` lists what the last load of the files could not read, and `/uxmquests types` every task type a
group file may write.

## Troubleshooting

- **A reload says the groups were kept.** A group file does not parse. The running groups stay as they were, and the
  console and `/uxmquests problems` name the file.
- **A quest pays only experience.** The shipped quests pay vanilla experience. Write your own lines; see [Rewards](../features/rewards.md).
- **A walking or jumping task never moves.** `objectives.sampling` is off.
- **The premium track is empty for a player.** They lack the node named by the pass file's `premium`.
- **A renamed command is unchanged.** `commands.conf` is read at start only.

Related: [Concepts](concepts.md), [config.conf](../config/config-conf.md)

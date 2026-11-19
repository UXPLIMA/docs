---
title: Installation
order: 202
description: Requirements, the three jars, optional plugins, the first start and the files that appear.
icon: download
---

uxmBedrock is three jars with uxmLib inside them. This manual describes uxmBedrock 0.1.0.

## Requirements

| Requirement | Detail |
|---|---|
| Java | 25 or newer on the Paper server; 21 or newer on the proxy and for Geyser Standalone |
| Server | Paper or Folia 26.2, 26.3 or 26.4, or a fork of Paper. Another version stops the plugin at start with a console line |
| Floodgate | Required on every Paper server: it tells which players are on Bedrock |
| Geyser | On the same server (Geyser-Spigot), on the Velocity proxy, or Geyser Standalone |
| Database | None for one server: SQLite is built in. MySQL, MariaDB or PostgreSQL for a network |
| Java's language | Not Turkish or Azerbaijani. There a small i upper-cases to a dotted capital I, and plugins that look names up in capitals break (MythicHUD draws no HUD). The console says so at start; start Java with `-Duser.language=en -Duser.country=US` |

The database drivers are downloaded on the first start, so the first start needs internet access. The first pack
build also fetches the Java client's font once from Mojang's download service and keeps it in `cache/`.

## The jars

| Jar | Goes in | For |
|---|---|---|
| `uxmBedrock-<version>.jar` | every Paper server's `plugins/` | Everything in this manual |
| `uxmBedrock-Geyser-<version>.jar` | Geyser's `extensions/` folder, wherever Geyser runs | Serving the pack per player, registering items, blocks and heads |
| `uxmBedrock-Velocity-<version>.jar` | the proxy's `plugins/` | Geyser on the proxy: one pack made from every server, and proxy command completion. See [A network](../features/network.md) |

## Optional plugins

| Plugin | Adds |
|---|---|
| PlaceholderAPI | The [placeholders](../placeholders/reference.md) |
| Nexo, ItemsAdder, Oraxen, CraftEngine | Their built packs are found and converted |
| BetterHud, MythicHUD, HappyHUD, CustomNameplates, CustomFishing, LuxDialogues | Their packs are converted and their HUD is drawn on Bedrock |
| uxmLang | One language choice for every UXM plugin |

## Installing

1. Drop `uxmBedrock-<version>.jar` into `plugins/` beside Floodgate.
2. Put `uxmBedrock-Geyser-<version>.jar` into Geyser's `extensions/` folder (`plugins/Geyser-Spigot/extensions/`
   when Geyser runs on the server).
3. Start the server. The plugin writes its folder and builds the first pack.
4. Run `/uxmbedrock status` to see what runs where, and `/uxmbedrock pack report` to see what the build found.
5. Restart Geyser once after the first build, so it loads the custom items, blocks and heads.

## The files

```
plugins/uxmBedrock/
├── config.conf      language, server name, storage, network, modules, player choices, preview page
├── text.conf        exact colours and the glyph pages
├── menus.conf       which menus of other plugins open as forms
├── pack.conf        how the Bedrock pack is built
├── gui.conf         menu pictures and the changes to Bedrock's screens
├── items.conf       the vanilla item under an item model the packs do not say
├── commands.conf    the name, aliases and switch of every command
├── messages/        one file per language
├── backups/         copies of a file from before a release added keys to it
├── reports/         menus.json, written by /uxmbedrock menus report
├── data/            bedrock.db, the SQLite database
├── cache/           the Java client's font, fetched skins and past builds
└── output/          the build Geyser serves: packs, mappings, choices, heads, report.txt
../uxmTheme/theme.conf   the colours every UXM plugin shares
```

## Updating

Replace the three jars and restart. At start, every file the plugin ships (config.conf, text.conf, menus.conf,
pack.conf, gui.conf, items.conf) gets the keys the new release adds; your values stay as they
are, the file as it was is kept in `backups/`, and the console names each file it brought up to date and any key it
does not know, such as a typo. The Geyser extension does the same with its own `config.conf`, keeping the file as it
was in its own `backups/`. New packs reach players from their next join.

A release that changes how the pack names items builds the pack again at the first start. Geyser on that server
read the item names of the build before, so the console asks for a restart once the new build is done; restart
then, or set `new-items-restart` to `when-empty` in [pack.conf](../config/pack-conf.md).

## Checking the install

| Command | Says |
|---|---|
| `/uxmbedrock status` | Floodgate and Geyser versions, where the pack goes, the network, the pack's size and build time, every switch |
| `/uxmbedrock doctor` | One line per thing that can be wrong: Floodgate, TCPShield, the Bedrock players online, the pack |
| `/uxmbedrock pack report` | What the last build converted and what it left out |

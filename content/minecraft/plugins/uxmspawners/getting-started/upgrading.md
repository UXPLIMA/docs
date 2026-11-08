---
title: Upgrading to 5.0.0
order: 16
description: Moving a 4.2.0 server to uxmSpawners 5.0.0, what the update does on its own, where every file went, and what behaves differently afterwards.
icon: circle-arrow-up
---

5.0.0 is a rewrite. Your spawners, their levels, storage, friends and boosts carry over, and so do
your settings, but most files have new names and places, and several things behave differently.
The update moves everything on the first start; this page says what it does and what is left to you.

## Before you start

| Check | Why |
|---|---|
| The server runs uxmSpawners 4.2.0 | The update reads 4.2.0 files. Update an older 4.x to 4.2.0 first. A 3.x `ancSpawners` folder is not read at all |
| Paper 1.21.11 or newer, Java 21 | 5.0.0 does not start on anything older |
| Vault and an economy plugin | Still required. NBTAPI and a hologram plugin are no longer needed |
| PacketEvents installed | Holograms need it now. See [Holograms](../features/holograms.md) |
| The server can reach the internet on its first start | The plugin downloads its libraries and checks its license |
| A copy of `plugins/uxmSpawners/` and the worlds | The plugin backs itself up too, but a copy of your own costs nothing |

<Callout type="warning" title="Customised texts of the shipped languages are not carried over">

For English, Turkish, German, French and Polish, 5.0.0 starts from its own new messages and menus.
Your 4.2.0 `lang_<code>.yml` is kept in `backups/v5-<date>/configs/lang/`. Note what you changed
before updating, and put it into the new files afterwards. A language you added yourself, such as
`lang_es.yml`, is carried over with its texts.

</Callout>

## The update

1. Stop the server, replace the jar and start it.
2. The plugin copies your files and `database.db` to `backups/v5-<date>/`, then moves every setting
   to its new place. The console names each step.
3. It starts on the new files and the database is brought to the new tables. The 4.2.0 tables are
   copied, not changed.
4. The console says `The update to 5.0.0 is done.`

If a step fails, or the plugin cannot start on the new files, everything is put back as it was from
the backup, and the console says why. The server can go on with the 4.2.0 jar, or start 5.0.0 again
once the problem is fixed. A crash in the middle is handled the same way on the next start.

## Where everything went

| 4.2.0 | 5.0.0 |
|---|---|
| `config.yml`: `spawners` | One file per type in `spawners/`, such as `spawners/diamond.yml`. See [Spawner Types](../config/spawner-types.md) |
| `config.yml`: `itemPrices` | `prices.yml`, under `default-prices` and `price-provider` |
| `config.yml`: `disabled-worlds` | `world-management` in `config.yml`, as a `BLACKLIST` |
| `config.yml`: `auto-save` | Gone. Production is written every `database.flush-interval-seconds`, 5 |
| `config.yml`: `use-modern-autokill` | `use-modern-auto-kill` |
| `config.yml`: `nerf-spawner-mobs`, `no-equipment`, `open-gui-on-click`, `disable-xp-drops`, `disable-vanilla-spawners-spawning`, `protect-spawners-from-explosions` | `general-settings.yml` |
| `modules/settings.yml`, `other.yml`, `storage_limits.yml`, `friends.yml`, `autokill.yml`, `multipliers.yml` | `general-settings.yml`, as one file |
| `modules/boosts.yml` | `boosts.yml` |
| `modules/hologram.yml` | `hologram.yml`, with the FancyHolograms look carried over where it fits |
| The hologram lines in the language file | `content` in `hologram.yml`, from the language in use |
| `lang/lang_<code>.yml` | `languages/<code>/messages.yml` and four files in `languages/<code>/menus/` |
| `logs/errors.txt` | A file a day in `logs/`. The old file is left where it is |

Renamed keys, all carried with their values:

| 4.2.0 | 5.0.0 |
|---|---|
| `spawnerInfo`, `sell_button` | `spawner-info`, `sell-button` |
| `{entity_name}`, `{entity_name_lang}` in names and lore | `<entity_name>`, `<entity_name_lang>` |
| `{uses}` in the pickaxe name | `<uses>` |
| `storage-limit.xpLimit`, `xpPermissions` | `xp-limit`, `xp-permissions` |
| `auto-kill.loottable-only-autokill` | `loot-table-only-auto-kill` |
| `vanilla-spawner-support.support-on-break`, `drop-vanilla-on-break` | `on-break`: `UXMSPAWNERS`, `VANILLA` or `NONE`, set to what your server did |
| `spawner-drop-chance.default` | Also copied to the new `vanilla-default`, since 4.2.0 used it for natural spawners too |
| Loot tables: `itemDrops`, `xpDrops`, `xpDropChance` | `item-drops`, `xp-drops`, `xp-drop-chance`. The old names are still read |

Every new setting is added with its default. `database.type` is set to `SQLITE`, since 4.2.0 only
ever used `database.db`, whatever the file said.

## The items players carry

Spawner items, boost items and spawner pickaxes from 4.2.0 keep working while
`item-signing.accept-old-items` is on, which is the default. The ones in a player's inventory and
ender chest are converted to the 5.0.0 format when they join, so they stack with new ones. A boost
item does what its entry in `boosts.yml` says now. Once the old items have gone around, turn
`accept-old-items` off: anyone in creative mode can make an item in the old format. See
[Item Signing](../features/item-signing.md).

## What behaves differently

| Area | 4.2.0 | 5.0.0 |
|---|---|---|
| Selling, items without a price | Sold for nothing and gone | Stay in the storage |
| Selling, renamed items | Sold at their material's price | Not sold, with `price-items-by: ITEM`. Set it to `MATERIAL` in `prices.yml` for the 4.2.0 way. Item spawners such as `diamond` need it to sell anything |
| Breaking the last level with items stored | The storage was deleted | Refused until it is empty. `break-with-storage` changes it |
| A spawner blown up, or its block removed | The storage was lost | Sold for the owner. `lost-spawner-storage` changes it |
| `break-management.mode` | The names did not match what they did | Each mode does what its name says. Check yours against [Breaking Rules](../features/breaking.md#requiring-a-tool) |
| `block: false` without the right tool | The whole spawner was lost | One level is lost. `lost-levels: ALL` for the 4.2.0 way |
| The spawner pickaxe | Lost a use on every break | Only when the mode asked for it. `pickaxe-uses: ALWAYS` for the 4.2.0 way |
| Natural spawners with `on-break: VANILLA` | Always dropped | Rolled against `vanilla-default` |
| Loot tables | Applied to every spawner mob | With `loot-table-only-auto-kill: true`, the 4.2.0 default, only to mobs Auto Kill kills |
| Auto Kill `enabled` and `force` | `enabled: false` changed little, `force` only new spawners | `enabled: false` turns it off everywhere, `force` keeps it on for every spawner |
| Friends | Every owner could add them | Owners need `uxmspawners.friends`, op only until you grant it |
| Friends and the hologram | Friends could toggle it | Owner only |
| Boosts | Anyone could apply one to any spawner | Owner and friends only. `SELL_MULTIPLIER` and `enabled: false` now work |
| Spawn eggs on natural spawners | Anyone | Players with `uxmspawners.spawnegg` |
| Mobs from spawners | No cap | At most `max-living-mobs`, 32, alive per spawner. Players cannot ride, tame or equip them |
| Storage limits | Updated when the owner opened the menu | Also on join, reload and LuckPerms changes. New spawners store XP at once |
| A type without `range` | Range 0, never ran | Range 16 |
| Holograms | Drawn by FancyHolograms, DecentHolograms or CMI | Drawn by uxmSpawners, with PacketEvents |
| Saving | Every `auto-save.interval`, 300 seconds | Player actions at once, production every 5 seconds |
| Menus | The 4.2.0 layout | A new layout, with a stop button, a Settings menu and pages of friends |

`protect-spawners-from-explosions` keeps the value your 4.2.0 `config.yml` had, `false` unless you
changed it. New servers start with `true`; consider turning it on.

## After the update

1. Read the console from the first start for warnings.
2. Put your text changes back into `languages/<code>/`, from the copy in `backups/v5-<date>/configs/lang/`.
3. Decide on `price-items-by` in `prices.yml`.
4. Grant `uxmspawners.friends`, and `uxmspawners.spawnegg` if players used eggs. See [Permissions](../permissions.md).
5. Check `break-management`, `break-with-storage` and `lost-spawner-storage` in `general-settings.yml`.
6. Open a few spawners and check their level, storage and friends.

To move to MySQL, set it up after the update has finished. The plugin does not copy spawners from
SQLite to MySQL: a server switched over starts with none.

## Going back

Once 5.0.0 has started, going back to 4.2.0 means stopping the server and copying the files and
`database.db` from `backups/v5-<date>/` back into the plugin folder. Everything that happened since
is lost, and items made or converted by 5.0.0 do not work in 4.2.0.

Related: [Installation](installation.md), [Configuration](../config/index.md), [Troubleshooting](../troubleshooting.md)

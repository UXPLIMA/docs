---
title: Commands
order: 20
icon: terminal
---

uxmFarmer has two command roots: `/farmer` for players and `/uxmfarmer` for staff. Both accept
Turkish names as well — `/çiftçi` is the primary name of the player command and `/farmer` its
alias, so both spellings work everywhere.

## Player commands

| Command | Aliases | Permission |
|---|---|---|
| `/farmer` | `/çiftçi`, `/farmer help` | — |
| `/farmer buy` | `/çiftçi satınal` | — |
| `/farmer menu` | `/çiftçi menü` | `uxmfarmer.command.menu` (default: everyone) |
| `/farmer teleport` | `/farmer tp`, `/çiftçi getir` | `uxmfarmer.command.teleport` |
| `/farmer disappear` | `/farmer getrid`, `/çiftçi yoket` | `uxmfarmer.command.disappear` |
| `/farmer sellall` | `/çiftçi hepsinisat` | `uxmfarmer.command.sellall` |
| `/farmer skin <name>` | `/çiftçi skin` | `uxmfarmer.changeskin` |
| `/farmer return` | `/çiftçi iade` | `uxmfarmer.returnfarmer` |

Every one of these checks the world list and then checks that the player has access to the region
they are standing in. Where they may stand is decided by
`general-settings.yml → where-players-can-reach-farmer`.

### What the bare command does

`/farmer` on its own follows two settings:

- `farmer-command-opens-gui: true` opens the farmer menu directly.
- `farmer-command-opens-buy-menu: true` opens the purchase menu when the player has no farmer.

With both off it prints the help list.

### buy, return, disappear

`buy` charges `farmer-price` through your money hook and places a farmer. Set
`disable-buy-command: true` if farmers should only ever come from eggs.

`return` sells the farmer back for `return-percent` of its price — 75% by default — and deletes it.

`disappear` hides the farmer's body without deleting it. The data, the storage and the members all
survive; `/farmer teleport` brings it back to the player.

### sellall

Sells everything sellable in the storage at once. With
`sell-all-requires-confirmation: true` the player gets a confirmation menu first, which is worth
keeping on — a full storage is a lot of money to lose to a misclick.

## Admin commands

`/uxmfarmer`, alias `/uxmfarmer info`. Everything below needs its own permission; see
[Permissions](permissions.md).

### Selecting a farmer

Several admin commands act on a *selected* farmer rather than one you name:

```
/uxmfarmer select              # selects the farmer you are looking at
/uxmfarmer select <uuid>       # selects by UUID, works from console
```

Aliases `sel` and `seç`. The selection is remembered per sender until you select something else.

### Managing farmers

| Command | What it does |
|---|---|
| `/uxmfarmer givefarmer <player>` | Places a farmer for a player who has a region |
| `/uxmfarmer delete <player>` | Deletes that player's farmer |
| `/uxmfarmer deleteall <player>` | Deletes every farmer that player owns — run it twice, the first run only asks |
| `/uxmfarmer deleteuuid <uuid>` | Deletes one farmer by its UUID |
| `/uxmfarmer setowner <player>` | Transfers the selected farmer to another player |
| `/uxmfarmer uuidof <player>` | Prints the UUIDs of that player's farmers |
| `/uxmfarmer nearfarmers [range]` | Lists farmers within range, each clickable to teleport |
| `/uxmfarmer teleport` | Teleports you to the selected farmer |

### Settings and levels

| Command | What it does |
|---|---|
| `/uxmfarmer editsetting <setting> <true\|false>` | Changes one setting on the selected farmer |
| `/uxmfarmer bulkedit <setting> <true\|false>` | Changes that setting on **every farmer on the server** |
| `/uxmfarmer setproductlevel <product> <level>` | Sets one product's level on the selected farmer |
| `/uxmfarmer fixstatus` | Re-enables every farmer whose region is no longer ruined |

Setting names are the values of `FarmerSetting`: `AUTO_HARVEST`, `AUTO_SELL`, `AUTO_KILL`,
`AUTO_KILL_SPAWNER`, `XP_COLLECTOR`, `XP_SELLABLE`, `GLOWING`, `NO_VISUAL`,
`COLLECT_PLAYER_DROPS`, `COLLECT_NATURAL_DROPS`, `BABY`, `ENABLED`. Tab completion lists them.

<Callout type="warning" title="bulkedit touches every farmer">

`bulkedit` asks for confirmation once and the confirmation expires after ten seconds, then applies
to every farmer in the database. There is no undo beyond running it back the other way, which will
also overwrite the choices players made for themselves.

</Callout>

### Giving items

```
/uxmfarmer give FARMER_EGG <player> <amount>
/uxmfarmer give LEVEL_CHANGER <player> <amount> <product> <new level>
/uxmfarmer give LEVEL_ADDER <player> <amount> <product> <levels to add>
```

The three items are defined in `items.yml`. `LEVEL_CHANGER` sets a product to an exact level;
`LEVEL_ADDER` adds levels to whatever the product is at. Both are used by right-clicking near a
farmer.

`give-even-if-has-farmer: false` in `items.yml` stops eggs from reaching players who already own a
farmer.

### Diagnostics

| Command | What it does |
|---|---|
| `/uxmfarmer statistics` | Statistics for the selected farmer |
| `/uxmfarmer checkage.target` | Reports the growth stage of the crop you are looking at |
| `/uxmfarmer checkage.chunk [radius]` | Reports growth stages across the chunk |
| `/uxmfarmer productiondebug [product]` | Explains what the production module is counting |
| `/uxmfarmer checkperm <player> <permission>` | Shows a permission's state both in cache and in LuckPerms |
| `/uxmfarmer clearpermcache` | Drops the permission cache |
| `/uxmfarmer clearlogs` | Deletes the selected farmer's logs |
| `/uxmfarmer menu` | Opens the admin menu |
| `/uxmfarmer bypass` | Toggles bypassing region and ownership checks |
| `/uxmfarmer save` | Writes everything to the database now |
| `/uxmfarmer reload` | Reloads configuration and languages |
| `/uxmfarmer help` | Lists the admin commands |

`checkperm` is the command to reach for when a player insists a module is off for no reason: it
tells you whether the permission is genuinely missing or whether the cache is stale, and
`clearpermcache` fixes the second case.

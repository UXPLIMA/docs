---
title: "Reference"
order: 60
description: "Every command, permission, placeholder and file of uxmShop."
---

Command names below are the defaults; `commands.conf` renames them. A command marked "Player" can only be
run by a player. Permissions come from `paper-plugin.yml` and the shipped configuration.

## Commands

### Catalogue

| Command | Permission | What it does |
|---|---|---|
| `/shop` (alias `/shops`) | `uxmshop.use` | Opens the hub. Player. |
| `/shop open <section>` | `uxmshop.use` and `uxmshop.section.<section>` | Opens one section. Player. |
| `/shop lang` | none | Shows the language you read and the ones the server has. Player. |
| `/shop lang set <language>` | none | Chooses your language. Player. |
| `/shop lang reset` | none | Goes back to the client or default language. Player. |
| `/shop lang set-player <player> <language>` | `uxmshop.admin.language` | Chooses another player's language. |
| `/shop lang reset-player <player>` | `uxmshop.admin.language` | Clears another player's choice. |
| `/sell` | none | The same as `/sell hand`. Player. |
| `/sell hand` | `uxmshop.sell.hand` | Sells the stack in your hand. Player. |
| `/sell all` | `uxmshop.sell.all` | Sells everything in your inventory the catalogue buys, up to `shop.sell-all-limit` items. Player. |
| `/sell window` | `uxmshop.sell.window` | Opens a window to drop items into. Player. |
| `/value` (alias `/worth`) | `uxmshop.value` | What the item in your hand is worth, in both directions. Player. |

### Player shops

| Command | Permission | What it does |
|---|---|---|
| `/myshop` (alias `/ms`) | `uxmshop.myshop` | Opens your shop, or says you have none. Player. |
| `/myshop open` | `uxmshop.myshop` | Opens a shop behind the container you are looking at. Player. |
| `/myshop close` | none | Closes your shop and gives the stock back. Player. |
| `/myshop add <buy> <sell> <match>` | `uxmshop.myshop` | Adds the item in your hand as a line. Player. |
| `/myshop stock <offer> <amount>` | `uxmshop.myshop` | Moves items from your inventory into a line. Player. |
| `/myshop take <offer> <amount>` | `uxmshop.myshop` | Moves items from a line back to you. Player. |
| `/myshop remove <offer>` | `uxmshop.myshop` | Takes a line off the shop. Player. |
| `/myshop price <offer> <buy> <sell>` | `uxmshop.myshop` | Changes what a line charges. Player. |
| `/myshop staff add <player>` | none | Lets a player restock and reprice. Player. |
| `/myshop staff remove <player>` | none | Takes a player off the shop. Player. |
| `/myshop list` | `uxmshop.myshop` | Lists the shops on the server. Player. |
| `/myshop find` | `uxmshop.myshop` | Shows which shops trade what you are holding. Player. |
| `/myshop profits` | `uxmshop.myshop` | What your shops earned and spent, per item. Player. |
| `/myshop profits clear` | `uxmshop.myshop` | Starts the report again. Player. |

### Operator

| Command | Permission | What it does |
|---|---|---|
| `/uxmshop` (alias `/shopadmin`) | `uxmshop.admin.version` | Says what the command is for. |
| `/uxmshop version` | `uxmshop.admin.version` | Shows the running version. |
| `/uxmshop reload` | `uxmshop.admin.reload` | Reads every file again. |
| `/uxmshop doctor` | `uxmshop.admin.doctor` | Checks the plugin's health and prints one line per check. |
| `/uxmshop logs <player> <page>` | `uxmshop.admin.logs` | What one player traded, newest first. |
| `/uxmshop logs direction <player> <direction> <page>` | `uxmshop.admin.logs` | The same, for one direction. |
| `/uxmshop prune <days>` | `uxmshop.admin.prune` | Deletes log rows older than that many days. |
| `/uxmshop adminshop` | `uxmshop.admin.shop` | Opens a shop with the server behind it on the container you look at. Player. |
| `/uxmshop iteminfo` | `uxmshop.admin.iteminfo` | The item in your hand as a block to paste into a section file. Player. |
| `/uxmshop modifier all <player> <factor>` | `uxmshop.admin.modifier` | A price multiplier for one player over the whole shop. |
| `/uxmshop modifier section <player> <section> <factor>` | `uxmshop.admin.modifier` | The same for one section. |
| `/uxmshop modifier line <player> <section> <line> <factor>` | `uxmshop.admin.modifier` | The same for one line. |
| `/uxmshop modifier clear <player>` | `uxmshop.admin.modifier` | Removes every modifier of a player. |
| `/uxmshop resetprice <section> <line>` | `uxmshop.admin.reset` | Puts a line back on its base price. |
| `/uxmshop resetstock <section> <line> <direction>` | `uxmshop.admin.reset` | Resets the server stock of a line in one direction. |
| `/uxmshop resetquota <player> <section> <line> <direction>` | `uxmshop.admin.reset` | Resets one player's quota on a line. |

### Editor

Registered only while `editor.enabled` is `true`. Every subcommand needs `uxmshop.editor`. See
[the editor](editor.md).

| Command | What it does |
|---|---|
| `/shopedit` | Says what the editor is for. |
| `/shopedit section <name>` | Starts a section, with the item in your hand as its icon. Player. |
| `/shopedit add <section> <line> <buy> <sell>` | Adds the item in your hand to a section. Player. |
| `/shopedit price <section> <line> <buy> <sell>` | Changes what a line charges. |
| `/shopedit match <section> <line> <mode>` | Changes how strictly a line matches. |
| `/shopedit remove <section> <line>` | Takes a line out. |
| `/shopedit tiles <window>` | Names the tiles of a window and the slots they hold. |
| `/shopedit move <window> <tile> <slot>` | Moves a tile to another slot. |
| `/shopedit icon <window> <tile>` | Draws a tile from the item in your hand. Player. |

## Permissions

| Permission | Default |
|---|---|
| `uxmshop.use` | everyone |
| `uxmshop.value` | everyone |
| `uxmshop.sell.hand` | everyone |
| `uxmshop.sell.all` | everyone |
| `uxmshop.sell.window` | everyone |
| `uxmshop.shops` | everyone |
| `uxmshop.myshop` | everyone |
| `uxmshop.section.*` | op |
| `uxmshop.rank.*` | op |
| `uxmshop.bypass.tax` | op |
| `uxmshop.bypass.blacklist` | op |
| `uxmshop.editor` | op |
| `uxmshop.admin.*` | op, grants every `uxmshop.admin` permission |
| `uxmshop.admin.shop` | op |
| `uxmshop.admin.modifier` | op |
| `uxmshop.admin.iteminfo` | op |
| `uxmshop.admin.logs` | op |
| `uxmshop.admin.reset` | op |
| `uxmshop.admin.prune` | op |
| `uxmshop.admin.doctor` | op |
| `uxmshop.admin.version` | op |
| `uxmshop.admin.reload` | op |
| `uxmshop.admin.language` | op |

Families the operator completes with names of their own:

| Permission | Meaning |
|---|---|
| `uxmshop.section.<section>` | Opens one catalogue section. A section file may name a different node in `permission`. |
| `uxmshop.rank.<rank>` | The node of a rank written in `shop.ranks`, unless the rank names its own `permission`. |
| `uxmshop.bypass.blacklist.<material>` | Trades one blacklisted material. |
| `uxmshop.shops.<n>` | How many player shops a player may own. `uxmshop.shops.*` is no limit. Without one, `player-shops.limit` applies. The prefix is `player-shops.limit-permission`. |

A catalogue section or line may also name `permission`, `buy-permission` and `sell-permission` nodes in its
file. See [the catalogue](catalogue.md).

## Placeholders

PlaceholderAPI is needed. The identifier is `uxmshop`. Every answer is read from memory, and an unknown key
or a request with no player answers nothing.

| Placeholder | Answer |
|---|---|
| `%uxmshop_sections%` | How many catalogue sections there are. |
| `%uxmshop_lines%` | How many lines of the catalogue can be traded. |
| `%uxmshop_shops%` | How many player shops there are on the server. |
| `%uxmshop_my_shops%` | How many shops the player owns. |
| `%uxmshop_shop_limit%` | How many shops the player may own. An offline player gets the default limit. |

---
title: "Guide"
order: 10
description: "How uxmCosmetics works, how to set it up and what each file holds."
---

uxmCosmetics gives players a wardrobe of cosmetics in twelve categories: trails, halos, cloaks, balloons, pets,
mounts, morphs, hats, suits, banners, emotes and gadgets. A player wears one cosmetic per category. Cosmetics are
bought with the server's currency, won from mystery boxes, granted by staff, or unlocked by a permission.

## Install

Put `uxmCosmetics-<version>.jar` in `plugins/` and start the server. The first start writes `config.conf`,
`boxes.conf`, `stations.conf`, one file per category under `categories/`, the menus under `menus/` and the message
files into `plugins/uxmCosmetics/`. The shipped prices are in experience points, so buying works with nothing else
installed. Players open the wardrobe with `/cosmetics` or the nether star in hotbar slot 8.

`/uxmcosmetics reload` reads every file again. A cosmetic or box that does not parse is dropped with its name in
the console; the rest load. `storage`, `animation`, `permissions.wildcard` and `commands.conf` are read once, so a
change to them needs a restart.

## How a player gets a cosmetic

A player may wear a cosmetic they own or one whose permission they hold. A cosmetic's permission is
`uxmcosmetics.<category>.<id>` unless its entry names another. `uxmcosmetics.*` unlocks every cosmetic while
`permissions.wildcard` is on.

- **Buying.** A cosmetic with a price above 0 is for sale. The price is taken, then the cosmetic is added; if the
  save fails, the money goes back. A player who already owns it, or holds its permission, is not charged.
- **Mystery boxes.** Opening a box takes its price and draws one entry from its weighted table. A new cosmetic is
  added to the wardrobe. A cosmetic the player already owns pays coins back instead: the entry's
  `duplicate-coins`, or `economy.duplicate-refund-fraction` of the cosmetic's own price. A table entry that names a
  cosmetic that does not exist refunds the box.
- **Staff.** `/uxmcosmetics grant <player> <cosmetic>` and `revoke`. Revoking also takes the cosmetic off.

## Content files

Each `categories/<category>.conf` holds a `cosmetics` list. Every entry has an `id`, `name`, `description`,
`rarity` (common, uncommon, rare, epic or legendary), `price`, `icon`, an optional `permission`, and the keys of its
kind: an `item` or `pieces` for worn cosmetics, a particle pattern for trails, halos and cloaks, an entity for
followers. The comments at the top of each file list them.

`boxes.conf` holds the mystery boxes:

| Key | Meaning |
|---|---|
| `id` | The box's name, used in its permission and in a station. |
| `tier` | common, uncommon, rare, epic or legendary. |
| `icon`, `price` | The menu item and the cost of one opening. |
| `permission` | Who may open it. Default `uxmcosmetics.box.<id>`. |
| `rewards` | Entries of `cosmetic`, `weight` and optional `duplicate-coins`. A weight of 0 is never drawn. |

A box with no entries, or whose weights are all 0, is refused when the file is read.

`stations.conf` binds blocks in the world to boxes. Stand in front of the block and run
`/uxmcosmetics station add <box>` or `station add all`; the entry is written for you.

## Configuration

| Key | Default | Meaning |
|---|---|---|
| `permissions.wildcard` | `true` | Whether `uxmcosmetics.*` alone unlocks every cosmetic and box. |
| `animation.tick-ms` | `100` | How often the one animation task runs. Raise it on a busy server. |
| `animation.follower-speed` | `0.35` | How fast pets, balloons and mounts catch up; 1.0 snaps. |
| `animation.max-particles-per-frame` | `64` | The most particle points one player may cost per frame. |
| `economy.purchase-enabled` | `true` | Whether cosmetics can be bought. The wardrobe still works without it. |
| `economy.currency` | `"exp"` | `exp`, `levels`, `vault`, `vault-unlocked`, `playerpoints`, `ecobits:<currency>` or `treasury`. |
| `economy.format` | `"#,##0.## 'exp'"` | How an amount is written. |
| `economy.duplicate-refund-fraction` | `0.25` | The share of a cosmetic's price a duplicate pays back. |
| `opener` | slot 8, nether star | The hotbar item that opens the wardrobe: `enabled`, `give-on-join`, `slot`, `material`, `keep-on-death`. |
| `gadget.slot` | `5` | The hotbar slot a worn gadget takes. |
| `cleanup.sweep-on-start` | `true` | Removes followers left behind by a crash when the server starts. |
| `menu.roulette` | | The spin shown when a box opens: `spins`, intervals and sounds. |
| `station` | | How close a click opens a station, its particle effect and its hologram. |
| `effects` | a message and a sound | What happens on `equipped`, `unequipped`, `unequipped-all`, `bought` and `box-won`. |

## Commands

| Command | Permission | What it does |
|---|---|---|
| `/uxmcosmetics` |  | Administer the plugin. |
| `/uxmcosmetics grant <player> <cosmetic>` | `uxmcosmetics.admin.grant` | Give a cosmetic to a player, online or not. |
| `/uxmcosmetics revoke <player> <cosmetic>` | `uxmcosmetics.admin.revoke` | Take a cosmetic away again. |
| `/uxmcosmetics reload` | `uxmcosmetics.admin.reload` | Read the configuration and the content again. |
| `/uxmcosmetics cleanup` | `uxmcosmetics.admin.cleanup` | Remove a follower that outlived its owner. |
| `/uxmcosmetics version` | `uxmcosmetics.admin.version` | Read the version this server runs. |
| `/uxmcosmetics station add <box> [block]` | `uxmcosmetics.admin.station` | Bind a box station to a block. |
| `/uxmcosmetics station set <station> <piece> <value>` | `uxmcosmetics.admin.station` | Set one piece of one station. |
| `/uxmcosmetics station show <station>` | `uxmcosmetics.admin.station` | Print every piece of one station. |
| `/uxmcosmetics station remove <station>` | `uxmcosmetics.admin.station` | Take a box station away again. |
| `/uxmcosmetics station` | `uxmcosmetics.admin.station` | The bare word lists them. |
| `/uxmcosmetics station list` | `uxmcosmetics.admin.station` | Every box station this server has. |
| `/uxmcosmetics doctor` | `uxmcosmetics.admin.doctor` | Say how this plugin is, in one line per thing that can be wrong. |
| `/cosmetics` |  | Open your wardrobe. |
| `/cosmetics open <category>` |  | Open one category straight away. Players only. |
| `/cosmetics boxes` |  | Open the mystery boxes. Players only. |
| `/cosmetics equip <cosmetic>` |  | Wear one cosmetic. Players only. |
| `/cosmetics unequip <category>` |  | Take off whatever is worn in one category. Players only. |
| `/cosmetics unequip all` |  | Take everything off in one go. Players only. |
| `/cosmetics lang` | `uxmcosmetics.language` | Say what language this player reads, and what else this server has. Players only. |
| `/cosmetics lang set <language>` | `uxmcosmetics.language` | Read this plugin in one language, whatever the client is set to. Players only. |
| `/cosmetics lang reset` | `uxmcosmetics.language` | Follow the client again, as a player who never chose anything does. Players only. |
| `/cosmetics lang set-player <player> <language>` | `uxmcosmetics.admin.language` | Set the language another player reads. |
| `/cosmetics lang reset-player <player>` | `uxmcosmetics.admin.language` | Clear the language another player chose. |

The `/cosmetics` subcommands need `uxmcosmetics.use`, which every player has. All nodes and their defaults are in
[reference.md](reference.md).

## Placeholders

| Placeholder | Value |
|---|---|
| `%uxmcosmetics_equipped_<category>%` | The name of what the player wears in a category, or empty. |
| `%uxmcosmetics_equipped_id_<category>%` | Its id, or empty. |
| `%uxmcosmetics_owned%` | How many cosmetics the player owns. |
| `%uxmcosmetics_owned_<category>%` | How many they own in one category. |
| `%uxmcosmetics_total%` | How many cosmetics the server has. |
| `%uxmcosmetics_total_<category>%` | How many in one category. |

## Examples

**Sell with the server economy.** Set `economy.currency = "vault"` and `economy.format = "'$'#,##0"`, then adjust the
prices in the category files and `boxes.conf`.

**A box only donors can open.** Add a box with `permission = "rank.donor"` and grant that node to the donor group.

**A box in the lobby.** Place a chest, look at it and run `/uxmcosmetics station add common`.

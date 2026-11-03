---
title: Player commands
order: 302
description: Every /enchants subcommand, with its arguments, node and what it does.
icon: user
---

Every subcommand here needs `uxmenchants.use`, which everyone has by default, except the two that set another player's
language.

| Command | What it does |
|---|---|
| `/enchants` | Opens the [wiki](../menus/wiki.md) |
| `/enchants groups` | Opens the [grouped wiki](../menus/grouped.md): one tile per rarity, type and target |
| `/enchants info <enchantment>` | Opens the [page](../menus/info.md) of one enchantment; tab offers every id |
| `/enchants list` | One chat line per enchantment this server defines: name, rarity and type |
| `/enchants limit` | The whole limit calculation for the item in your main hand |
| `/enchants descriptions` | Turns the descriptions under enchantments on or off, for you only |
| `/enchants lang` | The language you read this plugin in, and the ones this server has |
| `/enchants lang set <language>` | Read this plugin in one language, whatever your client says |
| `/enchants lang reset` | Follow the client again |
| `/enchants lang set-player <player> <language>` | `uxmenchants.admin.language`: set another player's language |
| `/enchants lang reset-player <player>` | `uxmenchants.admin.language`: clear another player's choice |

## info

`/enchants info sharpened`. An id that is not defined answers `enchant.unknown`: "There is no enchantment called
sharpened."

## list

```
Enchantments (12)
| Blunted : Common, Enchantment
| Curse of Brittleness : Common, Curse
...
```

A server with no enchantments answers `list.empty`. Names, rarities and types are in the reader's language.

## limit

```
Your limit
| Everybody starts with 3.
| uxmenchants.limit.6 raises that to 6.
| This item uses 2 of 6.
| 4 left.
```

Without a node above the base the second line reads "No node of yours raises it." An empty hand counts as an item that
carries nothing. This is the command a refusal names: "The item is full at 3. Run /enchants limit to see why."

## descriptions

Flips the choice and says `Descriptions are on` or `off`. A player who never used it sees
`display.descriptions-on-by-default` (true). The choice is kept in the database and follows the player across a
network.

## lang

`lang set` takes a language this server has a file for, by tag (`tr`, `pt-BR`); tab offers them, and a tag it does not have lists
the ones it does. When `language.force` is set, `lang set` and `lang reset` are refused with the forced language. With
uxmLang installed the choice is uxmLang's and shared by every UXM plugin.

`lang set-player` and `lang reset-player` work on a player who is away: the change is read on their next join.

Related: [Admin commands](admin.md), [Languages](../config/languages.md)

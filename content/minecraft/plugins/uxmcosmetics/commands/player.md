---
title: Player commands
order: 302
description: Every /cosmetics subcommand, what it opens or does, and what it answers.
icon: user
---

`/cosmetics`, aliases `/cosmetic` and `/wardrobe`.

| Command | Permission | What it does |
|---|---|---|
| `/cosmetics` | `uxmcosmetics.use` | Opens the [wardrobe](../menus/wardrobe.md) |
| `/cosmetics open <family>` | `uxmcosmetics.use` | Opens one [family](../menus/category.md) straight away: `trail`, `halo`, `cloak`, `hat`, `suit`, `emote`, `balloon`, `gadget`, `pet`, `mount`, `morph`, `banner` |
| `/cosmetics boxes` | `uxmcosmetics.use` | Opens the [mystery boxes](../menus/boxes.md) |
| `/cosmetics equip <cosmetic>` | `uxmcosmetics.use` | Puts on a piece the player has, by its id. Tab completion offers the pieces they own |
| `/cosmetics unequip <family>` | `uxmcosmetics.use` | Takes off what is worn in one family |
| `/cosmetics unequip all` | `uxmcosmetics.use` | Takes everything off in one go |
| `/cosmetics lang` | `uxmcosmetics.language` | The language the player reads, and the choices |
| `/cosmetics lang set <language>` | `uxmcosmetics.language` | Chooses a language |
| `/cosmetics lang reset` | `uxmcosmetics.language` | Follows the client's language again |
| `/cosmetics lang set-player <player> <language>` | `uxmcosmetics.admin.language` | Sets another player's language; one who is away gets it at their next join |
| `/cosmetics lang reset-player <player>` | `uxmcosmetics.admin.language` | Clears another player's choice |

## Answers

| Command | Answer |
|---|---|
| `equip` | `equip.done`; `equip.already` if it is on; `equip.not-owned` without owning it or its node; `error.unknown-cosmetic` for an id nobody wrote |
| `unequip` | `unequip.done` with the piece; `unequip.nothing` when the family is empty |
| `unequip all` | `unequip.all` with the count; `unequip.all-nothing` when nothing was on |

`equip` does not buy: a piece the player does not have is bought from the [family window](../menus/category.md).
An emote equipped by command is played by clicking it in its window.

## The effects

Every equip, unequip, take-everything-off, purchase and box win runs the matching list under `effects` in
[config.conf](../config/config-conf.md#effects): by default a message and a sound.

Related: [Overview](overview.md), [The wardrobe](../features/wardrobe.md)

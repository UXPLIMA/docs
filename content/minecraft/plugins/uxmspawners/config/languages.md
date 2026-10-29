---
title: Language and Menus
order: 62
description: The language files, which hold the messages and the four menus, and what each menu item can show.
icon: languages
---

Everything players read is in `lang/lang_<code>.yml`: the messages, the four menus, the hologram
lines and the names of mobs. `config.lang` picks the file: `en`, `tr`, `pl`, `de` or `fr`. A code
without a file falls back to English, with a warning. Changes apply with `/uxmspawners reload`.

| Section | Holds |
|---|---|
| `menu` | The main menu of a spawner |
| `storageMenu` | The storage, with the sell button |
| `settingsMenu` | The virtual storage and XP storage switches |
| `friendsMenu` | The friends list |
| `lang` | Every message, the hologram lines and the storage bar |
| `translates` | Mob names, used by `{entity_name_lang}` in spawner names |

To add a language, copy `lang_en.yml` to a new name such as `lang_es.yml`, translate it and set
`config.lang: es`. Keep every key: a missing message shows as `Missing message for key:` and is noted
in `logs/errors.txt`. Updates add new keys only to the five shipped files.

## A menu item

```yaml
  storage:
    gui: true
    material: CHEST
    name: "<white><bold>STORAGE <dark_gray>- <gray>Sell/Take Items"
    lore:
      - " <dark_gray>• <gray>Items stored: <white><amount>x"
    slot: 29
```

| Key | Meaning |
|---|---|
| The key, `storage` | What the item does. The keys are fixed; a new key does nothing |
| `gui` | Main menu only: `false` hides the item |
| `material` | An item name, or `head-` followed by a texture value for a custom head |
| `name`, `lore` | In MiniMessage |
| `slot` | Where it sits, counting from 0 at the top left |

Each menu also has `menuName`, `menuRows` and `fillItem`, the item that fills empty slots. Set the fill
item's `material` to `AIR` for none. The friends menu always has 6 rows.

## The main menu

| Key | Does | Shown for item spawners |
|---|---|---|
| `info` | Shows the type, level, owner and active boosts. Clicking closes the menu | Yes |
| `storage` | Opens the storage menu | Yes |
| `level-up` | Merges matching spawners from the inventory. See [Levels](../features/levels.md) | Yes |
| `exp` | Gives all stored XP | No |
| `auto-kill` | Switches auto-kill | No |
| `hologram` | Switches the hologram | Yes |
| `friends` | Opens the friends menu, for the owner | Yes |
| `settings` | Opens the settings menu | Yes |

Placeholders in main menu lore: `<type>`, `<level>`, `<owner>`, `<amount>` (items stored),
`<multiplier>` (the viewer's sell multiplier), `<experience>`, `<money>` (what selling everything
pays) and `<active_boosts>`, which becomes one line per boost with its time left.

## The other menus

| Menu | Item | Placeholders |
|---|---|---|
| `storageMenu` | `sellItem` | `<money>`, `<multiplier>` |
| `storageMenu` | `itemLores`, added under every stored item | `<amount_type>` (how many), `<storage_limit>`, `<storage_bar>` |
| `settingsMenu` | `virtual_storage`, `xp_storage` | `{virtual_storage}`, `{xp_storage}`: a green tick or a red cross |
| `friendsMenu` | `addItem`, and `friends.lore` under each friend's head | None |

## Messages

Messages in `lang` are MiniMessage and start with `lang.prefix`. Values the plugin fills in are
numbered, `<0>`, `<1>`, `<2>`, and each message has its own: in `tookItems`, `<0>` is the amount and
`<1>` the item. Keep the numbers a message already uses. The title texts `sellItemsTitle` and
`takeXpTitle` use `{money_gained}` and `{xp_gained}` instead.

`storage-bar` sets the bar in menus and holograms: the `character`, repeated ten times, and a colour
for each fill level from `color-empty` to `color-full`.

Related: [Holograms](../features/holograms.md), [Friends](../features/friends.md), [config.yml](config-yml.md)

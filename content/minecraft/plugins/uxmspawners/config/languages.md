---
title: Language and Menus
order: 162
description: The language folders, the messages, the five menus, the actions a menu item can run and the placeholders it can show.
icon: languages
---

Everything players read is in `languages/<code>/`: `messages.yml` and five menu files in `menus/`.
`config.lang` picks the folder: `en`, `tr`, `pl`, `de` or `fr`, or one you add. Changes apply with
`/usp reload`.

```
languages/en/
├── messages.yml               messages, help, mob names, spawn condition texts, number format, storage bar
└── menus/
    ├── spawner-main-menu.yml  the main menu of a spawner
    ├── storage-menu.yml       the storage, with Sell All
    ├── withdraw-menu.yml      how many of one stored item to take
    ├── settings-menu.yml      Virtual Storage, XP Storage, Hologram
    └── friends-menu.yml       the friends list
```

To add a language, copy the whole `languages/en` folder to a new name such as `languages/es`,
translate it and set `config.lang: es`. A language whose folder lacks a file falls back to English,
with a warning. A message missing from `messages.yml` shows the shipped text, in English for a
language you added. An update adds new messages, and new mob names, to the `messages.yml` of the
language in use, and keeps the mob names you added; menus are never changed by an update.

## Messages

`messages.yml` is MiniMessage. With PlaceholderAPI installed, `%placeholders%` work too.

| Part | Holds |
|---|---|
| `prefix` | The tag `<prefix>` at the start of a message stands for this. A message without the tag has no prefix |
| The messages | Values the plugin fills in are named, such as `<amount>`, `<spawner>`, `<player>`, `<money>`, and the same in every language. Keep the ones a message already uses |
| `help` | The `/usp` help: `header`, one line per subcommand, `footer` |
| `translates` | Mob names, used by `<entity_name_lang>` and `<mob>`. A mob it lacks shows as its words, `Happy Ghast` |
| `spawn-conditions` | The lines of the conditions button and `/usp debug`, and `names` for the parts of the day and the weathers. See [Spawn Conditions](../features/spawn-conditions.md#what-players-see) |
| `time-units` | The letters of boost times, `1d 4h 20m` |
| `number-format` | `thousands-separator` and `decimal-separator` for money, amounts and multipliers |
| `storage-bar` | The bars in menus and holograms: `length`, `character`, `color-empty`, and the filled part's colour: `color-low` below 50%, `color-medium` from 50%, `color-medium-high` from 70%, `color-high` from 90% and `color-full` when full |
| `debug` | The lines of `/usp debug` |

## A menu file

```yaml
title: "<#2F6F67>⬢ <#373D4A>Spawner"
rows: 3
filler-material: "AIR"
filler-slots: []
items:
  storage:
    material: "CHEST"
    display-name: "<#E6C27A><bold>Storage"
    lore:
      - "<#E6C27A>• <#9AA3B5>Items <#DCE1EA><amount>"
    slots: [10]
    actions:
      - "[sound] UI_BUTTON_CLICK"
      - "[menu] storage"
```

| Key | Meaning |
|---|---|
| `title` | The menu's title. It takes the spawner placeholders below |
| `rows` | 1 to 6 |
| `filler-material`, `filler-model-data`, `filler-slots` | An item put in each of `filler-slots`. Other empty slots stay empty |
| `pageable-slot-size` | Friends menu only: how many heads one page holds |
| `items` | The items, by name |

| Item key | Meaning |
|---|---|
| `material` | An item name. `PLAYER_HEAD` with `head-data: "texture-<base64>"` or `"head-<player name>"` for a custom head. `<spawner_item>` shows the spawner's own item, and in the withdraw menu `<item>` shows the stored item |
| `display-name`, `lore` | In MiniMessage, with the placeholders below |
| `amount`, `model-data`, `glowing` | Stack size, a custom model data number or an item model (`namespace:key`), an enchantment glint |
| `slots` | A list of slots, counting from 0 at the top left |
| `actions` | What a click does, run in order. See below |
| `enabled`, `disabled` | Two whole items in place of one, see below |

An item with a slot outside the menu is left out with a warning in the console, and the menu still
opens. A menu whose item or filler material is no item does not open: the console names the file,
the item and the material, and the player is told the menu is unavailable. A menu file that cannot be
read leaves out only that menu.

A click that keeps the menu open redraws its items in place. Clicks closer than 150 ms apart are
ignored, and a menu acts only while the player is near its spawner.

## Items with a meaning

Most item names are free: name an item what you like and give it actions. These names mean
something:

| Name | Menu | Meaning |
|---|---|---|
| `auto-kill`, `virtual-storage`, `xp-storage`, `hologram`, `spawner` | Main, Settings | With `enabled` and `disabled` sub items, shows the one for the setting's state. `spawner` is the stop button |
| `exp` | Main, Storage | Hidden on `ITEM` spawners, like `auto-kill` and `xp-storage` |
| `sell` | Storage | Hidden when the spawner type has `sell-button: false` |
| `item-lores` | Storage | Not placed. Its `lore` is added under every stored item |
| `friends` | Friends | Not placed. The look of each friend's head |

`auto-kill` is also hidden while `auto-kill.enabled` is off, and `hologram` while holograms are off.
The main menu's clock, `conditions`, shows the spawn conditions through its placeholders.

## Actions

| Action | Does |
|---|---|
| `[menu] <main, storage, settings or friends>` | Opens that menu of the spawner |
| `[close]` | Closes the menu |
| `[toggle_setting] <auto-kill, virtual-storage, xp-storage, hologram or spawner>` | Switches that setting, with the usual permission and owner checks, and redraws the menu |
| `[level_up]` | Levels the spawner up from the inventory |
| `[take_xp]` | Gives the stored XP to the player |
| `[sell_all]` | Sells the storage |
| `[add_friend]` | Starts the add-friend prompt |
| `[next_page]`, `[previous_page]` | Turns the page of the friends menu |
| `[withdraw] <amount or all>` | Withdraw menu: takes up to that many of the item, or with `all` as many as fit |
| `[withdraw_amount]` | Withdraw menu: asks for an amount in chat |
| `[sound] <sound>` | Plays a sound, by its name, `UI_BUTTON_CLICK`, or its key, `ui.button.click` |
| `[player] <command>` | The player runs the command, without `/` |
| `[console] <command>` | The console runs the command, without `/` |
| `[player_message] <text>`, `[console_message] <text>` | Sends a MiniMessage line to the player or the console |
| `[check_permission] <permission> [message]` | Stops the actions after it when the player lacks the permission, and sends the message if one is given |
| `[wait] <ticks>` | Runs the actions after it that many ticks later |

`%player%` is the player's name in `[player]` and `[console]`, and PlaceholderAPI placeholders work in
both. An action that fails stops the ones after it. Action names take `-` as well as `_`:
`[toggle-setting]` works. Back buttons open their menu by name, `[menu] main`; there is no `[back]`.

## Placeholders

In menu titles, item names and lore:

| Placeholder | Shows |
|---|---|
| `<mob>`, `<type>` | The mob's translated name, and its name as written in the game, `ZOMBIE` |
| `<level>`, `<owner>`, `<friends>` | Level, owner's name, number of friends |
| `<amount>`, `<experience>` | Items and XP stored |
| `<xp_storage_bar>`, `<xp_percentage>`, `<xp_color>` | How full the XP Storage is; `<xp_color>` colours the text after it |
| `<money>`, `<multiplier>` | What Sell All pays the viewer, and the viewer's multiplier |
| `<worth>` | The money, or "Not for sale" when the spawner type does not sell |
| `<active>` | Whether the spawner runs, as `toggle-enabled` or `toggle-disabled` |
| `<active_boosts>` | One line per active boost with its time left. Write it as a lore line of its own |
| `<conditions_state>` | Whether the spawner works or waits for its conditions |
| `<conditions>` | One line per spawn condition that is on, met or not. Write it as a lore line of its own |
| `<auto_kill_status>`, `<virtual_storage_status>`, `<xp_storage_status>`, `<hologram_status>` | The setting's state |
| `<auto_kill_click>`, `<virtual_storage_click>`, `<xp_storage_click>`, `<hologram_click>` | What a click does, or that the server forces the setting |
| `<virtual_storage>`, `<xp_storage>`, `<hologram>` | The setting's state, for the main menu |

For a stored item, in the storage menu's `item-lores` and in the withdraw menu: `<amount_type>` or
`<stored>` (how many), `<storage_limit>`, `<storage_bar>` and `<storage_color>`, the colour of how full
it is. The withdraw menu also has `<item>`, the item's name, and `<fits>`, how many fit in the
inventory. In the friends menu: `<friends>`, and `<uuid>` in the head's lore. With PlaceholderAPI
installed, `%placeholders%` work in all of them.

Related: [Holograms](../features/holograms.md), [Friends](../features/friends.md), [config.yml](config-yml.md)

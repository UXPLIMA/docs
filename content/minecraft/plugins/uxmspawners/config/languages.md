---
title: Language and Menus
order: 162
description: The language folders, the messages, the four menus, the actions a menu item can run and the placeholders it can show.
icon: languages
---

Everything players read is in `languages/<code>/`: `messages.yml` and four menu files in `menus/`.
`config.lang` picks the folder: `en`, `tr`, `pl`, `de` or `fr`, or one you add. Changes apply with
`/usp reload`.

```
languages/en/
├── messages.yml               messages, help, mob names, number format, storage bar
└── menus/
    ├── spawner-main-menu.yml  the main menu of a spawner
    ├── storage-menu.yml       the storage, with Sell All
    ├── settings-menu.yml      Virtual Storage, XP Storage, Hologram
    └── friends-menu.yml       the friends list
```

To add a language, copy the whole `languages/en` folder to a new name such as `languages/es`,
translate it and set `config.lang: es`. A language whose folder lacks a file falls back to English,
with a warning. A message missing from `messages.yml` shows the shipped text, in English for a
language you added. An update adds new messages to the `messages.yml` of the language in use; menus
are never changed by an update.

## Messages

`messages.yml` is MiniMessage. With PlaceholderAPI installed, `%placeholders%` work too.

| Part | Holds |
|---|---|
| `prefix` | The tag `<prefix>` at the start of a message stands for this. A message without the tag has no prefix |
| The messages | Values the plugin fills in are numbered, `<0>`, `<1>`, `<2>`, or named, `<player_name>`, `<seconds>`. Keep the ones a message already uses |
| `help` | The `/usp` help: `header`, one line per subcommand, `footer` |
| `translates` | Mob names, used by `<entity_name_lang>` and `<mob>`. A mob it lacks shows as its words, `Happy Ghast` |
| `time-units` | The letters of boost times, `1d 4h 20m` |
| `number-format` | `thousands-separator` and `decimal-separator` for money, amounts and multipliers |
| `storage-bar` | The bar in menus and holograms: `length`, `character`, and a colour for each fill level |
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
| `material` | An item name. `PLAYER_HEAD` with `head-data: "texture-<base64>"` or `"head-<player name>"` for a custom head |
| `display-name`, `lore` | In MiniMessage, with the placeholders below |
| `amount`, `model-data`, `glowing` | Stack size, custom model data, an enchantment glint |
| `slots` | A list of slots, counting from 0 at the top left |
| `actions` | What a click does, run in order. See below |
| `enabled`, `disabled` | Two whole items in place of one, see below |

An item with a slot outside the menu is left out with a warning in the console, and the menu still
opens. A click that keeps the menu open redraws its items in place.

## Items with a meaning

Most item names are free: name an item what you like and give it actions. These names mean
something:

| Name | Menu | Meaning |
|---|---|---|
| `auto-kill`, `virtual-storage`, `xp-storage`, `hologram`, `spawner` | Main, Settings | With `enabled` and `disabled` sub items, shows the one for the setting's state. `spawner` is the stop button |
| `exp` | Main | Hidden on `ITEM` spawners, like `auto-kill` and `xp-storage` |
| `sell` | Storage | Hidden when the spawner type has `sell-button: false` |
| `item-lores` | Storage | Not placed. Its `lore` is added under every stored item |
| `friends` | Friends | Not placed. The look of each friend's head |

`auto-kill` is also hidden while `auto-kill.enabled` is off, and `hologram` while holograms are off.

## Actions

| Action | Does |
|---|---|
| `[menu] <main, storage, settings or friends>` | Opens that menu of the spawner |
| `[back]` | Back to the main menu |
| `[close]` | Closes the menu |
| `[toggle_setting] <auto-kill, virtual-storage, xp-storage, hologram or spawner>` | Switches that setting, with the usual permission and owner checks |
| `[level_up]` | Levels the spawner up from the inventory |
| `[take_xp]` | Gives the stored XP to the player |
| `[sell_all]` | Sells the storage |
| `[add_friend]` | Starts the add-friend prompt |
| `[next_page]`, `[previous_page]` | Turns the page of the friends menu |
| `[sound] <sound>` | Plays a sound, by its name, `UI_BUTTON_CLICK`, or its key, `ui.button.click` |
| `[player] <command>` | The player runs the command, without `/`. `%player%` is the player's name |
| `[console] <command>` | The console runs the command, without `/`. PlaceholderAPI placeholders work |
| `[player_message] <text>`, `[console_message] <text>` | Sends a MiniMessage line to the player or the console |
| `[check_permission] <permission> [message]` | Stops the actions after it when the player lacks the permission, and sends the message if one is given |
| `[wait] <ticks>` | Runs the actions after it that many ticks later |

An action that fails stops the ones after it. The 4.2.0 forms `[toggle_autokill]`,
`[toggle_virtual_storage]`, `[toggle_xp_storage]` and `[toggle_hologram]` still work.

## Placeholders

In menu titles, item names and lore:

| Placeholder | Shows |
|---|---|
| `<mob>`, `<type>` | The mob's translated name, and its name as written in the game, `ZOMBIE` |
| `<level>`, `<owner>`, `<friends>` | Level, owner's name, number of friends |
| `<amount>`, `<experience>` | Items and XP stored |
| `<storage_bar>`, `<storage_percentage>`, `<xp_storage_bar>`, `<xp_percentage>` | How full the storage is |
| `<money>`, `<multiplier>` | What Sell All pays the viewer, and the viewer's multiplier |
| `<active>` | Whether the spawner runs, as `toggle-enabled` or `toggle-disabled` |
| `<active_boosts>` | One line per active boost with its time left. Write it as a lore line of its own |
| `<auto_kill_status>`, `<virtual_storage_status>`, `<xp_storage_status>`, `<hologram_status>` | The setting's state |
| `<auto_kill_click>`, `<virtual_storage_click>`, `<xp_storage_click>`, `<hologram_click>` | What a click does, or that the server forces the setting |
| `<virtual_storage>`, `<xp_storage>`, `<hologram>` | The setting's state, for the main menu |

In the storage menu's `item-lores`: `<amount_type>` (how many of that item), `<storage_limit>` and
`<storage_bar>`. In the friends menu: `<friends>`, and `<uuid>` in the head's lore. With PlaceholderAPI
installed, `%placeholders%` work in all of them.

Related: [Holograms](../features/holograms.md), [Friends](../features/friends.md), [config.yml](config-yml.md)

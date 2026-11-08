---
title: Messages and menus
order: 404
description: Where every line players read lives, how a message is delivered, and how menus are laid out.
icon: message-square
---

No text players see is written in the plugin's code. Every line is in `messages.yml` and the
`menu/*.yml` files, in the [language](../features/languages.md) set in `config.yml`. The Discord texts
are in [webhooks.yml](webhooks-yml.md).

## A message

```yaml
infoBuildStarted:
  type: 'CHAT'
  text:
    - '<green>Construction started!</green> <dark_gray>#%job.id%</dark_gray>'
    - '<gray>Total blocks: <white>%job.total%</white></gray>'
  sound: 'BLOCK_ANVIL_USE'
  volume: 0.6
  pitch: 1.0
```

| Key | Meaning |
|---|---|
| `type` | Where it goes; see below |
| `text` | One line or a list, in MiniMessage |
| `subtitle` | For `TITLE` |
| `fadeIn`, `stay`, `fadeOut` | Ticks, for `TITLE`; `stay` also for `BOSSBAR` |
| `color` | Bar colour for `BOSSBAR` |
| `sound`, `volume`, `pitch` | Played with the message |
| `permission` | For `BROADCAST`, only players holding it receive it |

A simple chat line can be written on one line: `trustAdded: '<green>...</green>'`.

| Type | Delivered as |
|---|---|
| `CHAT` | A chat line to the player |
| `BROADCAST` | A chat line to everyone online |
| `ACTIONBAR` | Above the hotbar |
| `TITLE` | A title and subtitle |
| `BOSSBAR` | A temporary boss bar, hidden after `stay` ticks |
| `CONSOLE` | The server log, as plain text |
| `NONE` | Nothing; the way to silence a message without deleting it |

A misspelled `type` is logged once and treated as `CHAT` rather than breaking the file. A `sound` that is
not one of Minecraft's sound names is played as a sound key, so sounds from a resource pack work.

## Placeholders

Messages carry `%name%` placeholders filled by the plugin, such as `%job.id%`, `%package.name%`,
`%economy.refund%` or `%builder.level%`. Each file's comments list what each line receives.
PlaceholderAPI placeholders are not parsed inside these files.

## Menus

One file per menu under `menu/`:

| File | Menu |
|---|---|
| `main.yml` | `/builders`: shop, active builds, crew, warehouse, history, balance |
| `shop.yml` | Category tabs and packages |
| `confirm.yml` | The purchase confirmation |
| `jobs.yml` | Active builds, your own and those shared with you |
| `job.yml` | One build: progress, pause, resume, boost, materials, cancel |
| `materials.yml` | A package's or a build's material list |
| `crew.yml` | Builders: select, buy, upgrade |
| `warehouse.yml` | The material store |
| `history.yml` | Finished and cancelled builds |

```yaml
title: '<white>Construction #%job.id%</white>'
size: 5

# The progress bar: one pane per slot, lit from the first as the build advances.
itemSlots: [ 10-16 ]

items:
  cancel:
    type: 'BARRIER'
    name: '<red><bold>Cancel Build</bold></red>'
    lore:
      - '<gray>Refund:</gray> <gold>%refund%</gold>'
    glow: false
    amount: 1
    model: 0
    slot: 24
```

| Key | Meaning |
|---|---|
| `title` | MiniMessage, with the tokens the file's header lists |
| `size` | Rows, 1 to 6 |
| `itemSlots` | The slots a list fills (packages, builds, builders, history entries, materials), in the order written |
| `storageSlots` | In `warehouse.yml`, the slots that hold real items |
| `items` | The menu's buttons and the templates its lists are drawn from |
| `otherItems` | Decoration: an entry with a slot is drawn there; the one without a slot fills every empty slot |

Slot lists take single slots and ranges: `[ 10-16, 19-25, 31 ]`.

Each item takes:

| Key | Meaning |
|---|---|
| `type` | The material |
| `name`, `lore` | MiniMessage |
| `glow` | Enchantment shimmer |
| `amount` | Stack size, from 1 to the material's maximum |
| `model` | Custom model data, used when above 0 |
| `slot`, `slots` | Where it goes; `slots` is a list for an item shown in several places |
| `texture` | A base64 skin value, for a `PLAYER_HEAD` |
| `playerCommands`, `consoleCommands` | Commands run on click, as the player or as the console, with the item's tokens filled in |

Some items take their material from elsewhere (a package's or a category's icon) and use only the name
and lore from the file; the header comment of each file says which, and which placeholders each item
gets. An item without a slot is not drawn.

## Updating

An update never replaces the files on your server, yet they stay complete:

| Your file lacks | What happens |
|---|---|
| A message key | The English text built into the plugin is used. New keys are also added to your file when the plugin updates it |
| A menu button added in a later version | It is taken from the shipped copy in memory and shown, without your file changing |
| A decorative `otherItems` entry you deleted | It stays deleted |

To change a line, edit it; to silence a message, set its `type` to `NONE`. To hide a menu button, keep
its entry and remove its `slot`: a button you delete from `items` comes back from the shipped copy.

Related: [Languages](../features/languages.md), [Placeholders](../placeholders.md)

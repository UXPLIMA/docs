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
| `market-browse.yml` | The [player market](../features/market/index.md): category tabs, sort, search and listings |
| `market-listing.yml` | One listing: details, buy, preview, materials, report |
| `market-confirm.yml` | The purchase confirmation of a design |
| `market-designs.yml` | My designs: what a player bought |
| `market-sell.yml` | A listing before it is published: icon, category, fee, publish |
| `market-mine.yml` | A seller's listings and earnings |
| `market-manage.yml` | One of the seller's listings: price, texts, category, icon, pause, withdraw |
| `market-reports.yml` | Reported listings, for moderators |
| `market-inspect.yml` | One listing as moderators see it, with their decisions |

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
| `itemSlots` | The slots a list fills (packages, builds, builders, history entries, materials, listings), in the order written |
| `storageSlots` | In `warehouse.yml`, the slots that hold real items; in `market-browse.yml`, the category tabs |
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

## Bedrock forms

Six market menus have a second file under `menu/bedrock/`: `market-browse`, `market-listing`,
`market-confirm`, `market-mine`, `market-designs` and `market-sell`. A player joining through Geyser or
Floodgate, with `bedrock.enabled` and `bedrock.menus` on in [config.yml](config-yml.md#dialog-and-bedrock),
gets these menus as a list of buttons instead of a chest. The buttons are the chest menu's items in slot
order, without the frame panes, and pressing one does what clicking the item does. Every other menu
stays a chest for Bedrock players.

```yaml
enabled: true
title: ''
content:
  - '<gray>Designs you bought: <white>%designs%</white></gray>'
buttons:
  entry:
    label: '<white>%purchase.title%</white><newline><gray>Builds: <white>%purchase.credits%</white></gray>'
    clicks:
      LEFT: '<green>Get the blueprint</green>'
      RIGHT: '<aqua>Preview where you look</aqua>'
      SHIFT_LEFT: '<white>Materials</white>'
```

| Key | Meaning |
|---|---|
| `enabled` | `false` gives Bedrock players the chest menu here too |
| `title` | The form's title; empty uses the chest menu's |
| `content` | Lines shown above the buttons |
| `buttons` | Only the items that should look different on Bedrock, by their key in the chest menu's file |

Each entry under `buttons` takes:

| Key | Meaning |
|---|---|
| `label` | The button's text, in MiniMessage, with the item's placeholders; empty uses the item's name |
| `image` | A texture of the Bedrock resource pack (`textures/items/emerald`), an `https` URL, or `none`; empty picks an icon for the item's material |
| `hidden` | `true` leaves the item out |
| `clicks` | For an item that does different things per click in the chest: the choices offered after pressing it, each under the click it stands for (`LEFT`, `RIGHT`, `SHIFT_LEFT`, `SHIFT_RIGHT`) |

A button that opens nothing, such as the sort, brings the form back once it is done, as a chest would
stay open. The button that leads back from a list of choices is the `bedrockBack` message. The files
follow the language and are read again on reload; one that cannot be read gives Bedrock players the
chest menu until it is fixed.

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

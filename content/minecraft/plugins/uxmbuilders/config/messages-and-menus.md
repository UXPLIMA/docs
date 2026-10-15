---
title: Messages and menus
order: 304
description: Where every line players read lives, how a message is delivered, and how menus are laid out.
icon: message-square
---

No text players see is written in the plugin's code. Every line is in `messages.yml` and the
`menu/*.yml` files, or their copies under `lang/<code>/` for another [language](../features/languages.md).

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
| `fadeIn`, `stay`, `fadeOut` | Ticks, for `TITLE` and `BOSSBAR` |
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
| `BOSSBAR` | A temporary boss bar, hidden after `stay` |
| `CONSOLE` | The server log, as plain text |
| `NONE` | Nothing; the way to silence a message without deleting it |

A misspelled `type` is logged and treated as `CHAT` rather than breaking the file.

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

`size` is the number of rows. `type` is the material, `model` the custom model data, `slot` the
position. Some items take their material from elsewhere (a package's icon) and use only the name and
lore from the file; the header comment of each file says which, and which placeholders each item gets.

## Updating

Bundled files are the fallback for anything your copy lacks. A line added in an update appears even
though your file is older, and deleting a line from your file brings the shipped text back. To change a
line, edit it; to silence a message, set its `type` to `NONE`.

Related: [Languages](../features/languages.md), [Placeholders](../placeholders.md)

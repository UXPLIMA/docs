---
title: Dialogs
order: 1346
description: Short menus and text questions as Java dialog screens on Spigot 1.21.6+ and Paper 1.21.7+.
icon: message-square
---

The dialogs module shows chosen menus as Java dialog screens with one button per choice, and asks for text
(a land name, an amount) in a dialog box instead of chat. On a server without a dialog API it starts and does
nothing, so menus stay chests and questions stay in chat.

Module `dialogs` · enabled by default · `modules/dialogs/config.yml`

## Settings

| Key | Default | What it does |
|---|---|---|
| `timeout-seconds` | `300` | How long a dialog's buttons keep working on Paper (3.x `dialog.timeoutSeconds`). Spigot does not use it. |
| `bedrock-fallback` | `true` | Keep dialogs from Bedrock players (Geyser or Floodgate). They get Bedrock forms with the [Bedrock](bedrock.md) module, or chests. |
| `text-input` | `true` | Ask for text with a dialog instead of chat. |
| `menus.enabled` | `true` | Menus whose file says `dialog: enabled: true` show as a dialog. |
| `menus.all` | `false` | Every menu as a dialog, whatever its file says. Not recommended: long lists, maps and storage work better as chests. |

The module's `lang/en/messages.yml` holds `menu.close` (the close button) and `menu.page` (`Page <page> of
<pages>`).

## Which players get a dialog

| Server | Result |
|---|---|
| Paper or Folia 1.21.7 and newer | Paper's dialog API. |
| Spigot 1.21.6 and newer | Spigot's dialog API. |
| Anything older | No dialogs; the log says so once at start. |

A player gets a dialog only when the server has one of these APIs, their client is 1.21.6 or newer (read from
ViaVersion when it is installed), and they are not a Bedrock player while `bedrock-fallback` is on. Everyone
else sees the chest menu or the chat question.

## Menus as dialogs

A menu shows as a dialog when its file in `lang/<code>/menus/` has a `dialog:` block with `enabled: true`. Two
menus ship that way: the invite answer menu of [Invites](invites.md) and the member menu of [Lands](lands.md).

```yaml
dialog:
  enabled: true        # default false: the menu stays a chest
  columns: 2           # buttons per row
  button-width: 150    # 1 to 1024, 0 for the client default
  body:
    - '<gray>Pick one'
  exit: '<red>Close'   # empty: menu.close from this module
```

Every clickable item of the menu becomes a button with its name as the label and its lore as the tooltip.
Decoration items are left out. See [Menus](../configuration/menus.md) for the rest of a menu file.

## Notes

- Two server builds are covered by one jar: the Spigot screen code ships as ordinary classes and the Paper
  screen code as a jar inside the plugin, loaded only on Paper. Neither is touched on a server that lacks
  its API.
- A dialog opens one tick after the click that asked for it, and closes any open chest first.
- Without ViaVersion the server cannot tell an older client apart, and assumes it matches the server.
- `bedrock-fallback: false` sends dialogs to Bedrock players too, whatever Geyser makes of them. Leave it on
  unless you have checked that your Geyser shows dialogs.

Related: [Bedrock](bedrock.md), [Menus](../configuration/menus.md), [Invites](invites.md), [Lands](lands.md)

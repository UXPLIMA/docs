---
title: Menus
order: 145
description: Where menu files live, how to copy one out of the jar to edit it, and the chest, dialog and Bedrock variants.
icon: layout-grid
---

Every menu is a YAML file in the jar. The plugin does not write menu files to disk: to change one, you copy it
into the plugin folder, and from then on your copy is used instead of the jar's.

## Where menus live

| Owner | Jar path | Your copy |
|---|---|---|
| A module | `uxm/modules/<id>/lang/<code>/menus/<menu>.yml` | `plugins/uxmClaims/modules/<id>/lang/<code>/menus/<menu>.yml` |
| A module, Bedrock look | `uxm/modules/<id>/lang/<code>/menus/bedrock/<menu>.yml` | `plugins/uxmClaims/modules/<id>/lang/<code>/menus/bedrock/<menu>.yml` |
| The kernel (setup menus) | `uxm/kernel/lang/<code>/menus/<menu>.yml` | `plugins/uxmClaims/lang/<code>/menus/<menu>.yml` |

A menu named `lands:main` is the file `main.yml` of the `lands` module. For a viewer, the plugin reads the file
on disk in their language, then the jar copy in that language, then the jar's English copy.

## Copying a menu to edit it

1. Open the uxmClaims jar with any zip tool.
2. Find the menu under `uxm/modules/<id>/lang/<code>/menus/` (use `en` when your language has no copy).
3. Copy it to `plugins/uxmClaims/modules/<id>/lang/<code>/menus/`, with the same file name, in your server
   language's folder.
4. Edit it and run `/uxm reload`. Open chest menus close, and the next one opens from your file.

Your copy replaces the jar copy as a whole, so copy the full file rather than writing only the keys you change.
On each start the plugin adds keys a new version introduced and updates texts you never edited, the same way
it does for language files (see [Languages](languages.md)).

## A chest menu file

```yaml
config-version: 1
title: '<dark_gray>Lands</dark_gray>'
type: chest
rows: 3
auto-slots: [14, 15, 16]
items:
  claim:
    action: lands:claim-here
    slot: 10
    material: grass_block
    glow: true
    name: '<green><bold>Claim This Chunk</bold></green>'
    lore:
      - '<gray>Create a land here.</gray>'
decoration:
  border:
    slots: [0-9, 17-21, 23-26]
    material: black_stained_glass_pane
    name: ' '
```

| Key | What it does |
|---|---|
| `title` | Menu title, MiniMessage |
| `type` | `chest` |
| `rows` | 1 to 6 (`size` is read too) |
| `items.<key>` | Clickable items, each with `action` and `arguments` |
| `decoration.<key>` | Items without an action |
| `filler` | One item for every empty slot |
| `entry-slots`, `entry` | The slots and the look of list rows (lands, members, warps) |
| `auto-slots` | Slots where other modules add their buttons |
| `buttons.<key>` | Extra buttons |
| `dialog` | How the menu shows as a dialog, see below |

Item keys: `slot` or `slots` (numbers and ranges such as `[0, 10-16]`), `material`, `name`, `lore`, `amount`,
`model` (custom model data), `glow`, `texture` (player head), `action`, `arguments`, `visible-if`,
`when-missing` (`hide` or `filler`: what to draw when the action is not available), `player-commands` and
`console-commands`. Unknown keys are ignored. Lore lines wrap at `menus.lore-width` characters, see
[Kernel config](kernel.md).

## Dialogs

With the [Dialogs](../modules/dialogs.md) module on (Spigot 1.21.6+, Paper 1.21.7+), a menu whose file sets
`dialog.enabled: true` shows as a list of buttons in a Java dialog instead of a chest. Menus are chests by
default, because long lists, maps and storage work better that way.

| Key | What it does |
|---|---|
| `dialog.enabled` | `true` shows this menu as a dialog |
| `dialog.columns` | Buttons per row |
| `dialog.button-width` | Button width, `0` for the default |
| `dialog.body` | Lines shown above the buttons |
| `dialog.exit` | Label of the exit button |

`menus.all: true` in `modules/dialogs/config.yml` shows every menu as a dialog whatever its file says.

## Bedrock forms

With the [Bedrock](../modules/bedrock.md) module on, Geyser and Floodgate players get each menu as a Bedrock
form made from the chest menu's clickable items, in slot order. The file under `menus/bedrock/<menu>.yml` only
changes how they look:

| Key | What it does |
|---|---|
| `enabled` | `false` gives Bedrock players the chest menu here too |
| `title` | Form title; empty for the chest menu's title |
| `content` | Lines above the buttons |
| `back` | Label of the button that goes back from a list of choices |
| `buttons.<item key>.label` | Button text; empty for the item's name. List rows use the key `entry` |
| `buttons.<item key>.image` | A resource pack texture, an https URL, or `none`; empty for an icon from the material |
| `buttons.<item key>.hidden` | `true` leaves the item out |
| `buttons.<item key>.clicks` | Choices offered after pressing (`LEFT`, `RIGHT`, `SHIFT_LEFT`, `SHIFT_RIGHT`) with their labels |
| `grid-radius` | Chunk map forms only: chunks listed around the middle |

Without an override file every clickable item shows with its own name. Each shipped Bedrock file lists the item
keys of its menu in its header comment.

## Notes

- A new item that an update adds to a menu is added to your copy at start-up, at the slot the jar gives it;
  check your layout after an update if you moved items around.
- Menus edited in 3.x are written to disk by the migration with your texts in the 4.0 layout, see
  [Behaviour changes](../upgrading/behaviour-changes.md).
- The setup menu (`/uxm setup`) has no Bedrock form yet.

Related: [Languages](languages.md), [Dialogs](../modules/dialogs.md), [Bedrock](../modules/bedrock.md)

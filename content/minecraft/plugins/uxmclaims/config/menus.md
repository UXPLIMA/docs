---
title: Menu layouts
order: 507
description: The 28 menu files, their shared structure, and how to re-skin a screen.
icon: layout-grid
---

Every screen is a file in `menu/`: 21 chest menus and 7 text-input screens. The Bedrock forms built from
the chest menus live in `menu/bedrock/` and have a format of their own, described in
[Bedrock menus](../menus/bedrock.md).

## The files

| Group | Chest menus | Text input |
|---|---|---|
| Personal | `common_main`, `common_list`, `common_selection`, `common_warp_list`, `common_invite_inbox`, `common_confirmation` | |
| Claim | `claim_main`, `claim_map`, `claim_flags`, `claim_vault`, `claim_time_adjust` | `claim_rename` |
| Members | `claim_member_list`, `claim_member_manage` | |
| Invites | `claim_invite_list` | `claim_invite_send` |
| Bans | `claim_ban_list` | `claim_ban_player` |
| Roles | `claim_role_list`, `claim_role_manage` | `claim_role_create`, `claim_role_rename` |
| Warps | `claim_warp_list`, `claim_warp_manage` | `claim_warp_create`, `claim_warp_rename` |
| Claim block | `claim_block`, `claim_block_change` | |

`/claim admin view <player>` reuses `common_list.yml`.

## A chest menu

```yaml
title: '<dark_gray>⚡ <white>%claim.name%</white></dark_gray>'
size: 6
type: 'CHEST'

items:
  changeFlag:
    type: 'OAK_SIGN'
    name: '<gold><bold>Flags</bold></gold>'
    glow: false
    amount: 1
    lore:
      - ''
      - '<gray>Configure claim settings</gray>'
    model: 0
    slot: 20

otherItems:
  border:
    type: 'BLACK_STAINED_GLASS_PANE'
    name: ' '
    slots: [ 0, 1, 2, 3, 4, 5, 6, 7, 8 ]
```

| Key | Meaning |
|---|---|
| `title` | The window title. MiniMessage, with placeholders. |
| `size` | Rows, not slots. `6` is a double chest. |
| `type` | The inventory type: `CHEST`, or a fixed-size type such as `HOPPER` or `DISPENSER`. |
| `items` | The buttons, keyed by the name the plugin looks them up by. |
| `otherItems` | Decoration. Any key works; these items do nothing when clicked unless you give them commands. |
| `itemSlots` | List screens only: where the generated entries go. |
| `dialog` | `common_confirmation` and `claim_time_adjust` only: the dialog that replaces the chest. |

Each item takes:

| Key | Meaning |
|---|---|
| `type` | Material. A type containing `HEAD` or `SKULL` becomes a player head. |
| `name` | Display name, MiniMessage |
| `lore` | Lines below it |
| `amount` | Stack size shown |
| `glow` | Enchantment glint |
| `model` | Custom model data |
| `texture` | Base64 head texture, for head types |
| `slot` | Where it sits, 0-indexed |
| `slots` | A list of slots, for the same item in several places |
| `playerCommands` | Commands the clicking player runs, without `/` |
| `consoleCommands` | Commands the console runs |

The commands run before the button's own action, so you can add a sound or a log line to any button.

## List screens

```yaml
itemSlots: [ 10, 11, 12, 13, 14, 15, 16, 19, 20, 21, 22, 23, 24, 25, 28, 29, 30, 31, 32, 33, 34 ]
```

The generated entries fill `itemSlots` in order, and `previousPage` and `nextPage` turn the page.
Shrinking the list makes a screen show fewer entries per page. In `claim_vault.yml` the list is the
storage itself: items can only be placed in those slots.

The flag, member and role editors take one item per entry, keyed by its name in camelCase
(`fireSpread` for `FIRE_SPREAD`). Those items carry no `slot`; delete one and that flag or permission
is left off the screen.

Some items exist in more than one form and the plugin picks by state:

```yaml
ownedClaimItem:          # normal
ownedClaimItemSelected:  # currently selected: glow true, a check mark in the name
```

Keep both keys. Deleting the variant leaves that state unrendered.

## Text input

The seven input files have no items. They choose how the player types:

```yaml
type: 'DIALOG'       # DIALOG, ANVIL or CHAT
fallback: 'ANVIL'    # ANVIL or CHAT, where a dialog cannot be shown
title: '<dark_gray>⚡ <white>Rename Claim</white></dark_gray>'
placeholder: 'Enter a new name'

dialog:
  title: ''          # empty: the title above
  body:
    - '<gray>Pick a new name for your claim.</gray>'
  label: '<gray>Claim name</gray>'
  maxLength: 32
  toggle: ''         # an on/off switch under the text box, empty hides it
  toggleDefault: false
  submit: '<green>Confirm</green>'
  cancel: '<red>Cancel</red>'
```

| `type` | What opens |
|---|---|
| `DIALOG` | A dialog with a text box. Where it cannot be shown, `fallback` opens instead. |
| `ANVIL` | An anvil window; `title` is its title and `placeholder` the starting text. |
| `CHAT` | A chat prompt with a 30-second countdown; `title` and `placeholder` are the title and subtitle, and `%time%` is the seconds left. |

A dialog needs Spigot 1.21.6, or Paper and Folia 1.21.7, or newer on the server, and a 1.21.6 or
newer client. Through ViaVersion, older clients get the fallback. With
[Bedrock forms](../menus/bedrock.md) on, Bedrock players get a form whatever the `type`. If the anvil
library does not support your server version, the console warns once and chat is used.

The shipped files use `DIALOG`, falling back to `ANVIL` for `claim_rename`, `claim_warp_create` and
`claim_warp_rename`, and to `CHAT` for the rest. Only `claim_warp_create` reads the toggle: it sets
whether the new warp is public, and without a dialog the warp gets `toggleDefault`.

## Dialog screens

`common_confirmation.yml` and `claim_time_adjust.yml` carry a `dialog` section with `enabled`, `title`,
`body`, `submit` and `cancel`. With `enabled: false`, or where dialogs cannot be shown, the chest opens.
The time dialog adds:

| Key | Meaning |
|---|---|
| `sliders.<unit>.label` | Slider label. Units are `days`, `hours`, `minutes` and `seconds`. |
| `sliders.<unit>.max` | The largest value. `0` hides the slider. |
| `confirm` | A second dialog showing `%time:totalMillis%` and `%float:cost%` before payment. `enabled: false` skips it. |

When the owner has a time limit, each slider offers no more than still fits.

The `dialog` section of [config.yml](config-yml.md) turns dialogs off everywhere and sets how long one
waits for an answer. That timeout applies on Paper and Folia, and changing it needs a restart.

## Rules

- **Item keys are looked up by name.** Rename `changeFlag` and that button disappears. Change its
  material, text, lore, slot and glow freely; leave the key alone.
- **`slot` is 0-indexed** and must fall inside `size × 9`.
- **A fixed item placed on a slot in `itemSlots`** covers the entry generated there.
- **Everything is MiniMessage.** Legacy `&` codes are not translated.
- **`/claim reload` rebuilds the menus**, Bedrock files included; no restart needed while you style.
  The exception is `common_selection.yml`, which is read only at startup.
- **Changing `language:` in config.yml** rewrites the menu texts you have not edited. See
  [config.yml](config-yml.md).

<Callout type="tip" title="Copy the folder before you start">

A missing menu file is only recreated when the server starts. Copy `menu/` before a redesign, so a
broken slot number is a file restore rather than an evening.

</Callout>

---
title: Menu layouts
order: 507
description: The 28 menu files, their shared structure, and how to re-skin a screen.
icon: layout-grid
---

Every screen is a file in `menu/`. They share one structure, so learning one teaches you all 28.

## The structure

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
      - ''
      - '<gold>▸ Click to configure</gold>'
    model: 0
    slot: 20
```

| Key | Meaning |
|---|---|
| `title` | The window title. MiniMessage, with placeholders. |
| `size` | Rows, not slots. `6` is a double chest. |
| `type` | The inventory type — `CHEST`, `HOPPER`, `DISPENSER` and so on. |
| `items` | The buttons, keyed by the name the plugin looks them up by. |

Each item takes:

| Key | Meaning |
|---|---|
| `type` | Material |
| `name` | Display name, MiniMessage |
| `lore` | Lines below it |
| `amount` | Stack size shown |
| `glow` | Enchantment glint |
| `model` | Custom model data |
| `slot` | Where it sits, 0-indexed |

## Paginated menus

List screens carry an extra key:

```yaml
itemSlots: [ 10, 11, 12, 13, 14, 15, 16, 19, 20, 21, 22, 23, 24, 25, 28, 29, 30, 31, 32, 33, 34 ]
```

`itemSlots` is where the generated entries go, in order. Everything not in that list is a fixed
button. Shrinking the list is how you make a list screen denser or sparser without touching code.

## State variants

Some items exist in more than one form and the plugin picks by state:

```yaml
ownedClaimItem:          # normal
ownedClaimItemSelected:  # currently selected — glow: true, a ✔ in the name
```

Keep both keys. Deleting the variant leaves that state unrendered.

## The files

| Group | Files |
|---|---|
| Personal | `common_main`, `common_list`, `common_selection`, `common_warp_list`, `common_invite_inbox`, `common_confirmation` |
| Claim | `claim_main`, `claim_map`, `claim_flags`, `claim_vault`, `claim_rename`, `claim_time_adjust` |
| Members | `claim_member_list`, `claim_member_manage` |
| Invites | `claim_invite_list`, `claim_invite_send` |
| Bans | `claim_ban_list`, `claim_ban_player` |
| Roles | `claim_role_list`, `claim_role_manage`, `claim_role_create`, `claim_role_rename` |
| Warps | `claim_warp_list`, `claim_warp_manage`, `claim_warp_create`, `claim_warp_rename` |
| Claim block | `claim_block`, `claim_block_change` |

## Rules

- **Item keys are looked up by name.** Rename `changeFlag` and that button disappears. Change its
  material, text, lore, slot and glow freely; leave the key alone.
- **`slot` is 0-indexed** and must fall inside `size × 9`.
- **A slot listed in `itemSlots` and also given to a fixed item** will be overwritten by the generated
  entry.
- **Everything is MiniMessage.** Legacy `&` codes are not translated.
- **`/claim reload` rebuilds the menus** — no restart needed while you are styling.

<Callout type="tip" title="Copy the folder before you start">

`menu/` is 28 files of layout you cannot regenerate without deleting them and restarting. Copy it
somewhere before a redesign, so a broken slot number is a file restore rather than an evening.

</Callout>

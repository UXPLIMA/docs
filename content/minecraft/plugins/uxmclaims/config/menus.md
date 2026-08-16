---
title: Menu Customization
order: 290
icon: layout-grid
---

## Menu Files

The plugin has 27 menu files:

| File                      | What It Is           |
|---------------------------|----------------------|
| `common_main.yml`         | Main /claim menu     |
| `common_list.yml`         | Claim list           |
| `common_invite_inbox.yml` | Invite inbox         |
| `claim_main.yml`          | Claim management     |
| `claim_member_list.yml`   | Member list          |
| `claim_member_manage.yml` | Member management    |
| `claim_ban_list.yml`      | Ban list             |
| `claim_ban_player.yml`    | Ban a player         |
| `claim_invite_list.yml`   | Sent invites         |
| `claim_invite_send.yml`   | Send invite          |
| `claim_flags.yml`         | Flag settings        |
| `claim_map.yml`           | Claim map            |
| `claim_vault.yml`         | Vault screen         |
| `claim_block.yml`         | Block selector       |
| `claim_block_change.yml`  | Change block         |
| `claim_time_adjust.yml`   | Time extension       |
| `claim_rename.yml`        | Rename claim         |
| `claim_role_list.yml`     | Role list            |
| `claim_role_manage.yml`   | Role permissions     |
| `claim_role_create.yml`   | Create role          |
| `claim_role_rename.yml`   | Rename role          |
| `claim_warp_list.yml`     | Warp list            |
| `claim_warp_manage.yml`   | Warp management      |
| `claim_warp_create.yml`   | Create warp          |
| `claim_warp_rename.yml`   | Rename warp          |
| `common_confirmation.yml` | Confirmation dialogs |
| `common_warp_list.yml`    | Public warps         |

---

## Menu Structure

Every menu file has a similar structure:

```yaml
title: '<dark_gray>Menu Title</dark_gray>'
size: 3
type: 'CHEST'

items:
  buttonName:
    type: 'MATERIAL_NAME'
    name: '<color>Button Text</color>'
    amount: 1
    slot: 0
    glow: false
    lore:
      - '<gray>Description line 1</gray>'
      - '<gray>Description line 2</gray>'

otherItems:
  filler:
    type: 'GRAY_STAINED_GLASS_PANE'
    name: ' '
    slots: [ 1, 2, 3, 4, 5 ]
```

---

## Main Settings

### title

The menu title shown at the top. We recommend using a **premium theme** with the ⚡ symbol:

```yaml
title: '<dark_gray>⚡ <white>Menu Name</white></dark_gray>'
```

Supports MiniMessage colors.

### size

Number of rows (1-6).

```yaml
size: 3  # 3 rows = 27 slots
size: 6  # 6 rows = 54 slots
```

### type

Always use `'CHEST'`.

---

## Item Settings

### type

Minecraft material name (UPPERCASE).

```yaml
type: 'DIAMOND_SWORD'
type: 'PLAYER_HEAD'
type: 'GRASS_BLOCK'
```

### name

Display name with colors.

```yaml
name: '<green><bold>Click Me!</bold></green>'
```

### amount

Stack size shown.

```yaml
amount: 1
amount: 64
```

### slot

Position in the menu (0-indexed).

```
Row 1: 0  1  2  3  4  5  6  7  8
Row 2: 9  10 11 12 13 14 15 16 17
Row 3: 18 19 20 21 22 23 24 25 26
...
```

### slots

For items that appear in multiple locations:

```yaml
slots: [ 0, 1, 2, 3, 4 ]
```

### glow

Enchantment glint effect.

```yaml
glow: true
glow: false
```

### lore

Description lines.

```yaml
lore:
  - ''
  - '<gray>Click to open</gray>'
  - '<yellow>Cost: $100</yellow>'
```

### model

Custom model data for resource packs.

```yaml
model: 0  # No custom model
model: 1001  # Custom model ID
```

---

## Items vs OtherItems

| Section      | Purpose                                 |
|--------------|-----------------------------------------|
| `items`      | Clickable buttons with actions          |
| `otherItems` | Decoration (glass panes, info displays) |

---

## Common Customizations

### Changing Button Appearance

```yaml
items:
  myButton:
    type: 'EMERALD'  # Change material
    name: '<green>Custom Name</green>'  # Change name
    glow: true  # Add glow
    lore:
      - '<gray>New description</gray>'
```

### Moving Buttons

Change the `slot` value:

```yaml
slot: 13  # Center of row 2
```

### Adding Filler

```yaml
otherItems:
  border:
    type: 'BLACK_STAINED_GLASS_PANE'
    name: ' '
    slots: [ 0, 1, 2, 6, 7, 8 ]
```

### Standard Back Button

All menus use a **BARRIER** block for back/close buttons with consistent styling:

```yaml
items:
  back:
    type: 'BARRIER'
    name: '<red>Back</red>'
    glow: false
    amount: 1
    lore: []
    slot: 49
```

---

## Using Placeholders

Many items support placeholders:

```yaml
lore:
  - '<gray>Owner: %claim.owner.name%</gray>'
  - '<gray>Size: %count:claim.chunks% chunks</gray>'
  - '<gray>Created: %date:claim.creationDate%</gray>'
```

---

## Color Codes

Use MiniMessage format:

| Tag              | Color        |
|------------------|--------------|
| `<red>`          | Red          |
| `<green>`        | Green        |
| `<blue>`         | Blue         |
| `<yellow>`       | Yellow       |
| `<gold>`         | Gold         |
| `<gray>`         | Gray         |
| `<white>`        | White        |
| `<dark_gray>`    | Dark Gray    |
| `<aqua>`         | Aqua         |
| `<light_purple>` | Light Purple |

**Formatting:**

- `<bold>` - Bold
- `<italic>` - Italic
- `<reset>` - Reset formatting

---

## Tips

1. **Backup first** - Copy files before editing
2. **Validate YAML** - Use an online YAML validator
3. **Test in-game** - Use `/claim reload` after changes
4. **Keep slots organized** - Plan your layout
5. **Use consistent colors** - Match your server theme

---

## Input Menus

Some menus require text input from players (e.g., naming a claim, renaming a warp). These input menus are configured in separate files and support two different types.

### Input Menu Files

| File                    | Purpose                  |
|-------------------------|--------------------------|
| `claim_rename.yml`      | Rename a claim           |
| `claim_warp_rename.yml` | Rename a warp            |
| `claim_warp_create.yml` | Create a new warp        |
| `claim_role_create.yml` | Create a new role        |
| `claim_role_rename.yml` | Rename a role            |

### Configuration Structure

```yaml
# The type of input menu to use.
# Available options: ANVIL, CHAT
type: ANVIL

# The title of the input menu.
# For ANVIL: appears at the top of the anvil window.
# For CHAT: appears as the main title, use %time% for countdown.
title: '<dark_gray>Rename Your Claim</dark_gray>'

# The subtitle/placeholder text.
# For ANVIL: acts as a placeholder in the text field.
# For CHAT: appears as subtitle, use %time% for countdown.
placeholder: '<gray>Enter a new name</gray> <yellow>%time%s</yellow>'
```

### Input Menu Types

| Type    | Description                                           |
|---------|-------------------------------------------------------|
| `ANVIL` | Opens an anvil GUI for text input (default)           |
| `CHAT`  | Player types in chat with a countdown timer           |

### Settings Explained

| Setting       | ANVIL Mode                    | CHAT Mode                          |
|---------------|-------------------------------|------------------------------------|
| `type`        | Set to `ANVIL`                | Set to `CHAT`                      |
| `title`       | Title at top of anvil window  | Main title shown on screen         |
| `placeholder` | Placeholder text in input     | Subtitle shown below title         |

### Using %time% Placeholder

The `%time%` placeholder shows the remaining seconds for input (used primarily in CHAT mode):

```yaml
placeholder: '<gray>Enter a new name</gray> <yellow>%time%s</yellow>'
```

This displays: "Enter a new name **30s**" (counting down)

---


## Troubleshooting

### Menu won't open

- Check YAML syntax (spaces, not tabs)
- Look for console errors

### Items not showing

- Verify slot numbers
- Check material names are correct

### Colors not working

- Use MiniMessage format, not legacy `&` codes
- Close all tags properly

---

## Next Steps

- [🛡️ Protection Overview](../protection/overview.md) - How protection works
- [💾 Database Setup](../database/sqlite.md) - Configure database

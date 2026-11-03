---
title: Menu files
order: 605
description: Every key a window file takes, the tile tokens, the lists, clicks and actions uxmCrates adds.
icon: layout-template
---

Every window is a file in `menus/`. Move a slot, change a material or a sound, and the plugin draws what you wrote.
`/uxmcrates reload` reads the files again. A file that is missing is written back at the next start; a file that cannot
be read is reported in the console. The windows are described under [Menus](../menus/overview.md).

## The window

| Key | Meaning |
|---|---|
| `title` | The window title: `@key` for a line of the messages files, or your own words |
| `rows` | Chest height, `1` to `6` |
| `layout` | Optional: one string per row, a character per slot, matched by `layout-char` on a tile |
| `open-actions` | Actions run when the window opens, such as its sound |
| `placeholders { }` | Values the window reads, such as `tile-colour` |

## A tile

Each entry under `items { }` is one tile, keyed by a name you choose.

| Key | Meaning |
|---|---|
| `slot`, `slots` | Where it sits: a number, or a list mixing numbers and ranges such as `"10-16"` |
| `layout-char` | The character of `layout` this tile fills |
| `priority` | Which tile wins a slot two of them ask for; the highest is drawn last |
| `material` | The item: a material name, or a `%token%` the window fills in |
| `name` | `@key`, your own words, or `" "` for a tile whose title is drawn by its lore |
| `lore` | Lines under the name, or one `%tile_...%` token |
| `type` | `previous` or `next` turns the page of the window's list |
| `click { }` | What each gesture does: `left`, `right`, `shift_left`, `shift_right`, `middle` and more |
| `list { source, template }` | Fills the tile's slots from a list the plugin provides |
| `decor { }` | `amount`, `glow`, `model-data`, `item-model`, `hide-tooltip` |

## Tile colours

A window's `placeholders` block names the colour of each kind of tile, so tiles of one kind look alike and tiles of
another kind stand apart:

```hocon
placeholders {
  tile-colour    = "5"
  keys-colour    = "2"
  pending-colour = "4"
}
```

A value is a place on the theme's colour wheel as a number, a gradient of [theme.conf](theme.md), or a role such as
`good` or `money`. Without one, the title takes the theme's header gradient.

## The tokens

A `%tile_...%` token in `lore` is a whole house tooltip drawn by the plugin. The words are in the `menu` block of the
messages files.

| Window | Tokens |
|---|---|
| `crates.conf` | `%crate_icon%`, `%crate_keys%`, `%tile_crate%`, `%tile_keys_button%`, `%tile_pending_button%` |
| `preview.conf` | `%reward_icon%`, `%reward_glow%`, `%tile_reward%` |
| `keys.conf` | `%key_icon%`, `%tile_key%` |
| `pending.conf` | `%pending_icon%`, `%tile_pending%` |
| `milestones.conf` | `%milestone_at%`, `%tile_milestone%` |
| `cost.conf` | `%tile_cost%` |
| `bulk.conf` | `%result_icon%`, `%result_count%`, `%tile_result%` |
| `reroll.conf` | `%reroll_icon%`, `%reroll_left%`, `%tile_reroll_look%`, `%tile_reroll_accept%`, `%tile_reroll_again%` |
| `log.conf` | `%tile_log%` |

Window placeholders that pick an icon: `icon-menu` (the crate list icon of a `menu` crate), `icon-command` (a reward
with no item), `icon-reward` (the reroll look button), `gap-material` (the pane that fills the rest of a group's page
in the preview).

## Lists

| Source | Window | Entries |
|---|---|---|
| `crates:crates` | `crates.conf` | Every crate |
| `crates:rewards` | `preview.conf` | The rewards of the crate, a page per group |
| `crates:keys` | `keys.conf` | Every key not hidden |
| `crates:pending` | `pending.conf` | The player's waiting rewards |
| `crates:milestones` | `milestones.conf` | The rungs of the ladder |
| `crates:cost` | `cost.conf` | The cost lines |
| `crates:results` | `bulk.conf` | The rewards of a bulk run |
| `crates:log` | `log.conf` | The log rows that match |
| `crates:editor-crates`, `crates:editor-keys` | `editor.conf`, `editor-keys.conf` | Crates and keys in the editor |

The number of slots is the size of a page.

## Actions

Besides `close`, `open:<window>`, `command:<line>`, `message:<line>` and `sound:<name> <volume> <pitch>`:

| Action | Does |
|---|---|
| `crates:open` | From a crate tile: opens it |
| `crates:preview` | From a crate tile: its preview |
| `crates:open-list` | The crate list |
| `crates:open-keys` | The keys window |
| `crates:open-pending` | The waiting rewards |
| `crates:key-left`, `crates:key-right` | From a key tile: runs the key's `menu.click` lines |
| `crates:claim` | From a waiting reward: takes it |
| `crates:milestone-claim` | From a rung: takes it |
| `crates:reroll-look`, `crates:reroll-accept`, `crates:reroll-again` | The reroll buttons |
| `crates:editor-home`, `crates:editor-open`, `crates:editor-open-keys` | Editor navigation |
| `crates:editor-properties`, `crates:editor-rewards`, `crates:editor-new` | From an editor crate tile, and the new crate button |
| `crates:editor-key-properties`, `crates:editor-key-new` | From an editor key tile, and the new key button |

The list and back buttons use `crates:open-list` rather than `open:crates`, because those windows read their rows off
the server thread before they open.

## The editor's property windows

`editor-crate.conf`, `editor-reward.conf`, `editor-key.conf`, `editor-list.conf` and `editor-reward-list.conf` take
slot lists instead of tiles: `property-slots`, `entry-slots` or `content-slots`, and `back-slot`, `delete-slot`,
`add-slot`, `create-slot`, `previous-slot`, `next-slot`. The buttons themselves are built by the plugin.

## Notes

- **Keep a filler.** The grey pane under every free slot is what makes a handful of icons read as a menu.
- **A slot past the window's last** is dropped.
- `/uxmcrates doctor` counts the windows that were read.

Related: [Menus](../menus/overview.md), [Messages](messages.md), [theme.conf](theme.md)

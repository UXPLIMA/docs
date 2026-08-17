---
title: Actions
order: 44
description: Every action type a menu item can run, and what each one takes.
icon: play
---

An action is a string that starts with a tag in square brackets and takes the rest of the line as
its argument. Menu items carry a list of them, and `settings.default-click-actions` is the list
that runs when a player clicks the NPC itself.

```yaml
default-click-actions:
  - "[menu] main"
  - "[sound] ENTITY_ALLAY_AMBIENT_WITHOUT_ITEM"
```

Actions run in order, and an action that fails stops the ones after it — which is what makes
`[check_permission]` and `[cooldown]` useful as guards.

## Reference

| Action | Argument | What it does |
|---|---|---|
| `[player]` | command | Runs the command as the player |
| `[console]` | command | Runs the command as the console |
| `[player_message]` | text | Sends the text to the player |
| `[console_message]` | text | Prints the text to the console |
| `[menu]` | `main`, `glow`, `appearance`, `shop` | Opens one of the plugin's menus |
| `[close]` | — | Closes the open inventory |
| `[modify_appearance]` | entity type | Changes the NPC's entity type |
| `[glow_color]` | colour name | Sets the glow colour |
| `[glow_status]` | — | Toggles the glow on or off |
| `[buy]` | — | Buys an NPC, charging the configured price |
| `[check_permission]` | `<node> [message]` | Continues if the player has the node; otherwise sends the message and stops |
| `[cooldown]` | cooldown name | Continues if the cooldown has expired; otherwise stops |
| `[sound]` | sound name | Plays a sound to the player |
| `[wait]` | ticks | Delays the actions after it |
| `[ask_question]` | — | Starts an assistant question, see [AI Assistant](../features/ai-assistant.md) |

## Guarding a button

```yaml
actions:
  - "[check_permission] uxmhelpernpc.usenotch <red>You do not own this skin."
  - "[cooldown] change-glowing-cooldown"
  - "[glow_color] BLUE"
  - "[sound] UI_BUTTON_CLICK"
```

The permission check runs first and stops the rest if it fails, the cooldown check stops it if the
player is too early, and only then does the colour change.

<Callout type="note" title="Entity and sound names are Bukkit's">

`[modify_appearance]` takes an `EntityType` and `[sound]` takes a `Sound`, both spelled exactly as
Bukkit spells them. A wrong name is logged as a warning and the action is skipped rather than
crashing the menu.

</Callout>

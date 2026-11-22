---
title: Bedrock
order: 1347
description: Menus, text questions and yes-or-no confirmations as Bedrock forms for Geyser and Floodgate players.
icon: smartphone
---

The bedrock module shows uxmClaims menus to Bedrock players as native forms instead of chest menus. Text
questions (a land name, an amount) become a form with one input box, and confirmations become a form with two
buttons. Java players are not affected.

Module `bedrock` · enabled by default · `modules/bedrock/config.yml` · needs Floodgate or Geyser-Spigot

## Settings

| Key | Default | What it does |
|---|---|---|
| `menus` | `true` | Shows menus as Bedrock forms. Off: Bedrock players get the chest menu, which Geyser translates. |
| `text-input` | `true` | Asks for text with a form instead of chat. |
| `confirmations` | `true` | Asks yes-or-no questions (deleting a land, leaving a town as its mayor) with a two-button form. The chat line with `/claim confirm` is sent as well. |
| `map-grid-radius` | `1` | How many chunks around you a chunk map form lists: `1` is a 3x3 area (9 buttons), `2` is 5x5, `-1` lists the whole chest map. |
| `head-image-url` | `https://mc-heads.net/avatar/%name%/64` | Image the Bedrock client downloads for player head buttons, `%name%` being the player. Empty: a built-in friend icon, nothing is downloaded. |
| `head-name-keys` | `member.name`, `player.name`, `target.name`, `name` | Which of a head button's placeholders holds the player name. The first one found wins. |
| `prefer-floodgate-api` | `true` | Asks Floodgate first whether a player is on Bedrock and sends forms through it. On a backend server Geyser often runs only on the proxy, so Floodgate is the reliable source. |

## How a menu becomes a form

Every clickable item of the chest menu becomes one button, in slot order. Decoration and filler are left out.
The button shows the item's name and an icon matched to its material; materials without a known Bedrock texture
get no icon. A button whose item offers several clicks (left, right, shift) asks which one in a second form,
whose back button returns to the menu.

To change how one menu looks on Bedrock, put a file at
`modules/<module>/lang/<language>/menus/bedrock/<menu>.yml`. Several modules ship one (bans, claim-block,
expiry, invites and others). List only the buttons that should look different:

```yaml
enabled: true          # false: Bedrock players get the chest menu here
title: ''              # empty: the chest menu's title
content: []            # lines above the buttons
back: '<gray>« Back</gray>'
grid-radius: 1         # chunk maps only, overrides map-grid-radius
buttons:
  entry:               # the chest item key; list rows use 'entry'
    label: '<white>%ban.player%'
    image: 'https://mc-heads.net/avatar/%ban.player%/64'
    hidden: false
    clicks:
      PRIMARY: 'Open'
      SECONDARY: 'Remove'
```

| Button key | What it does |
|---|---|
| `label` | Button text in MiniMessage. The item's placeholders work. Empty: the item's name. |
| `image` | A Bedrock texture path such as `textures/items/compass_item`, an `http(s)` URL, or `none`. Empty: the automatic icon. |
| `hidden` | `true` leaves the item out. |
| `clicks` | The clicks the button offers, with labels: `PRIMARY`, `SECONDARY`, `SHIFT_PRIMARY`, `SHIFT_SECONDARY` (or `LEFT`, `RIGHT`, `SHIFT_LEFT`, `SHIFT_RIGHT`). Two or more ask which one first. |

The 3.x camelCase keys (`detailFor`, `gridRadius`) are read too. A broken file is logged once and the menu falls
back to the automatic form.

## Notes

- Bedrock players are detected only through the Floodgate or Geyser API, never by a name prefix.
- If Geyser or Floodgate is too old to have the Geyser base API and Cumulus, the module logs a warning and Bedrock
  players get chests and chat.
- Bedrock players never get Java [Dialogs](dialogs.md), whether this module is on or not.
- Warp confirmations (deleting a warp, the warning before teleporting into a PvP land) still ask Bedrock players
  in chat. Some town and nation confirmations show their "Type /town confirm" line inside the form.
- The looks of 3.x Bedrock menu files (labels, clicks, content) are not carried over by the upgrade; only the
  claim map's `gridRadius` becomes `map-grid-radius`.
- The `/uxm setup` menu has no Bedrock form yet.

Related: [Dialogs](dialogs.md), [Menus](../configuration/menus.md), [Integrations](../integrations.md)

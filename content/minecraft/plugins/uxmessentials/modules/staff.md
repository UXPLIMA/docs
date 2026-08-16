---
title: Staff
order: 9240
description: An on-duty mode that swaps a moderator's inventory for a gadget hotbar and hands it back on exit.
---

`/staffmode` flips a moderator between playing and being on duty. Entering saves their real inventory to the
database, replaces it with the gadget hotbar, and applies vanish, flight and night vision as configured; leaving
clears the gadgets and restores the saved inventory untouched. Because the saved items live server-side they
survive a relog, a restart and a world rollback.

Module `staff` · enabled by default · `modules/staff/config.conf`

## Commands

{/* generated:commands */}
| Command | What it does | Permission |
|---|---|---|
| `/staffchat` (`/sc`) | Send a message on the staff channel. | `uxmessentials.staff.chat` |
| `/stafflist` | List who is currently online and on staff. | `uxmessentials.staff.list` |
| `/staffmode` | Toggle staff mode. | `uxmessentials.staff.mode` |
{/* /generated */}

## Permissions

{/* generated:permissions */}
| Node | Default | Grants |
|---|---|---|
| `uxmessentials.module.staff` | op | Hot-reload / inspect the staff module (staff mode, its loadout and staff chat). |
| `uxmessentials.staff.chat` | op | /staffchat (alias /sc) to send and receive lines on the staff-only chat channel. |
| `uxmessentials.staff.list` | op | /stafflist to open the online-staff GUI (vanish-aware) and click a head to teleport to that staff member. |
| `uxmessentials.staff.mode` | op | /staffmode [player] to flip into staff mode: your real loadout is saved and swapped for the gadget hotbar (and you vanish); leaving restores it. The EXAMINE gadget opens a player's inventory. |
{/* /generated */}

## Settings

{/* generated:settings */}
| Key | Default | What it does |
|---|---|---|
| `vanish-on-enter` | `true` | Whether entering staff mode also vanishes the staff member. Honoured when the presence module is enabled; with presence off this does nothing (no second vanish state is kept). |
| `flight-on-enter` | `true` | Whether entering staff mode grants flight. The player's real pre-mode flight allowance is captured, so leaving reverts it exactly; a survival player loses the granted flight, a creative one keeps theirs. Staff need uxmessentials.playerstate.fly.allworlds to keep this flight in configured no-fly worlds. |
| `night-vision-on-enter` | `true` | Whether entering staff mode grants night vision. It is cleared along with the rest of the in-mode effects when the player's real loadout is restored on exit. |
| `gadgets.vanish.enabled` | `true` |  |
| `gadgets.vanish.slot` | `0` |  |
| `gadgets.vanish.material` | `"SLIME_BALL"` |  |
| `gadgets.vanish.name` | `"<accent>Vanish</accent>"` |  |
| `gadgets.examine.enabled` | `true` |  |
| `gadgets.examine.slot` | `1` |  |
| `gadgets.examine.material` | `"BOOK"` |  |
| `gadgets.examine.name` | `"<accent>Examine</accent>"` |  |
| `gadgets.freeze.enabled` | `true` |  |
| `gadgets.freeze.slot` | `2` |  |
| `gadgets.freeze.material` | `"PACKED_ICE"` |  |
| `gadgets.freeze.name` | `"<accent>Freeze</accent>"` |  |
| `gadgets.compass.enabled` | `true` |  |
| `gadgets.compass.slot` | `3` |  |
| `gadgets.compass.material` | `"COMPASS"` |  |
| `gadgets.compass.name` | `"<accent>Navigator</accent>"` |  |
| `gadgets.follow.enabled` | `true` |  |
| `gadgets.follow.slot` | `4` |  |
| `gadgets.follow.material` | `"LEAD"` |  |
| `gadgets.follow.name` | `"<accent>Follow</accent>"` |  |
| `follow.interval-ticks` | `10` |  |
| `staff-chat.receive-node` | `"uxmessentials.staff.chat"` | The permission node identifying the staff-chat audience: every online holder of this node receives staff chat. Defaults to the same node that gates sending, so anyone who can send also receives. |
{/* /generated */}

## Placeholders

{/* generated:placeholders */}
| Placeholder | Renders |
|---|---|
| `%uxmessentials_staff_count%` | The same connected-staff count, under the spelling a config may prefer. |
| `%uxmessentials_staff_mode%` | Whether the player is in staff mode (yes/no). |
| `%uxmessentials_staff_online%` | How many staff members are connected. |
{/* /generated */}

## Notes

- **Staff mode is not `/vanish`.** Vanish is a standalone invisibility toggle that never touches an inventory;
  staff mode is a whole mode built on top of it. Enter staff mode to moderate, leave it to play, and do not
  stack the two carelessly.
- **Always leave staff mode rather than logging out in it,** so the saved inventory comes back cleanly.
- **The hotbar only shows the gadgets you enable.** Leave `follow` off if silent shadowing is not something your
  team should have.
- **Point `staff-chat.receive-node` at the group you already use** so a new moderator joins the channel by being
  added to that group rather than by a second grant.
- **Alerts are opt-in per staff member**, so a moderator can go on duty without subscribing to every notice.

Related: [Moderation](moderation.md), [Vanish](vanish.md), [Security](security.md)

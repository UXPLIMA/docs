---
title: Vanish
order: 9290
description: Layered invisibility with see and use levels, fake join and quit lines, and cross-server sync.
icon: eye-off
---

`/vanish` hides a player from everyone who is not allowed to see hidden players: they drop off the tab list,
their join and quit lines can be faked, and the world stops noticing them. This module is the single authority
over vanish state, so messaging, nametags and staff mode all read the same flag rather than keeping copies.

Module `vanish` · enabled by default · `modules/vanish/config.conf`

## Commands

{/* generated:commands */}
| Command | What it does | Permission |
|---|---|---|
| `/vanish` (`/v`) | Become invisible to other players. | `uxmessentials.vanish.use` |
{/* /generated */}

## Permissions

{/* generated:permissions */}
| Node | Default | Grants |
|---|---|---|
| `uxmessentials.module.vanish` | op | Hot-reload / inspect the vanish module. |
| `uxmessentials.vanish.list` | op | /vanish list to see the hidden players you are permitted to see (scoped to your see level). |
| `uxmessentials.vanish.others` | op | /vanish \<player> to toggle another player's vanish. |
| `uxmessentials.vanish.persist` | op | Remain vanished across a relog instead of reappearing on join. |
| `uxmessentials.vanish.see` | op | See other vanished players (staff-among-staff visibility) and target them with /tp. |
| `uxmessentials.vanish.see.level<n>` | op | How deeply you see: a viewer sees a vanished player when their see level reaches that player's use level; the largest level held wins and plain .see is level 1. |
| `uxmessentials.vanish.silent` | op | /vanish -s to vanish or reappear without the fake join/quit broadcast. |
| `uxmessentials.vanish.use` | op | /vanish to become invisible to other players; suppresses fake join/quit. |
| `uxmessentials.vanish.use.level<n>` | op | How deeply you vanish: only a viewer whose see level reaches this level finds you; the largest level held wins and plain .use is level 1. |
{/* /generated */}

## Settings

{/* generated:settings */}
| Key | Default | What it does |
|---|---|---|
| `silent-chests` | `true` | Silence the open animation and sound when a vanished player opens a chest, shulker box, ender chest, or barrel; the lidded containers whose open would otherwise be broadcast to every nearby player and give the vanished player away. |
| `pickup-items` | `false` | The default for whether a vanished player picks up items. It ships off so a vanished player does not silently vacuum drops; each player flips their own preference at runtime with /vanish pickup (the choice is remembered across relog). |
| `night-vision` | `true` | Grant permanent night vision while vanished, so a hidden staff member can see in the dark without a torch. |
| `allow-flight` | `true` | Allow flight while vanished. The allowance is restored to the player's game-mode default on reappear (creative and spectator keep flying; survival and adventure lose the granted allowance). |
| `no-hunger` | `true` | Stop hunger draining while vanished. A deliberate eat still tops the food bar back up; only the drain is blocked. |
| `no-damage` | `true` | Make a vanished player invulnerable; cancel all incoming damage while they are hidden. |
| `mob-target` | `true` | Stop mobs targeting a vanished player, and drop any target a mob already has on them, so nothing paths to or attacks a hidden player. |
| `fake-join-quit` | `true` | When a player vanishes, suppress their real quit line and broadcast a fake "left the game" instead; when they reappear, suppress the real join and broadcast a fake "joined the game". A vanished player who actually disconnects or reconnects also has their real connection line suppressed. The /vanish -s flag (uxmessentials.vanish.silent) skips the fake broadcast for a truly silent vanish. |
| `fake-quit-message` | `"<yellow>{player} left the game"` | The fake lines, as MiniMessage. \{player} is replaced with the player's name. These mirror the vanilla connection format so the illusion holds; change them to match your server's real join/quit style. |
| `fake-join-message` | `"<yellow>{player} joined the game"` |  |
| `fake-quit-message-staff` | `""` | The lines shown instead to viewers who CAN see the vanishing player (staff). Leave blank to send them nothing (the default), so staff simply watch the player wink out without a misleading "left the game". |
| `fake-join-message-staff` | `""` |  |
| `action-bar` | `true` | Show a persistent "You are vanished" action bar to a vanished player, refreshed once a second so it never fades. Its text is the vanish.actionbar entry in your message catalog (per-locale). Turn off to hide the indicator entirely. |
| `join-vanished` | `true` | Let a player who holds uxmessentials.vanish.persist rejoin already vanished: on join they are re-hidden and their real join line is suppressed, so a staff member who relogs stays invisible instead of announcing themselves. |
| `cross-server` | `false` | Cross-server vanish. When on (and the network bus in network.conf is enabled), a player's vanish state syncs across every backend: someone vanished on survival-1 arrives already hidden on survival-2, and /vanish list shows the hidden players across the whole network (still scoped to the caller's see level). Needs a Velocity proxy or Redis bus; ships OFF, and is inert with no behavioural change to same-server vanish when the bus is absent. |
| `foreign.read-supervanish` | `true` |  |
| `foreign.level` | `1` |  |
{/* /generated */}

## Placeholders

{/* generated:placeholders */}
| Placeholder | Renders |
|---|---|
| `%uxmessentials_cansee%` | Whether the viewer can see the player the line is about, or vanish hides them (yes/no). |
| `%uxmessentials_hidden%` | The same read the other way round: whether vanish hides them from the viewer (yes/no). |
{/* /generated */}

## Notes

- **See and use levels decide who sees whom.** A vanished player has a use level, a viewer has a see level, and
  the viewer sees them if their see level is at least the target's use level. Both come from numbered nodes, and
  the highest one held wins, so a senior admin can hide from a junior moderator while still watching over them.
- **Turning the module off is safe.** The messaging and nametag gates degrade to nobody being hidden and staff
  mode's vanish becomes a no-op.
- **Cross-server vanish is off by default and needs the network bus.** Switched on, a player vanished on one
  backend arrives hidden on the next and `/vanish list` covers the whole network, still scoped to the caller's
  see level. A server hop is not read as an unvanish.
- **SuperVanish and PremiumVanish are read, not replaced.** With `foreign.read-supervanish` on, a player they
  hide is treated as vanished by our tab list, nametags, `/list` and `/msg` too, folded in at `foreign.level`.

Related: [Staff](staff.md), [Moderation](moderation.md), [Messaging](messaging.md)

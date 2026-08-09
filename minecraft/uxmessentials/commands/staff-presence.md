---
title: Staff & Presence
order: 1040
---

## Staff & Presence Commands

Two modules cover "who is around and what they're doing." **Presence** is the everyday awareness layer — AFK, `/list`, nicknames, `/whois`, server health — most of which players use themselves. **Staff** is a dedicated, staff-mode-only toolkit that turns a moderator into a patrol loadout with a single command.

All output is **MiniMessage**-formatted, and every command is registered through **Brigadier** with permission-filtered tab completion.

---

## Presence

Awareness and identity commands. The read-only verbs default to everyone; nicknames are staff-gated. (Vanish moved to its own [`vanish`](#vanish) module — see below.)

| Command | Description | Permission |
|---------|-------------|------------|
| `/afk [reason]` (`away`) | Toggle AFK, with an optional reason | `uxmessentials.afk.use` |
| `/list` (`who`, `online`, `playerlist`) | See who is online | `uxmessentials.list.use` |
| `/realname <player>` | Resolve a nickname back to the real account name | `uxmessentials.realname.use` |
| `/nick <name>\|off\|clear` | Set or clear your own display name | `uxmessentials.nick.use` |
| `/nick <player> <name>` | Set another player's display name | `uxmessentials.nick.others` |
| `/whois <player>` | Account, identity, and status card | `uxmessentials.whois.use` |
| `/gc` (`lag`, `tps`, `mem`) | Server health: TPS, uptime, memory, loaded chunks | `uxmessentials.gc.use` |
| `/staff` | List the staff currently online | `uxmessentials.staff.use` |
| `/presencesettings` | Open your personal presence settings panel | `uxmessentials.presencesettings.use` |

<Callout type="info" title="AFK and staff-detection nodes">

A couple of marker nodes shape how presence behaves:

- `uxmessentials.afk.kick.exempt` — never auto-kicked for being AFK.
- `uxmessentials.staff.member` — marks an account as staff, so it shows in `/staff` and staff-only broadcasts.

</Callout>

---

## Vanish

The `vanish` module owns `/vanish` — PremiumVanish-class invisibility with layered see/use levels, buffs, fake join/quit messages, and cross-server sync. It is the single vanish authority the whole plugin reads (messaging, nametags, and staff mode all defer to it). Full details in the [Vanish feature guide](../features/vanish.md).

| Command | Description | Permission |
|---------|-------------|------------|
| `/vanish` (`v`) | Toggle your own vanish; `on` / `off` set it absolutely | `uxmessentials.vanish.use` |
| `/vanish <player>` | Toggle another player's vanish (console-usable) | `uxmessentials.vanish.others` |
| `/vanish list` | List the hidden players you may see (scoped to your see level) | `uxmessentials.vanish.list` |
| `/vanish pickup [on\|off]` | Flip whether you pick up items while vanished | `uxmessentials.vanish.use` |
| `/vanish -s` | Vanish or reappear **silently** — no fake join/quit broadcast | `uxmessentials.vanish.silent` |

<Callout type="info" title="See and persist nodes">

- `uxmessentials.vanish.see` — see vanished players (other staff) and `/tp` to them.
- `uxmessentials.vanish.persist` — rejoin already vanished across a relog.
- The layered families `uxmessentials.vanish.use.level<N>` / `.see.level<N>` tier who can see whom — a viewer sees a vanished player only when their see level is at least the target's use level.

</Callout>

---

## Staff Mode

The `staff` module is **staff-mode-only**: entering staff mode swaps your survival inventory for a moderation loadout (a gadget hotbar) and drops you into vanish, then restores everything when you toggle back out. It is separate from the everyday `/vanish` above.

| Command | Description | Permission |
|---------|-------------|------------|
| `/staffmode [player]` | Flip yourself (or a target) into staff mode — loadout swap, gadget hotbar, and vanish | `uxmessentials.staff.mode` |
| `/staffchat <message>` (`sc`) | Post to the staff-only chat channel | `uxmessentials.staff.chat` |
| `/stafflist` | Online-staff GUI (vanish-aware; click a head to teleport) | `uxmessentials.staff.list` |

<Callout type="tip" title="`/staff` vs `/stafflist`">

`/staff` (presence) prints a quick chat list of who is on duty and is fine for players. `/stafflist` (staff module) opens an interactive, vanish-aware GUI where you can click a staff head to teleport straight to them.

</Callout>

---

## Examples

### Everyday presence

```
/afk grabbing coffee
/list
/whois Steve
/nick <gold>KingSteve
/nick off
```

### Going on patrol

```
/staffmode
/staffchat heading to spawn, reports of grief
/stafflist
```

---

<Callout type="note" title="Rename or disable any of these">

Every command literal above can be renamed, re-aliased, or switched off in `commands/commands.conf` without touching permissions — see [Renaming Commands](../config/commands-conf.md).

</Callout>

## Next Steps

- [🫥 Vanish Feature Guide](../features/vanish.md) - See/use levels, buffs, fake messages, and cross-server sync
- [🕵️ Staff Mode Feature Guide](../features/staff-mode.md) - Loadout, gadgets, and the vanish workflow
- [🛡️ Moderation](moderation.md) - Freeze, punishments, and investigation tools
- [🔐 Permission Reference](../permissions/reference.md) - Every vanish, nick, and staff node

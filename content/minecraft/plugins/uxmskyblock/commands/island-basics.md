---
title: Island Basics
order: 520
description: The everyday commands for creating, reaching, and managing your own island.
icon: terminal
---

<Callout type="note" title="Default words">

Commands are shown with `/is` and the default English sub-command words. Your
server may use different words/aliases, see [config.yml](../configuration/config-yml.md).

</Callout>

---

## `/is`

Opens the **main menu**: your hub for settings, upgrades, warps, the bank, and
your team. See the [Main Menu](../menus/main-menu.md) guide.

---

## `/is create [type]`

Creates your island and teleports you to it. You become the **owner**.

```
/is create          # opens the island-type menu
/is create desert   # creates that type straight away
```

Run with **no type** to open the [Island Type menu](../menus/island-type.md) and
pick a starter island with a click. Pass a `[type]` to skip the menu: it's a
**schematic** defined in `schematics:` in config (e.g. `classic`, `desert`). On
`void` worlds the schematic is pasted; on `normal` worlds a starter platform is
placed on the surface.

<Callout type="note" title="Nether &amp; End">

Each island also gets its own [Nether and End](../configuration/config-yml.md#nether-end)
plot, reached through real portals you build on your island.

</Callout>

<Callout type="warning" title="One island per player">

You can own only one island. Delete the current one with `/is delete` to start over.

</Callout>

---

## `/is home`

Teleports you to your island spawn. Aliases: `go`, `tp`, `git`, `ışınlan`.

On a proxy network, if your island lives on another backend, this sends you to the
right server first, then teleports you in.

---

## `/is sethome`

Sets your island spawn point to where you are standing. Requires the `SET_HOME`
permission (architect rank and above by default).

---

## `/is visit <player>`

Teleports you to another player's island, if it is **open** to visitors and you
are not banned.

```
/is visit Steve
```

---

## `/is info`

Shows island information: owner, level, points, members, and bank balance.

---

## `/is settings`

Opens the [Settings menu](../menus/settings.md) where the owner (or anyone with
`TOGGLE_SETTINGS`) flips island flags, locks the island, and changes the time.

---

## `/is fly`

Toggles flight while you are inside your island border. Requires the `FLY`
permission. Flight ends automatically when you leave the protected area.

---

## `/is lock`

Opens or closes your island to visitors.

- **Locked** → only members can teleport in; `/is visit` is denied for others.
- **Open** → anyone may visit.

---

## `/is border <BLUE|GREEN|RED>`

Changes your personal island border color. The choice is saved per-island.

```
/is border GREEN
```

---

## `/is delete`

Permanently deletes your island after a confirmation menu. All members are
teleported to spawn first, then the entire grid cell is cleared. **This cannot be
undone.**

---

## Related

- [Team & Roles](team-roles.md): invite and manage members
- [Settings menu](../menus/settings.md): flags, lock, time
- [Bank & Economy](bank.md): the island bank

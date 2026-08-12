---
title: Homes & Warps
order: 960
---

## Homes, Warps & Player Warps

Three separate modules cover the "save a spot and jump back to it" family:

- **Homes**: private, per-player teleport points, managed through a slot-based GUI.
- **Warps**: server-wide destinations created by staff (with optional cost, locks, passwords, and ratings).
- **Player Warps**: player-owned public warps, keyed by owner and name.

<Callout type="tip" title="Renamed on your server?">

Every command below can be renamed, re-aliased, or disabled in `commands/commands.conf`: see [Command Configuration](../config/commands-conf.md). Permission nodes stay keyed to the command id regardless of what it's renamed to.

</Callout>

---

## Homes

Homes are **GUI-first**. The single root command `/home` (aliases `h`, `homes`) opens your slot grid, and creating, deleting, renaming, re-iconing, and teleporting all happen inside that menu. Home slots are numbered starting at **1**.

| Command | Description | Permission |
|---------|-------------|------------|
| `/home` | Open/manage your slot-based home grid | `uxmessentials.home.use` |
| `/home visit <player> [slot]` | Teleport to another player's public/invited home | `uxmessentials.home.visit` |
| `/home invite <player> [slot]` | Grant a player access to your home | `uxmessentials.home.invite` |
| `/home uninvite <player> [slot]` | Revoke a player's home access | `uxmessentials.home.invite` |
| `/home admin <player> list\|info\|set\|del\|tp\|clear [slot]` | Manage another player's homes | `uxmessentials.home.admin` |

### Examples

```
/home
/home visit Steve
/home visit Steve 2
/home invite Alex 1
/home admin Steve list
/home admin Steve clear
```

### Related nodes

| Node | Purpose |
|------|---------|
| `uxmessentials.home.limit.<n>` (optional `.<world>`) | Home quota: highest `<n>` wins, or stacks depending on `limit-mode` |
| `uxmessentials.home.icon` | Allow setting a custom home icon |
| `uxmessentials.home.bypass.unsafe` | Teleport even when the destination is unsafe |
| `uxmessentials.home.bypass.cost` | Skip any per-teleport home cost |

---

## Warps

Warps are server-wide destinations. The single root `/warp` (aliases `wp`, `warps`) covers usage and management through subcommands. Older standalone commands (`/warps`, `/setwarp`, `/delwarp`, `/warpinfo`, `/movewarp`) are served here as subcommands or aliases.

| Command | Description | Permission |
|---------|-------------|------------|
| `/warp <name>` | Teleport to a server warp | `uxmessentials.warp.use` |
| `/warp <name> <player>` | Send another player to a warp | `uxmessentials.warp.others` |
| `/warp list` | List warps you may use | `uxmessentials.warp.list` |
| `/warp create <name>` (alias `set`) | Create a warp at your position | `uxmessentials.warp.set` |
| `/warp move <name>` | Re-anchor a warp to your position | `uxmessentials.warp.move` |
| `/warp del <name>` | Delete a warp | `uxmessentials.warp.delete` |
| `/warp info <name>` | Show owner, creation time, and cost | `uxmessentials.warp.info` |
| `/warp lock <name>` | Lock a warp against use | `uxmessentials.warp.lock` |
| `/warp password <name> [pw]` | Set/clear a warp password | `uxmessentials.warp.password` |
| `/warp editor [name]` | Open the warp editor GUI (cost, gates, effects, welcome, icon) | `uxmessentials.warp.edit` |
| `/warp rate <name> <1-5>` | Rate a warp | `uxmessentials.warp.use` |
| `/warp rating <name>` | Show a warp's rating | `uxmessentials.warp.use` |

<Callout type="info" title="Warp signs">

Warps can also be reached from a `[warp]` sign. Creating one needs `uxmessentials.warp.sign.create`; using one needs `uxmessentials.warp.sign.use`.

</Callout>

### Examples

```
/warp spawn
/warp shop Steve
/warp create pvp
/warp password pvp secret123
/warp editor pvp
/warp rate spawn 5
```

### Related nodes

| Node | Purpose |
|------|---------|
| `uxmessentials.warp.use.<warp>` | Per-warp use gate (restrict a specific warp) |
| `uxmessentials.warp.cooldown.<seconds>` | Per-warp cooldown tier |
| `uxmessentials.warp.bypass.lock` | Use a locked warp |
| `uxmessentials.warp.bypass.password` | Skip a warp's password prompt |
| `uxmessentials.warp.bypass.safety` | Teleport even when the destination is unsafe |

---

## Player Warps

Player warps are public destinations owned by individual players. Warps are keyed by `(owner, name)`, so two players can each own a warp of the same name.

| Command | Description | Permission |
|---------|-------------|------------|
| `/pwarp` | Open the player-warp management list GUI | `uxmessentials.pwarp.use` |
| `/pwarp <name> [owner] [password]` | Teleport to your own / a player's public warp | `uxmessentials.pwarp.use` |
| `/pwarp visibility public\|private <name>` | Toggle a warp's visibility | `uxmessentials.pwarp.public` |
| `/pwarp del <name>` | Delete one of your warps | `uxmessentials.pwarp.delete` |
| `/pwarp lock\|password\|edit\|rate\|rating …` | Lock / password / edit / rate a warp | `uxmessentials.pwarp.use` |
| `/setpwarp <name>` | Create or re-anchor a warp at your position | `uxmessentials.pwarp.set` |
| `/pwarps [player]` | List your own / a player's public warps | `uxmessentials.pwarp.list` |

### Examples

```
/setpwarp myshop
/pwarp myshop
/pwarp myshop Steve
/pwarp visibility public myshop
/pwarps Steve
/pwarp del myshop
```

### Related nodes

| Node | Purpose |
|------|---------|
| `uxmessentials.pwarp.limit.<n>` (optional `.<world>`) | Player-warp quota |
| `uxmessentials.pwarp.gui` | Open the player-warp GUI |
| `uxmessentials.playerwarp.bypass.lock` | Use a locked player warp |
| `uxmessentials.playerwarp.bypass.password` | Skip a player warp's password |
| `uxmessentials.playerwarp.bypass.safety` | Teleport even when the destination is unsafe |

---

## Next Steps

- [🏠 Homes](../modules/homes.md) - Slot GUI, invites, and quotas explained
- [📍 Warps](../features/warps.md) - Costs, locks, ratings, and the editor
- [🌐 Player Warps](../features/player-warps.md) - Player-owned public destinations
- [🔐 Permission Reference](../permissions/reference.md) - Quota tiers and bypass nodes

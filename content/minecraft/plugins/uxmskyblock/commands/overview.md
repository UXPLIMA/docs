---
title: Overview
order: 510
description: The /island root, its configurable aliases, and how the tree is organised.
icon: book-open
---

## The Command Is Fully Configurable

By default the main command is `/island`, with aliases `/is`, `/sb`, `/ada`,
`/sky`. **Both the name and every alias are configurable** in
[config.yml](../configuration/config-yml.md) under `command:`, and the plugin
registers them dynamically: there is no command in `plugin.yml`.

Every **sub-command word** is configurable too, and most accept several aliases
(English + Turkish out of the box). For example `create` also responds to
`oluştur`, `kur`; `home` also responds to `go`, `tp`, `git`.

This wiki uses `/is` and the **default English sub-command words**.

<Callout type="tip" title="Tab completion everywhere">

Every sub-command and its arguments support context-aware tab completion:
player names, role IDs, warp names, border colors, bank actions, and more.

</Callout>

---

## All Commands at a Glance

| Command | Description | Page |
|---------|-------------|------|
| `/is` | Open the main menu | [Island Basics](island-basics.md) |
| `/is create [type]` | Create your island | [Island Basics](island-basics.md) |
| `/is home` | Teleport to your island | [Island Basics](island-basics.md) |
| `/is visit <player>` | Visit another island | [Island Basics](island-basics.md) |
| `/is sethome` | Set your island spawn | [Island Basics](island-basics.md) |
| `/is delete` | Delete your island | [Island Basics](island-basics.md) |
| `/is settings` | Open the settings menu | [Island Basics](island-basics.md) |
| `/is fly` | Toggle flight on your island | [Island Basics](island-basics.md) |
| `/is lock` | Open/close your island to visitors | [Island Basics](island-basics.md) |
| `/is border <BLUE\|GREEN\|RED>` | Change border color | [Island Basics](island-basics.md) |
| `/is invite <player>` | Invite a player | [Team & Roles](team-roles.md) |
| `/is accept` / `/is deny` | Respond to an invite | [Team & Roles](team-roles.md) |
| `/is leave` | Leave your team | [Team & Roles](team-roles.md) |
| `/is kick <player>` | Remove a member | [Team & Roles](team-roles.md) |
| `/is ban` / `/is unban <player>` | Ban/unban a player | [Team & Roles](team-roles.md) |
| `/is trust` / `/is untrust <player>` | Quick-trust a player | [Team & Roles](team-roles.md) |
| `/is transfer <player>` | Hand over ownership | [Team & Roles](team-roles.md) |
| `/is role <player> <role>` | Assign a role | [Team & Roles](team-roles.md) |
| `/is role create\|delete\|perm\|list` | Manage custom roles | [Team & Roles](team-roles.md) |
| `/is members` | List your team | [Team & Roles](team-roles.md) |
| `/is upgrade` | Open the upgrades menu | [Progression](../progression/upgrades.md) |
| `/is level` | Show your island level | [Leaderboard](leaderboard.md) |
| `/is top [holo]` | Leaderboard / following hologram | [Leaderboard](leaderboard.md) |
| `/is block` | Browse block point values | [Block Values](../menus/blocks.md) |
| `/is bank [deposit\|withdraw] <amount>` | Manage the island bank | [Bank & Economy](bank.md) |
| `/is warp <player> [name]` | Warp to a public island warp | [Warps](warps.md) |
| `/is setwarp <name>` / `/is delwarp <name>` | Manage your warps | [Warps](warps.md) |
| `/is info` | Show island info | [Island Basics](island-basics.md) |
| `/is admin <...>` | Admin tools | [Admin](admin.md) |
| `/is anaspawn` | Set the global return spawn | [Admin](admin.md) |
| `/is proxy` | Show proxy/cross-server status | [Admin](admin.md) |

---

## Permissions

uxmSkyblock uses very few server-level permission nodes: most "permissions" are
**island role permissions**, not Bukkit ones.

| Node | Default | Purpose |
|------|---------|---------|
| `skyblock.admin` | op | Access to `/is admin`, `/is anaspawn`, `/is proxy` |
| `skyblock.time` | op | Lock the island time (day/night) |
| `skyblock.warps.<n>` | - | Raise a player's warp limit to `<n>` |

See [Roles & Permissions](../protection/roles-permissions.md) for the in-island
permission system.

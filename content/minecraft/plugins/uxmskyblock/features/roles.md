---
title: Roles
order: 402
description: The four roles every island has, roles of your own, and the 35 island permissions.
icon: shield-check
---

Every island has four roles a member can hold, from the top: Owner, Co-Owner, Moderator and Member. Visitor is
everyone who is not a member, and holds no permission. An owner can make roles of their own beside the four.

| Role | Weight | Notes |
|---|---|---|
| Owner | 1000 | Holds every permission; the role never changes and the owner cannot be removed |
| Co-Owner | 800 | |
| Moderator | 600 | |
| Member | 400 | A new member's role, and the role a removed custom role falls back to |
| Visitor | 0 | Not a member |

A member acts only on members whose role is below their own, and gives only roles below their own. Moving somebody up
needs Move members up, and moving them down needs Move members down. Owner and Visitor are anchors: no command gives
them.

## Roles of your own

`/is permissions create <name>` makes a role: 2 to 16 lower case letters, digits and `_`. It starts with what members
may do. An owner makes 3 as shipped, raised by permission nodes you choose up to 10; see `roles.custom` in
[config.conf](../config/config-conf.md). `/is permissions delete <name>` removes it, and whoever held it is a member
again.

## The permissions

What each role holds as shipped. Change it per island in the [role permissions window](../menus/team.md#role-permissions)
or with `/is permissions <role> <permission> on | off`.

| Permission | Name | Owner | Co-Owner | Moderator | Member |
|---|---|---|---|---|---|
| `block_break` | Break blocks | yes | yes | yes | yes |
| `block_place` | Place blocks | yes | yes | yes | yes |
| `bucket_use` | Use buckets | yes | yes | yes | yes |
| `natural_interact` | Use doors, buttons and blocks | yes | yes | yes | yes |
| `redstone_interact` | Use redstone | yes | yes | yes | yes |
| `chest_open` | Open chests | yes | yes | yes | yes |
| `furnace_use` | Use furnaces | yes | yes | yes | yes |
| `shulker_open` | Open shulker boxes | yes | yes | yes | yes |
| `barrel_open` | Open barrels | yes | yes | yes | yes |
| `anvil_use` | Use anvils | yes | yes |  |  |
| `beacon_modify` | Change beacons | yes | yes |  |  |
| `spawner_break` | Break spawners | yes | yes |  |  |
| `spawner_change_type` | Change a spawner's mob | yes | yes |  |  |
| `spawner_upgrade` | Upgrade spawners | yes | yes |  |  |
| `crop_trample_bypass` | Trample crops | yes | yes |  |  |
| `animal_breed` | Breed animals | yes | yes | yes | yes |
| `animal_kill` | Kill animals | yes | yes | yes | yes |
| `bank_deposit` | Put money in the bank | yes | yes | yes | yes |
| `bank_withdraw` | Take money from the bank | yes | yes |  |  |
| `shop_access` | Use the island shop | yes | yes |  |  |
| `vault_view` | Open the vault | yes | yes | yes | yes |
| `vault_deposit` | Put items in the vault | yes | yes | yes | yes |
| `vault_withdraw` | Take items from the vault | yes | yes | yes |  |
| `member_invite` | Invite players | yes | yes | yes |  |
| `member_kick` | Remove members | yes | yes | yes |  |
| `member_promote` | Move members up | yes |  |  |  |
| `member_demote` | Move members down | yes |  |  |  |
| `settings_modify` | Change the settings | yes | yes |  |  |
| `biome_change` | Change the biome | yes | yes |  |  |
| `box_move` | Move the box | yes | yes |  |  |
| `island_fly` | Fly on the island | yes | yes | yes | yes |
| `warp_create` | Open warps | yes | yes |  |  |
| `warp_delete` | Close warps | yes | yes |  |  |
| `chat_view` | Read the island chat | yes | yes | yes | yes |
| `chat_send` | Write in the island chat | yes | yes | yes | yes |

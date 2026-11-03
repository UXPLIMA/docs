---
title: Members
order: 303
description: Invites, kicks, bans, roles and leaving.
icon: users
---

| Command | Turkish | Does |
|---|---|---|
| `/claim invite <player> [duration]` | `davet` | Invites a player to the claim you stand in; a duration ends the membership by itself |
| `/claim accept [claim]` | `kabul` | Joins a claim you were invited to |
| `/claim deny [claim]` | `reddet` | Turns an invite down |
| `/claim leave` | `ayril` | Leaves the claim you stand in |
| `/claim kick <player>` | `at` | Takes a member out |
| `/claim ban <player> [duration]` | `yasakla` | Keeps a player out; a ban takes a member's role away |
| `/claim unban <player>` | `affet` | Lets a banned player back |
| `/claim role <player> <role>` | `rol` | Gives a member another role |
| `/claim transfer <player>` | `devret` | Gives the claim away; the old owner is not left inside |

A player may act only on members whose role is below their own, and only with the role permission for it (`invite`,
`kick`, `ban`, `manage-members`).

Related: [Members and roles](../features/members-and-roles.md)

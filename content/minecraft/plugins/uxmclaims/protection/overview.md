---
title: How protection works
order: 301
description: Which rule decides each action inside a claim, in what order, and who bypasses it.
icon: shield-check
---

Inside a claim, three kinds of rule decide what happens. Unclaimed land is not protected at all, except
that nothing started there may reach into a claim.

## Three kinds of rule

| Rule | Decides | Set by | Applies to |
|---|---|---|---|
| [Role permission](permissions.md) | What a player does: building, containers, entities, entering, teleporting | The owner, per role, with per-member overrides | One player |
| [Flag](flags.md) | What the world does: explosions, fire, fluids, growth, spawning; also PvP and keep inventory | The owner, per claim | Everyone, the owner included |
| Border rule | Nothing crosses from one claim, or from the wilderness, into another | Always on | Everything |

A player's own action checks a role permission. A world event checks a flag. Turning `FIRE_SPREAD` off
stops fire spreading in the owner's base too: flags protect against the world, permissions against
people. A few actions check both: TNT that a player lights outside a claim breaks blocks inside only when
the claim allows TNT and the player holds `BLOCK_BREAK` there.

## Border rules

These cannot be turned off.

| Across a claim border | What is blocked |
|---|---|
| Fluids | Water and lava flowing in or out |
| Fire | Spreading, burning blocks or igniting them across the border |
| Pistons | Pushing or pulling any block across, slime and honey drag included |
| Growth | Trees, huge mushrooms and bone meal growth reaching into another claim |
| Dispensers | Firing, placing, equipping armour or shearing across |
| Hoppers | Moving items between containers in different claims, or picking up items from another claim |
| Sponges | Soaking up water across |
| Explosions | Blocks of another claim, for a bed or anchor blast, or for a player's TNT, end crystal or TNT minecart without that player's `BLOCK_BREAK` there |

## The order of a permission check

When a player acts inside a claim, the first matching row decides:

| Step | Result |
|---|---|
| Operator, or holds `uxmclaims.admin` | **Allowed** |
| Banned in this claim | **Denied** |
| The owner | **Allowed** |
| A member, with the permission in their denied overrides | **Denied** |
| A member, with the permission in their allowed overrides | **Allowed** |
| Any other member | Their role decides; the `Member` role if their role no longer exists |
| Not a member | The `Default` role decides |

## Bypasses

| Holder | Effect |
|---|---|
| Operator or `uxmclaims.admin` | Every role permission in every claim, every ability, the admin commands. Opens any claim's menu from its claim block, and is exempt from `disabledCommandsInClaim`. Through LuckPerms, also no limits, costs or delays |
| `uxmclaims.bypass.*`, `uxmclaims.bypass.<category>.*`, `uxmclaims.bypass.<category>.<action>` | Pass the matching [ability](../permissions/abilities.md) checks |
| `uxmclaims.bypass.teleport` | Teleports and ender pearls into a claim are never refused: not for a ban, `MOVE_INSIDE`, `TELEPORT`, `ENDERPEARL_USE` or `CHORUS_TELEPORT` |
| `uxmclaims.bypass.enter` | The access guard never ejects the player |

Nothing bypasses a flag. A flag is a property of the world inside the claim, and turning off
`TNT_EXPLOSIONS` stops an admin's TNT too.

## Entering, and the access guard

`MOVE_INSIDE` is checked twice:

| Check | What happens |
|---|---|
| Crossing into the claim | A player without `MOVE_INSIDE` is bounced back across the border, vehicle included. A banned player cannot teleport in by any means. |
| The access guard | With `generalSettings.accessGuardEnabled: true`, every online player is checked every `accessGuardIntervalTicks` (5) ticks. A banned player, or one without `MOVE_INSIDE`, is moved to the nearest safe spot outside, however they got in: vehicles, pistons, knockback, portals, logging in. |

## Outside the permission system

| Setting | Effect |
|---|---|
| `generalSettings.disabledWorlds` | No claims or chunks in these worlds. Matched without regard to case. It applies with or without WorldGuard, and `/claim reload` applies a changed list. |
| `generalSettings.disabledCommandsInClaim` | Commands that a player who is not a member may not run while standing in a claim. `/sethome` and `/essentials:sethome` both match `sethome`. Admins are exempt. |
| The claim block | Nobody can break it, blow it up or move it with a piston. |

`disabledCommandsInClaim` ships with `sethome` in it, which stops visitors saving a home inside someone
else's base and teleporting back into it later.

---
title: Ability Permissions
order: 330
icon: key
---

## Overview

Ability permissions control **what actions** a player can perform within uxmClaims. These are Bukkit permissions that must be assigned using a permission plugin like LuckPerms.

<Callout type="warning" title="Default Behavior">

By default, **no abilities are granted** to players. You must assign permissions (e.g., `uxmclaims.ability.*`) via your permission plugin to enable actions.

</Callout>

---

## Wildcard Permission

| Permission | Description |
| :--- | :--- |
| `uxmclaims.ability.*` | Grants all abilities |

---

## Bypass Permissions

Bypass permissions allow players to perform abilities **without needing the regular ability permission**. This is useful for staff members who need to manage claims without having full admin access.

| Permission | Description |
| :--- | :--- |
| `uxmclaims.bypass.*` | Bypass all ability checks |
| `uxmclaims.bypass.<category>.*` | Bypass all abilities in a category |

### All Bypass Permissions

#### Claim Management
| Permission Node | Description |
| :--- | :--- |
| `uxmclaims.bypass.claim.create` | Bypass claim creation check |
| `uxmclaims.bypass.claim.delete` | Bypass claim deletion check |
| `uxmclaims.bypass.claim.rename` | Bypass claim rename check |
| `uxmclaims.bypass.claim.relocate` | Bypass claim relocate check |
| `uxmclaims.bypass.claim.reschedule` | Bypass claim reschedule check |
| `uxmclaims.bypass.claim.transfer` | Bypass claim transfer check |
| `uxmclaims.bypass.claim.flag.<flag>` | Bypass flag change check |

#### Chunk Management
| Permission Node | Description |
| :--- | :--- |
| `uxmclaims.bypass.chunk.extend` | Bypass chunk extend check |
| `uxmclaims.bypass.chunk.shrink` | Bypass chunk shrink check |

#### Member Management
| Permission Node | Description |
| :--- | :--- |
| `uxmclaims.bypass.member.invite` | Bypass member invite check |
| `uxmclaims.bypass.member.revoke` | Bypass member revoke check |
| `uxmclaims.bypass.member.kick` | Bypass member kick check |
| `uxmclaims.bypass.member.ban` | Bypass member ban check |
| `uxmclaims.bypass.member.unban` | Bypass member unban check |
| `uxmclaims.bypass.member.role` | Bypass member role change check |
| `uxmclaims.bypass.member.permissions.<perm>` | Bypass member permission change check |

#### Role Management
| Permission Node | Description |
| :--- | :--- |
| `uxmclaims.bypass.role.create` | Bypass role creation check |
| `uxmclaims.bypass.role.delete` | Bypass role deletion check |
| `uxmclaims.bypass.role.rename` | Bypass role rename check |
| `uxmclaims.bypass.role.priority` | Bypass role priority change check |
| `uxmclaims.bypass.role.permissions.<perm>` | Bypass role permission change check |

#### Warp Management
| Permission Node | Description |
| :--- | :--- |
| `uxmclaims.bypass.warp.create` | Bypass warp creation check |
| `uxmclaims.bypass.warp.delete` | Bypass warp deletion check |
| `uxmclaims.bypass.warp.rename` | Bypass warp rename check |
| `uxmclaims.bypass.warp.relocate` | Bypass warp relocate check |
| `uxmclaims.bypass.warp.visibility` | Bypass warp visibility change check |

#### Movement
| Permission Node | Description |
| :--- | :--- |
| `uxmclaims.bypass.teleport` | Bypass teleport restrictions |

#### Block & Vault
| Permission Node | Description |
| :--- | :--- |
| `uxmclaims.bypass.block.place` | Bypass block place check |
| `uxmclaims.bypass.block.destroy` | Bypass block destroy check |
| `uxmclaims.bypass.block.change` | Bypass block change check |
| `uxmclaims.bypass.vault.access` | Bypass vault access check |

<Callout type="tip" title="Use Case">

Give moderators `uxmclaims.bypass.member.*` to allow them to manage members in any claim without granting `uxmclaims.admin`.

</Callout>

---

## Claim Management

| Permission Node | Description |
| :--- | :--- |
| `uxmclaims.ability.claim.create` | Allow creating new claims |
| `uxmclaims.ability.claim.delete` | Allow deleting owned claims |
| `uxmclaims.ability.claim.rename` | Allow renaming claims |
| `uxmclaims.ability.claim.relocate` | Allow relocating a claim |
| `uxmclaims.ability.claim.reschedule` | Allow extending claim duration |
| `uxmclaims.ability.claim.transfer` | Allow transferring claim ownership |
| `uxmclaims.ability.claim.flag.<flag>` | Allow toggling a specific flag (or `*` for all) |

---

## Chunk Management

| Permission Node | Description |
| :--- | :--- |
| `uxmclaims.ability.chunk.extend` | Allow adding chunks to a claim |
| `uxmclaims.ability.chunk.shrink` | Allow removing chunks from a claim |

---

## Member Management

| Permission Node | Description |
| :--- | :--- |
| `uxmclaims.ability.member.invite` | Allow inviting players to a claim |
| `uxmclaims.ability.member.revoke` | Allow revoking invitations |
| `uxmclaims.ability.member.kick` | Allow kicking members |
| `uxmclaims.ability.member.ban` | Allow banning members |
| `uxmclaims.ability.member.unban` | Allow unbanning players |
| `uxmclaims.ability.member.role` | Allow changing a member's role |
| `uxmclaims.ability.member.permissions.<perm>` | Allow toggling specific member permissions (or `*` for all) |

---

## Role Management

| Permission Node | Description |
| :--- | :--- |
| `uxmclaims.ability.role.create` | Allow creating custom roles |
| `uxmclaims.ability.role.delete` | Allow deleting roles |
| `uxmclaims.ability.role.rename` | Allow renaming roles |
| `uxmclaims.ability.role.priority` | Allow changing role priority |
| `uxmclaims.ability.role.permissions.<perm>` | Allow toggling specific role permissions (or `*` for all) |

---

## Warp Management

| Permission Node | Description |
| :--- | :--- |
| `uxmclaims.ability.warp.create` | Allow creating claim warps |
| `uxmclaims.ability.warp.delete` | Allow deleting warps |
| `uxmclaims.ability.warp.rename` | Allow renaming warps |
| `uxmclaims.ability.warp.relocate` | Allow moving warp location |
| `uxmclaims.ability.warp.visibility` | Allow changing warp visibility (public/private) |

---

## Block Operations

| Permission Node | Description |
| :--- | :--- |
| `uxmclaims.ability.block.place` | Allow placing the claim block |
| `uxmclaims.ability.block.destroy` | Allow breaking the claim block |
| `uxmclaims.ability.block.change` | Allow changing the claim block type via menu |

---

## Miscellaneous

| Permission Node | Description |
| :--- | :--- |
| `uxmclaims.ability.vault.access` | Allow opening the claim vault |

---

## Example: Restricting Flag Access

To prevent players from toggling specific flags:

```
# Negate the specific flag permission
lp user <player> permission set uxmclaims.ability.claim.flag.pvp false

# Or negate all flags
lp user <player> permission set uxmclaims.ability.claim.flag.* false
```

---

## Next Steps

- [🛡️ Role Permissions](../protection/permissions.md) - Internal claim permissions for roles
- [⚖️ Limits & Costs](../config/entitlements-yml.md) - Configure limits and economy costs

---
title: roles.yml
order: 502
description: The roles every new claim is created with, and their starting permissions.
icon: shield-user
---

`roles.yml` is a template. It is read when a claim is **created**, and the roles it describes are
copied into that claim. Editing it later changes nothing about claims that already exist.

## The three system roles

```yaml
defaults:
  owner:
    name: "Owner"
    priority: 0
    permissions: [ ]
  member:
    name: "Member"
    priority: 1
    permissions:
      - BLOCK_PLACE
      - BLOCK_BREAK
      # …
  default:
    name: "Default"
    priority: 2
    permissions:
      - MOVE_INSIDE
```

The keys `owner`, `member` and `default` are fixed and lowercase; any other key under `defaults` is
skipped with a warning. Only `name` and `permissions` are read here: the system roles always sit at
Owner first, Member second and Default last, whatever `priority` says. The `name` is what players see
and may be anything.

| Role | Ships with |
|---|---|
| `Owner` | An **empty** list. The owner holds everything implicitly; entries here change nothing. |
| `Member` | 31 permissions: build, interact, entities, movement, private warps. No management. |
| `Default` | `MOVE_INSIDE` only. |

The full `Member` set is `BLOCK_PLACE`, `BLOCK_BREAK`, `BUCKET_FILL`, `BUCKET_EMPTY`,
`HANGING_PLACE`, `HANGING_BREAK`, `SIGN_EDIT`, `USE_REDSTONE`, `USE_MECHANISMS`, `CONTAINER_OPEN`,
`ARMOR_STAND_MANIPULATE`, `SLEEP`, `ANIMAL_DAMAGE`, `MONSTER_DAMAGE`, `ANIMAL_INTERACT`,
`SHEAR_ENTITY`, `VILLAGER_TRADE`, `VEHICLE_INTERACT`, `RIDE_ENTITY`, `POTION_USE`, `MOVE_INSIDE`,
`TELEPORT`, `ENDERPEARL_USE`, `CHORUS_TELEPORT`, `ELYTRA_USE`, `WIND_BURST`, `WIND_CHARGE`,
`ITEM_DROP`, `ITEM_PICKUP`, `FISHING` and `USE_WARPS`. Every permission name is listed in
[Claim permissions](../protection/permissions.md); a misspelt name is skipped with a warning.

## Custom roles

```yaml
custom:
  trusted:
    name: "Trusted"
    priority: 2
    permissions:
      - BLOCK_PLACE
      - BLOCK_BREAK
      - CONTAINER_OPEN
      - USE_MECHANISMS
      - TELEPORT
      - MANAGE_INVITES
```

Anything under `custom` is added to every new claim, in file order. `custom: { }` ships empty: new
claims get exactly the three system roles. Each name must differ from every other role name.

Each custom role is first placed directly above Default, then moved to the `priority` you write:

| `priority` | Result |
|---|---|
| `1` | Swaps with Member: the custom role outranks Member |
| `2` up to the number of custom roles plus 1 | Between Member and Default, in that position |
| `0`, or Default's position or lower | Refused, and claim creation fails |

## Priority

Lower is higher rank. Priority drives three things:

- the order `/claim member promote` and `demote` walk
- who wins when two members try to open the vault at once
- the order roles appear in the role list menu

## Notes

- **Editing `roles.yml` does not touch existing claims.** Use
  `/claim admin bulk setrolepermission <role> <perm> <value>` for that: it finds the role by name or
  by type (`OWNER`, `MEMBER`, `DEFAULT`) in every loaded claim, without confirmation.
- **`Default` is the one to think about.** It is what a stranger gets. Adding `CONTAINER_OPEN` here
  opens every chest on the server to everyone.
- **Do not put management permissions in `Member`.** A new member with `MANAGE_INVITES` can invite
  their friends into someone else's base. Ship those in a custom role you assign deliberately.
- **`USE_WARPS` arrives with the update.** Existing claims get it on their Member and custom roles
  (database version v6). The update also adds it to the `member` list in `roles.yml`, unless you
  edited that list: then add `USE_WARPS` yourself, or members of new claims cannot use private warps.
- **A deleted role drops its holders onto `Member`,** not `Default`. If `Member` is generous and your
  custom role was restrictive, deleting the role is an upgrade for everyone holding it.
- **The role names are translated.** Switching [language](languages.md) renames `Owner`, `Member`
  and `Default` in this file if you left them as shipped; claims created earlier keep their names.

<Callout type="warning" title="Test a custom role before opening the server">

A custom role with a priority outside the allowed range, or a name that repeats another role's, makes
every claim creation fail. Create one test claim after editing `custom`.

</Callout>

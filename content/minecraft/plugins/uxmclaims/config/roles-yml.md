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

The keys `owner`, `member` and `default` are fixed and lowercase. The `name` is what players see and
may be anything.

| Role | Ships with |
|---|---|
| `Owner` | An **empty** list. The owner holds everything implicitly; entries here change nothing. |
| `Member` | 30 permissions — build, interact, entities, movement. No management. |
| `Default` | `MOVE_INSIDE` only. |

The full `Member` set is `BLOCK_PLACE`, `BLOCK_BREAK`, `BUCKET_FILL`, `BUCKET_EMPTY`,
`HANGING_PLACE`, `HANGING_BREAK`, `SIGN_EDIT`, `USE_REDSTONE`, `USE_MECHANISMS`, `CONTAINER_OPEN`,
`ARMOR_STAND_MANIPULATE`, `SLEEP`, `ANIMAL_DAMAGE`, `MONSTER_DAMAGE`, `ANIMAL_INTERACT`,
`SHEAR_ENTITY`, `VILLAGER_TRADE`, `VEHICLE_INTERACT`, `RIDE_ENTITY`, `POTION_USE`, `MOVE_INSIDE`,
`TELEPORT`, `ENDERPEARL_USE`, `CHORUS_TELEPORT`, `ELYTRA_USE`, `WIND_BURST`, `WIND_CHARGE`,
`ITEM_DROP`, `ITEM_PICKUP` and `FISHING`.

## Custom roles

```yaml
custom:
  moderator:
    name: "Moderator"
    priority: 1
    permissions:
      - BLOCK_PLACE
      - BLOCK_BREAK
      - CONTAINER_OPEN
      - USE_MECHANISMS
      - TELEPORT
      - MANAGE_INVITES
```

Anything under `custom` is added to every new claim. Priorities are adjusted automatically to sit
between `Member` and `Default`, so the number you write is a hint about ordering rather than an
absolute.

`custom: { }` ships empty — new claims get exactly the three system roles.

## Priority

Lower is higher rank. Priority drives three things:

- the order `/claim member promote` and `demote` walk
- who wins when two members try to open the vault at once
- the order roles appear in the role list menu

## Notes

- **Editing `roles.yml` does not touch existing claims.** Use
  `/claim admin bulk setrolepermission <role> <perm> <value>` for that — it works on the system roles,
  in every loaded claim, without confirmation.

- **`Default` is the one to think about.** It is what a stranger gets. Adding `CONTAINER_OPEN` here
  opens every chest on the server to everyone.

- **Do not put management permissions in `Member`.** A new member with `MANAGE_INVITES` can invite
  their friends into someone else's base. Ship those in a custom role you assign deliberately.

- **A deleted role drops its holders onto `Member`,** not `Default`. If `Member` is generous and your
  custom role was restrictive, deleting the role is an *upgrade* for everyone holding it.

<Callout type="tip" title="A three-tier setup that works">

Leave `Member` as shipped, keep `Default` at `MOVE_INSIDE`, and add one custom `Trusted` role with the
`Member` set plus `MANAGE_WARPS` and `USE_WARPS`. Owners then have somewhere to promote a friend that
is not "give them everything".

</Callout>

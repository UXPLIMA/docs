---
title: Sub areas and rent
order: 406
description: Areas inside a claim with their own members and flags, offered for rent.
icon: layout-grid
---

A sub area is a set of chunks inside a claim with its own members and flags. `/claim areacreate <name>` makes one of
the chunk you stand in and `/claim areachunk <area>` adds or takes out a chunk. An owner's `sub-areas` limit caps them.

## Rent

The owner offers a sub area with `/claim areaoffer <area> <price> [days]`, up to `rent.max-period-days` (30). A player
rents it with `/claim rent` and holds the member role inside it.

- Rent renews when the period ends if the tenant can pay.
- A tenant who is away is charged at their next join, and the tenancy ends when `rent.grace-hours` (24) pass unpaid.
- `/claim unrent` stops renewing.
- The owner ends a tenancy from the [area window](../menus/areas.md#area).

The tenant and owner read what happened in their [inbox](staff-tools.md#the-inbox).

Related: [Sub area commands](../commands/areas-and-market.md#sub-areas)

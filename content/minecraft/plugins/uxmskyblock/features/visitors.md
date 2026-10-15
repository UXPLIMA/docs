---
title: Visitors & Visit Fees
order: 740
description: What a visitor may not run on an island, and the fee an island may charge for a
  visit.
icon: door-open
---

A player standing on an island they are no member of is a visitor there. `modules/visitors.conf`
sets what that means.

## Blocked commands

`visitors.blocked-commands` lists the commands a visitor cannot run, each with the words after it
that are blocked too: `"home set"` blocks `/home set` and leaves `/home` alone. A plugin's prefix,
as `/essentials:sethome`, does not get past.

```hocon
blocked-commands = [
    "sethome"
    "setwarp"
]
```

## Visit fees

An island may charge a visitor who comes by `/is visit`. A member whose role may change the
island's settings sets the fee with `/is visitfee <amount>`, and `0` charges nothing. The visitor is
shown the fee and confirms with `/is visit <player> confirm`, and the money goes to the island's
bank.

- `visitors.fee.most` is the most an island may charge, `10000.00` as shipped. `0` turns visit
  fees off.
- Members, and staff who bypass the island protection, pay nothing.
- Public warps stay free.
- Fees need an economy plugin.

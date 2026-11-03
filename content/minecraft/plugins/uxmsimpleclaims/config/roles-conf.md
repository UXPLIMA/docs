---
title: roles.conf
order: 602
description: The four starting roles and the role grammar.
icon: shield
---

```hocon
roles {
  visitor   { name = "Visitor",   priority = 0,   permissions = [enter, doors, switches, ...] }
  member    { name = "Member",    priority = 10,  permissions = [...] }
  moderator { name = "Moderator", priority = 50,  permissions = [...] }
  owner     { name = "Owner",     priority = 100, permissions = [] }
}
```

| Key | Meaning |
|---|---|
| `name` | What players read |
| `priority` | A higher number outranks a lower; the owner is always highest |
| `permissions` | The words of [Members and roles](../features/members-and-roles.md#role-permissions) |

`visitor` and `owner` must stay. A change applies to claims made afterwards; an owner changes their own claims' roles
in the role window.

Related: [Members and roles](../features/members-and-roles.md)

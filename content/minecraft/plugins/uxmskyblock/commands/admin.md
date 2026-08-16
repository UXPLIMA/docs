---
title: Admin
order: 570
description: 'Tools for server staff. Everything here needs the skyblock.admin permission
  (default: op). To players without it, these sub-commands are hidden from tab completion
  and respond as unknown commands.'
---

---

## `/is admin <subcommand>`

The admin router. The target island is resolved from the named player (as owner or
member).

| Sub-command | What it does |
|-------------|--------------|
| `info <player>` | Show full island details |
| `tp <player>` | Teleport to a player's island |
| `setowner <player> <newOwner>` | Force-transfer ownership |
| `delete <player>` | Force-delete an island |
| `bank set\|add\|remove <player> <amount>` | Adjust the island bank |
| `role <player> <role>` | Force-set a member's role |
| `points set\|add <player> <amount>` | Adjust island points |
| `level <player> <level>` | Set island level |
| `reload` | Reload all config files |
| `help` | List admin sub-commands |

```
/is admin info Steve
/is admin bank add Steve 10000
/is admin points set Steve 5000
/is admin reload
```

<Callout type="tip" title="Reload without restart">

`/is admin reload` re-reads `config.yml`, `messages.yml`, `roles.yml`,
`upgrades.yml`, `block-values.yml`, `levels.yml`, and the menus. New keys from a
jar update are merged in automatically on load.

</Callout>

---

## `/is anaspawn`

Saves the **global return spawn**: where players are teleported after their
island is deleted (and on spawn return). Stand where you want the spawn and run it.

```
/is anaspawn
```

This is the same location described by `spawn:` in
[config.yml](../configuration/config-yml.md). On a proxy network, if
`proxy.spawn-server` is a different server, players are routed there instead and
these coordinates aren't used.

---

## `/is proxy`

Shows **cross-server status**: whether the proxy is enabled, this server's name,
the create-server list, and which server a given island lives on. Useful for
diagnosing routing problems.

See [Cross-Server Setup](../proxy/cross-server.md).

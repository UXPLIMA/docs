---
title: network.conf
order: 604
description: The network block, key by key, and which side reads each.
icon: network
---

```hocon
network {
  enabled = false
  server-name = ""
  secret = ""
  claim-seconds = 30
  allowed-commands = []
  auth-servers = []
  lobbies = []
  balance = least-players
  after-login = lobby
  by-domain = []
  wait-in = server
  limbo-file = "limbo.schem"
  limbo-spawn = [0.5, 100, 0.5]
  redis {
    uri = ""
    channel = "uxmauth"
  }
}
```

| Key | Read by |
|---|---|
| `enabled`, `server-name`, `secret` | Every server |
| `claim-seconds`, `allowed-commands`, `auth-servers`, `lobbies`, `balance`, `after-login`, `by-domain`, `wait-in`, `limbo-file`, `limbo-spawn` | The proxy |
| `redis.uri`, `redis.channel` | Every member |

Every key is explained in [A network](../features/network.md).

Related: [A network](../features/network.md)

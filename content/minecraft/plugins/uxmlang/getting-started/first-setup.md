---
title: Your first setup
order: 104
description: A server with one language, a server where players choose, and a network.
icon: flag
---

## A server in one language

Set `language.force = "tr"` in `config.conf` and `/uxmlang reload`. Everybody reads Turkish, whatever they chose and
whatever their client says. `/lang set` answers that the server reads one language. You may also take `uxmlang.use`
from everyone so `/lang` is refused:

```
lp group default permission set uxmlang.use false
```

## Players choose, nothing follows the client

Set `language.follow-client = false` and keep `language.default = "en"`. A new player reads English until they pick
a language with `/lang`.

## A network

1. Point `storage` of every server at one MariaDB, MySQL or PostgreSQL database, with the same `table-prefix`.
2. For changes that reach a server at once, set `network.transport = "redis"` and the same `network.redis` block on
   every server.
3. Restart each server.

A player who picks Turkish on the lobby arrives on survival reading Turkish.

## Check it

Join as a player who is not an operator, run `/lang`, click a flag. The reply comes in the new language, and every UXM
plugin's commands and windows follow.

Related: [Concepts](concepts.md), [A network](../features/network.md)

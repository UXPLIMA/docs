---
title: A network
order: 1310
description: Geyser on a backend, on the Velocity proxy, or standalone, and how the pack reaches it.
icon: network
---

| Where Geyser runs | What you install | How the pack reaches Geyser |
|---|---|---|
| On a Paper server | uxmBedrock and the extension on that server | The extension reads `plugins/uxmBedrock/output` beside it |
| On the Velocity proxy | uxmBedrock on every server, the extension in the proxy's Geyser, uxmBedrock-Velocity on the proxy | Each server sends its build to the proxy over the network; the proxy makes one pack from all of them |
| On the proxy without uxmBedrock-Velocity, or Geyser Standalone | uxmBedrock on every server, the extension with `source.folder = player-connection` | The next Bedrock player to join carries what changed over their own connection |

## One pack for the network

With uxmBedrock-Velocity, a player downloads the pack once and keeps it when they change servers. Each server's build
arrives in `plugins/uxmbedrock/servers/<server>/` on the proxy; the proxy asks only for files it does not already
have, checks each by its hash, and makes the network pack in `plugins/uxmbedrock/output`, where the extension finds
it with `source.folder = auto`.

| Part | Merged |
|---|---|
| Packs built the same on several servers | Into one pack: JSON key by key, language files line by line, glyph pages pixel by pixel; any other file keeps the first server's copy |
| Packs only one server has | Kept as they are |
| Custom items | Joined, each Bedrock identifier once |
| Blocks, names, font slots | Joined key by key |
| Heads | All of them |
| A player's choices | From the server that wrote them last |

`pack.order` in the proxy's config.conf names the servers whose copy wins; the console names the files two servers
hold differently.

## Setting it up

1. Put uxmBedrock-Velocity in the proxy's `plugins/` and start the proxy. It writes `plugins/uxmbedrock/config.conf`
   and, in direct mode, `secrets.conf` with one secret per server.
2. On each server, set `server-name` to the server's name in velocity.toml in lower case, `network.mode = "direct"`,
   and `network.secret` to that server's secret. Restart the server.
3. The proxy's console says `The build ... from <server> is in` and `The network pack is made from ...`.

With Redis, set `network.mode = "redis"` and the same `redis-uri` on the proxy and every server; no secrets are
needed. The proxy uses port 25591, beside uxmChat's 25590. See [velocity config](../config/velocity-conf.md).

A player's choices and the glyph slots follow them across servers only when every server shares one database; see
[Database](../database/overview.md).

## Proxy commands

uxmBedrock-Velocity adds the first argument values of the proxy's own commands (server names, player names) to the
command list Bedrock players get, as the servers do for theirs. It asks Floodgate on the proxy, or Geyser, who plays
Bedrock.

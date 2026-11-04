---
title: Velocity
order: 609
description: The proxy plugin's config.conf and secrets.conf.
icon: server
---

uxmBedrock-Velocity writes `plugins/uxmbedrock/config.conf` on the proxy's first start. A change needs a proxy
restart.

## network

| Key | Default | Meaning |
|---|---|---|
| `mode` | `"direct"` | `direct`: the broker on this proxy; `redis`: through a Redis server, like a server |
| `host` | `"127.0.0.1"` | The address the broker listens on. Use an address the servers reach, or `0.0.0.0`, when they run elsewhere, and keep the port closed to the internet |
| `port` | `25591` | The port each server's `network.port` names |
| `redis-uri` | `"redis://127.0.0.1:6379"` | The Redis server, for `redis` |

## pack

| Key | Default | Meaning |
|---|---|---|
| `order` | `[]` | The servers whose copy wins where two servers' packs hold the same file differently, first first, by name. Others follow by name |
| `zip-level` | `9` | The compression of merged packs, 0 to 9 |
| `max-build-megabytes` | `1024` | The most one server's build may have; a larger build is refused |
| `merge-delay-seconds` | `5` | How long the proxy waits after a build arrives, so builds that arrive together are merged once |

## completions

| Key | Default | Meaning |
|---|---|---|
| `enabled` | `true` | Add the proxy commands' first argument values to Bedrock players' command list |
| `max-values` | `100` | The most values added per command, 1 to 500 |

## secrets.conf

In direct mode the proxy writes one secret per server it knows to `secrets.conf`, under the server's name in lower
case. Copy each into that server's `network.secret`. A secret already written is never changed; delete a line and
restart to make a new one. Never put this file in git.

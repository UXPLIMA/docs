---
title: Security
order: 196
description: What uxmBedrock touches, what leaves the server, which ports it opens and what it trusts from the network.
icon: lock
---

uxmBedrock changes what Bedrock players see. It never changes what Java players get, sends nothing about your
server anywhere, and opens no port unless you turn on a feature that needs one. This page lists every place where
that could matter.

## What it changes

| Part | Changes | Never |
|---|---|---|
| Packets | The text, menus, screens and entities a Bedrock player's connection carries | A Java player's connection: every rewrite runs only on a Bedrock player's channel |
| Other plugins' menus | How a menu reaches a Bedrock player: form, settings form or chest | The menu itself. A form button sends the click the plugin waits for, so the plugin's own checks run |
| Files | Its own folder, `plugins/uxmBedrock/`, and the Geyser extension's folder | Other plugins' files. Their packs are only read |

A server whose packet classes differ from the ones a polish was built for loses that one polish with a console line;
nothing else is touched.

## What leaves the server

| To | When | What |
|---|---|---|
| `piston-meta.mojang.com`, `resources.download.minecraft.net` | The first pack build | The Java client's font and vanilla pictures, kept in `cache/` |
| `sessionserver.mojang.com`, `api.mojang.com`, Mojang's texture server | A custom head is seen, a player is named | The skin of a head or a player, kept in `cache/skins/` |
| Maven Central | The first start | The database drivers |
| The address in `resource-pack` of `server.properties` | `sources.server-properties` is on | The pack Java players get, at most 512 MB |

Nothing else: no statistics, no license check, no update check. The pack is built on your server from the packs your
plugins built.

## Ports it opens

| Port | Opened when | Who it answers |
|---|---|---|
| The preview page, `8170` | `preview-page = true` under `modules` | Only requests carrying the secret its link holds, made new at every start. It listens on `127.0.0.1` unless you change `preview.host`. It serves the glyph pages and menu pictures of the last build, and a save is taken only for a picture and a screen that build knows |
| The pack host, `8171` | `pack-host.enabled` in the [extension's config](config/geyser-conf.md) | Anyone, for the packs of the current build only, each under the SHA-256 of its bytes. Any other request gets nothing |
| The network port, `25591` on the proxy | `network.mode = "direct"` | Servers that hold a secret from the proxy's `secrets.conf` |

## The network

In direct mode every frame a server sends is signed with that server's secret, and the proxy drops a frame it cannot
check. With `network.mode = "redis"` there are no secrets: whoever can write to the Redis server can offer a build, so
keep Redis on a private address with a password.

The proxy checks every build, whoever sent it:

| Check | Limit |
|---|---|
| The size of one build | `max-build-megabytes` in the [proxy config](config/velocity-conf.md), 1024 by default |
| The size of a pack once unzipped | 1 GiB |
| The files in a pack | 100,000 |
| A glyph page | 8192 pixels a side |
| Each file | Its hash against the manifest, and its name inside its server's folder |
| Chunks | In order; a build that sends nothing for five minutes is given up |

## Secrets

`network.secret` on each server, `secrets.conf` on the proxy and database passwords stay in the server's files.
Never put them in git, and never paste them into a support ticket. A secret the proxy wrote is never changed; delete
its line and restart the proxy to get a new one.

## Commands

Every `/uxmbedrock` node is given to operators only by default. The ones that write files are `gui measure`, the
preview page's save, `pack rollback`, `pack cache clear` and `menus report`; grant `uxmbedrock.admin.gui` and
`uxmbedrock.admin.pack` only to staff who may change the pack. See [Permissions](permissions/reference.md).

## What is stored about players

| Data | Where | Why |
|---|---|---|
| Each Bedrock player's choices: forms or chests, parts switched off, animated pack, language | The database | So a choice follows them across servers |
| Custom heads seen | The database and `output/skulls.json` | So Geyser can register them |
| The menus Bedrock players opened | Memory only, until a restart | `/uxmbedrock menus report`, which writes `reports/menus.json` when you run it |

## Loading protection

While a Bedrock player's client is still loading the packs, the Geyser extension answers the server's keep-alives
for it, so a phone loading a large pack is not dropped as timed out. It does so only until the player is in the
world, and for at most `loading-protection-seconds` (300 by default); a client that never arrives is dropped then, as
before. `0` turns it off.

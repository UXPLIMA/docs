---
title: config.conf
order: 601
description: This server on the network, the island grid, language, storage, player state, roles, homes, the REST API, backups and menu sounds.
icon: settings
---

uxmSkyblock. The settings of this server; every module has its own file under modules/.

Each start adds to this file, and to every file under modules/, the keys a new release brought. No value you wrote is changed. The file as it was is kept beside it as config.conf.bak. A key you deleted stays deleted: what each release shipped is kept under .defaults/, and only a key that is new since then is added. The file is written again from its contents, so your own alignment inside a block can move. To turn something off, use its switch rather than deleting it.

The file is `plugins/uxmSkyblock/config.conf`. Every default below is the shipped value.

## `network`

This server. On a single server nothing here needs a change but world-name, and only when the island world is not called world.

| Key | Default | What it does |
|---|---|---|
| `server-name` | `skyblock-node-default` | This server's name among the servers that share islands. Each server needs its own, and it must stay the same across restarts: the islands this server holds are recorded under it. |
| `world-name` | `world` | The world the islands live in on this node. It should be an empty world made by this plugin's generator, which bukkit.yml gives it: worlds: world: generator: uxmSkyblock Write that before the world is first made. A world that already has terrain keeps it. Folia makes no world while it runs, so on Folia the island world is the default world. The plugin says at startup when the island world is missing or is generated terrain. |
| `clustered` | `false` | Whether this node is one of several sharing a database and a Redis instance. A single server leaves this false and never needs redis-uri. |
| `redis-uri` | empty | Where the cluster's Redis lives. Required when clustered is true, ignored otherwise. |
| `authority-lease` | `10m` | How long this node's authority over an island lasts before it must be pushed forward. It is also how long an island waits before another node may pick it up after this one stops. A shorter lease recovers faster from a crash and asks the database for more. |
| `authority-heartbeat-interval` | `2m` | How often the lease is pushed forward. It must be shorter than authority-lease, and well inside it, so one missed beat does not cost the node its islands. |
| `route-cache-ttl` | `15s` | How long a route to another node stays cached. It bounds how long a visitor can be sent to a node that has since lost authority over the island they asked for. Only read when clustered. |

## `network.placement`

Where a visitor is sent when the node that held an island has stopped beating. The live nodes serving the island's world are offered to a strategy, which recommends one. The recommendation takes nothing: the node the visitor lands on takes the island through its lease.

| Key | Default | What it does |
|---|---|---|
| `strategy` | `least-loaded` | round-robin, least-loaded, mspt-aware or capacity-weighted. Another plugin may add its own. |
| `capacity` | `1000` | How many islands this node holds before it counts as full. |
| `mspt-ceiling` | `45.0` | The tick time, in milliseconds, above which mspt-aware avoids a node while another is under it. Folia keeps a tick time per region and none for the whole server, so a Folia node reports 0 and mspt-aware cannot tell Folia nodes apart. Use least-loaded or capacity-weighted there. |

## `grid`

Where islands stand in the world.

| Key | Default | What it does |
|---|---|---|
| `spacing` | `5120` | How many blocks apart the centres of two neighbouring islands are, on both axes. Islands are laid out on a square spiral from 0,0. Keep it above twice the largest radius an upgrade can reach. The default puts every island in regions of its own on Folia. This can only change while no island stands away from the centre. Once one does, the spacing those islands stand on is kept, and the start says so, because a new island at another spacing would land inside an old one. |
| `clear-margin` | `16` | How many blocks past everything an island wrote it is cleared when it is deleted, reset or recycled: its border, the grid, sea or cave its game mode laid past it, and its other worlds. The margin catches water, lava and trees that reached over the edge. |

## `language`

The language players read. Every file in messages/ is a language: drop in messages_de.conf and German is offered.

| Key | Default | What it does |
|---|---|---|
| `default` | `en` | The language every message falls back to when a player's own is not shipped. A file messages/messages_`<tag>`.conf must exist for this tag. |
| `follow-client` | `true` | true = a player reads the language their own client is set to, when we ship it. false = every player reads the default above, whatever their client says. |
| `force` | empty | A language every player reads, whatever they chose or their client says, such as "tr". Empty lets each player choose with /is lang. |

## `storage`

Where the plugin keeps islands, players and everything it remembers.

| Key | Default | What it does |
|---|---|---|
| `type` | `sqlite` | sqlite, mysql, mariadb or postgresql. sqlite needs nothing and keeps one file under data/. Servers that share islands use one mysql, mariadb or postgresql database. A change needs a restart. |
| `file` | `skyblock.db` | sqlite only: the file under data/. |
| `host` | `localhost` | The network database. port 0 is the database's own: 3306, or 5432 for postgresql. The environment variables SKYBLOCK_JDBC_URL, SKYBLOCK_DB_USER and SKYBLOCK_DB_PASSWORD, when set, are used instead of this block. |
| `port` | `0` |  |
| `database` | `minecraft` |  |
| `username` | `minecraft` |  |
| `password` | empty |  |
| `pool-size` | `10` | How many connections to the network database are kept open. |
| `lock-timeout` | `1s` | How long a statement on MariaDB, MySQL or PostgreSQL waits for a row another connection holds. A node that dies holding one keeps it until the database notices, and everything behind it waited: fifty seconds on MariaDB, for ever on PostgreSQL. Past this the statement fails and is tried again later; it never goes round the lock. MariaDB and MySQL count whole seconds. |
| `idle-transaction-timeout` | `5s` | PostgreSQL only: how long a transaction may stay open with nothing happening in it before the server ends it and gives its locks back. |
| `durability-profile` | `DEVELOPMENT` | What startup does when the database would lose a commit if the machine lost power: SQLite below synchronous FULL, MariaDB or MySQL with innodb_flush_log_at_trx_commit other than 1 (or sync_binlog other than 1 with the binary log on), PostgreSQL with fsync or synchronous_commit off. "DEVELOPMENT" warns in the console and starts. "PRODUCTION_STRICT" refuses to start. |

## `player-state`

How a player's inventory and state are saved while they play.

| Key | Default | What it does |
|---|---|---|
| `durability-mode` | `HYBRID` | Durability mode for player profile state. Supported values: "HYBRID" (combines write-ahead logs with periodic checkpoints). |
| `ambient-checkpoint-interval` | `60s` | Ambient periodic checkpoint interval for online players. Examples: "30s", "60s", "2m", "5m" |

## `notifications`

What happened to your island while you were away is kept, and read to you on your next join, in your own language. Nothing here is a sentence: the plugin stores which message to show and the names it fills in, so rewriting messages/messages_`<tag>`.conf rewrites old notices too.

| Key | Default | What it does |
|---|---|---|
| `read-retention` | `7d` | How long a notification a player has already read is kept before it is deleted. Set it to 0 to delete a notice the moment it has been read. |
| `sweep-interval` | `6h` | How often the notifications already read are swept out. The island activity feed is swept on the same pass. |
| `activity-retention` | `30d` | How long a line stays in an island's activity feed. The feed is a digest of what happened lately, not a ledger, so it is kept longer than a notice somebody has read and not forever. |
| `recovery-retention` | `7d` | How long a settled recovery record is kept. A write-ahead inventory journal and an economy saga exist so a crash in the middle of something can be finished or undone; once one has committed or been undone it has nothing left to recover. A journal a crash left unreconciled is never deleted, and neither is a saga that failed. |

## `roles`

Roles an island's owner makes beside owner, co-owner, moderator and member. The owner types /is permissions create `<name>`, or presses the button in the roles window, and edits the new role there like any other. Taking one away makes whoever held it a member.

## `roles.custom`

| Key | Default | What it does |
|---|---|---|
| `base` | `3` | How many roles of their own every owner may make, and the ceiling no permission can pass. |
| `max` | `10` |  |

## `roles.custom.permission-tiers`

Permission nodes that raise the allowance, written by you. The plugin names no rank: it reads the number off whichever of these nodes the owner holds, and the highest wins. Example:

```text
permission-tiers {
  "uxmskyblock.roles.5" = 5
}
```

## `homes`

Named island homes. A player sets a home with /is sethome `<name>` and travels to it with /is gohome `<name>`.

| Key | Default | What it does |
|---|---|---|
| `enabled` | `true` |  |
| `base` | `1` | What every player gets, and the ceiling no permission can pass. |
| `max` | `10` |  |

## `homes.permission-tiers`

Permission nodes that raise the allowance, written by you. The plugin names no rank: it reads the number off whichever of these nodes the player holds, and the highest wins. Example:

```text
permission-tiers {
  "uxmskyblock.homes.3" = 3
  "uxmskyblock.homes.5" = 5
}
```

## `homes.dimensions`

Where a home may be set, by dimension id: overworld, the_nether, the_end, or one a server adds. A dimension not named here takes no home.

```text
allowed         whether a home may be set there at all
on-island-only  whether it must stand within the player's own island
permission      a node the player must hold, or "" for none
```

## `homes.dimensions.overworld.the_nether.the_end`

| Key | Default | What it does |
|---|---|---|
| `homes.dimensions.overworld.the_nether.disabled-in-modes` | empty list | Game modes whose islands keep no named homes, such as ["oneblock"]. |

## `homes.dimensions.overworld.rest`

The REST API: a read mostly HTTP surface for web leaderboards, Discord bots and a web store deposit. It ships off on purpose: an HTTP port that opens itself, with a token that is identical on every copy of this plugin, is a hole rather than a feature.

| Key | Default | What it does |
|---|---|---|
| `enabled` | `false` | Turn the API on. It still refuses to start while bearer-token is the shipped value below. |
| `host` | `127.0.0.1` | The address to bind. Leave it on loopback unless a reverse proxy is in front of it. |
| `port` | `8080` |  |
| `bearer-token` | `change-me` | The token every request must carry as "Authorization: Bearer `<token>`". Change this. A node that is enabled while this still reads change-me does not open the port. |
| `idempotency-retention` | `24h` | How long a deposit's Idempotency-Key is remembered, so a retry of a deposit already made is answered rather than made again. Long enough for any retry a web store makes. |
| `idempotency-capacity` | `10000` | The most keys held at once, whatever the clock says. The key comes from the caller, so this is the ceiling on what a caller holding the token can make a node remember. |

## `homes.dimensions.overworld.island-backups`

Where island backups are kept: this server's disk, an S3 bucket, or both.

| Key | Default | What it does |
|---|---|---|
| `type` | `local` | Storage type: "local" (default), "s3", or "mirrored". "mirrored" writes every backup to the local folder and to the s3 bucket below. A backup counts as available only once both hold it, and a restore reads from whichever still has it. |
| `local-path` | `data/island-backups` | The folder on this server's disk, inside the plugin's folder. |
| `capture-timeout` | `30s` | How long a backup waits for every region to hand its chunks over. A backup that waits longer is given up whole and nothing of it is written. |

## `homes.dimensions.overworld.island-backups.s3`

Remote S3 settings (when type = "s3")

| Key | Default | What it does |
|---|---|---|
| `endpoint` | `https://s3.amazonaws.com` |  |
| `region` | `us-east-1` |  |
| `bucket` | `uxmskyblock-backups` |  |
| `access-key` | empty |  |
| `secret-key` | empty |  |
| `addressing-mode` | `PATH` |  |
| `provider-target` | `GENERIC_S3` |  |

## `homes.dimensions.overworld.leaderboard`

The island boards PlaceholderAPI reads: level, worth and bank. See the placeholders in the manual.

| Key | Default | What it does |
|---|---|---|
| `size` | `10` | How many places each board keeps. |
| `refresh-seconds` | `60` | How often, in seconds, the boards are read again. At least 5. |
| `empty-head` | `entity/player/wide/steve` | What the head of a place nobody holds is drawn from: a texture path, a player's name or an id. |

## `homes.dimensions.overworld.menu`

The sounds every window plays: click for a button that does something, page for a page arrow, denied for a button the player may not use. A sound is named by its vanilla key, because the key is what the client plays. Write an empty name to silence one. A button whose own actions play a sound in its menu file plays that sound alone. A reload reads them again.

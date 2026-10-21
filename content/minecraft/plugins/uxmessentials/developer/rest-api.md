---
title: REST API
order: 1670
description: HTTP and WebSocket access for panels, bots, webhooks and scripts.
icon: globe
---

HTTP and WebSocket access for the programs that are not plugins: a panel, a bot, a store webhook, a
script.

Everything the [Query API](queries.md) and the [Action API](actions.md) offer a plugin, the REST add-on
offers over HTTP. It is a separate jar, `uxmEssentials-rest`, and it ships switched off.

It is built on the published API and nothing else. Every endpoint below is one call to a surface a plugin could
make itself, which is the rule that keeps the two from drifting: if an endpoint cannot be written without reaching
into the plugin's internals, the gap gets filled in the published API rather than worked around here.

## Installing

Drop `uxmEssentials-rest.jar` into `plugins/` beside uxmEssentials and restart. Nothing opens: the add-on writes
`plugins/uxmEssentials-rest/config/rest.conf`, logs one line saying where the switch is, and stays dormant. A jar
that starts listening because it was installed turns "I tried the add-on" into an incident.

```hocon
enabled = false

# Where to listen. The default is the loopback address, which is where this belongs.
bind = "127.0.0.1"
port = 8123

# How many requests one token may make per minute. A token over the limit gets 429 until the minute rolls.
requests-per-minute = 120

# How many event-stream connections may be open at once, across every token.
max-subscribers = 8
```

Set `enabled = true` and restart.

<Callout type="warning" title="There is no TLS here">

The listener speaks plain HTTP. Leave it on `127.0.0.1` and put a reverse proxy in front of it if the outside
world needs to reach it. Binding to `0.0.0.0` is allowed and logs a warning once, because an operator with a
firewall and a proxy knows their network better than this plugin does.

</Callout>

## Tokens

Nothing is answered without a token, and tokens are made in game:

```
/uxmapi token create <label> [scopes]   issue one, shown once
/uxmapi token list                      what exists, without the secrets
/uxmapi token revoke <label>            stop one working
/uxmapi status                          whether it is listening, and who is connected
```

The secret is printed once, click-to-copy, and never again; what the server keeps is a hash. A secret an operator
has to paste into a config file ends up in a paste bin, a backup and a support ticket, so there is nowhere to paste
it. Lost one is revoked and reissued.

`/uxmapi` needs `uxmessentials.rest.admin`, which nobody has by default.

### Scopes

| Scope | What it opens |
|---|---|
| `read` | Every `GET` |
| `write` | Every `POST` |
| `events` | The event stream |

`/uxmapi token create panel` issues `read,events`, which is what a dashboard needs. Ask for more explicitly:
`/uxmapi token create shop read,write`.

A token without the scope a route needs gets `403` naming the scope, because that one is a configuration mistake
its holder can fix. A token nobody issued gets `401` and nothing else, because that one is not.

## Authenticating

Either header form works, and both end at the same lookup:

```bash
curl -H "Authorization: Bearer uxm_..." http://127.0.0.1:8123/api/v1/status
curl -u panel:uxm_... http://127.0.0.1:8123/api/v1/status
```

Basic is there because half the tools an operator already owns send it. The username has to be the token's label.

## The shape of an answer

Every answer is JSON with the same envelope:

```json
{ "ok": true, "data": { "currency": "coins", "amount": 1250.50 } }
```

An operation the server understood and declined is **not** an HTTP error. It comes back as `200` with `ok:false`
and the same failure code the Java API returns, so a consumer branches on the same string over HTTP as in process:

```json
{ "ok": false, "code": "insufficient-funds", "message": "not enough coins" }
```

The codes are the ones from `UxmFailure`: `cancelled`, `not-found`, `already-exists`, `player-offline`,
`insufficient-funds`, `already-in-state`, `refused`, `failed`.

HTTP statuses are kept for what HTTP is about:

| Status | When |
|---|---|
| `400` | A malformed body or path, with the field named |
| `401` | No token, or one nobody issued |
| `403` | A real token without the scope this route needs |
| `404` | No route answers that method and path |
| `405` | The route exists, but not for that method |
| `413` | A body longer than 64 KiB |
| `426` | An ordinary request to the event-stream path |
| `429` | Over `requests-per-minute`, with `Retry-After` |
| `503` | The module behind the route is switched off, or the subscriber cap is full |
| `504` | The server was asked and did not answer within ten seconds |

Three conventions run through every payload, and they are the same ones the Java views use. A time is an ISO-8601
instant. A duration is whole seconds, in a field named `-seconds`. A value that is absent is present and `null`
rather than missing, so the shape of an answer never depends on the data in it.

## Reading

Everything is under `/api/v1`. A module that is switched off answers `503` with `module-off` rather than an empty
list, so "nobody has a home" and "homes are off" are never the same answer.

### The server

| Route | Answers |
|---|---|
| `GET /status` | The version, the API version, and which modules are on |

### Economy

| Route | Answers |
|---|---|
| `GET /economy/currencies` | Every currency this server has |
| `GET /economy/top?limit=&currency=` | The leaderboard, up to 100 rows |
| `GET /players/{uuid}/balance?currency=` | One balance; the default currency without the parameter |
| `GET /players/{uuid}/balances` | Every currency this player holds |
| `GET /players/{uuid}/balance/afford?amount=&currency=` | Whether they hold at least that much |

Affordability is worth a route rather than two numbers subtracted client side: it is the same comparison the plugin
makes before charging, and a shop that works it out itself can disagree with the plugin, which shows up as a
purchase failing after the player was told it would go through.

### Homes

Slots in a path count from one, the way a player counts them.

| Route | Answers |
|---|---|
| `GET /players/{uuid}/homes` | The homes, with the count and the limit beside them |
| `GET /players/{uuid}/homes/{slot}` | One home, or `404` |

### Warps and player warps

| Route | Answers |
|---|---|
| `GET /warps?visible-to=` | Every warp, or only the ones that player may see |
| `GET /warps/{name}` | One warp, with its average rating |
| `GET /playerwarps?page=&limit=` | The public listing, a page at a time |
| `GET /playerwarps/{name}` | One player warp |
| `GET /players/{uuid}/playerwarps` | What that player owns, with the count and the limit |

### Kits and vaults

| Route | Answers |
|---|---|
| `GET /kits` | Every kit |
| `GET /kits/{id}` | One kit |
| `GET /players/{uuid}/kits` | Every kit with this player's cooldown and whether they may claim it |
| `GET /players/{uuid}/vaults` | The vaults, with the count, the limit and the row size |
| `GET /players/{uuid}/vaults/{index}` | One vault |

**Vault contents are not here.** Item stacks are a Bukkit type with no published form, and an inventory rendered as JSON would be a second,
worse item format that this project would then have to keep in step with Minecraft's own.

### Moderation

| Route | Answers |
|---|---|
| `GET /players/{uuid}/sanctions` | Ban, mute, jail and warnings, in one answer, each `null` when there is none |
| `GET /players/{uuid}/history?limit=` | What has been done to this player, newest first |

### Players

| Route | Answers |
|---|---|
| `GET /players/{uuid}/state` | God, flight, game mode and speeds, for a player who is online |
| `GET /players/{uuid}/playtime` | How long they have played |
| `GET /players/{uuid}/presence` | Whether they are away, since when, and why |
| `GET /presence/afk` | Everybody currently away |
| `GET /players/{uuid}/vanish?viewer=` | Whether they are hidden, at what level, and with `?viewer=` whether that viewer can see them |
| `GET /vanish` | Everybody hidden |

Vanish has levels, so "is this player hidden" and "is this player hidden from you" are different questions with
different answers. Passing `?viewer=` adds `visible-to-viewer` and saves a caller reimplementing the rule.

### Trade

| Route | Answers |
|---|---|
| `GET /trades` | Every trade open on this server |
| `GET /players/{uuid}/trade` | The trade they are in, or `null` |

Not being in a trade is an answer rather than a miss, so it is `null` in the envelope's `data` and not a `404`.

### Discord links

| Route | Answers |
|---|---|
| `GET /players/{uuid}/discord` | The Discord account bound to them, or `null` |
| `GET /discord/{id}` | The same binding read from the Discord side, or `null` |

Both answer `null` rather than `404`: not being linked is an answer. An id that is not a snowflake answers `null`
too, which is the honest reply to a bot passing along whatever a user typed.

### Ranks

| Route | Answers |
|---|---|
| `GET /ranks` | The ladder, in order |
| `GET /players/{uuid}/rank` | Their standing, the rung above it, and whether it is in reach |

`can-rank-up` is `false` for a player who is offline, because a rank requirement may read their inventory. The
`standing` key is `null` rather than missing when the ranks module holds nothing for them.

### Regions, snapshots and security

| Route | Answers |
|---|---|
| `GET /regions` | Whether region support is installed at all |
| `GET /worlds/{name}/regions` | Every region in the world, or those covering a point with `?x=&y=&z=` |
| `GET /worlds/{name}/regions/{id}` | One region, or `null` |
| `GET /players/{uuid}/snapshots` | The inventory snapshots held for them, newest first |
| `GET /players/{uuid}/security` | Which factors are on file, and whether they are locked out |
| `GET /players/{uuid}/skin` | The skin they chose, or `404` when they chose none |

Ask `GET /regions` before the others. With no WorldGuard installed every region read answers empty, which looks
exactly like a world nobody has protected; that route is what tells the two apart.

The covering set comes back highest priority first, which is the order that decides an overlap. A partial
coordinate is a `400`: a point needs all three. No factor material is in the security answer and none ever will be.

### Teleport, worlds, votes and messaging

| Route | Answers |
|---|---|
| `GET /players/{uuid}/teleport-requests` | Incoming and outgoing, in one answer |
| `GET /players/{uuid}/back` | Where `/back` would take them, or `404` |
| `GET /worlds` | Every managed world |
| `GET /worlds/{name}` | One world |
| `GET /worlds/{name}/access?player=` | Whether that player may enter, and why not when they may not |
| `GET /vote/top?period=&limit=` | The leaderboard for `all-time`, `monthly`, `weekly` or `daily` |
| `GET /vote/party` | How close the party is |
| `GET /players/{uuid}/votes` | Their totals and how many rewards are waiting |
| `GET /players/{uuid}/mail` | The mailbox, with the unread count |
| `GET /players/{uuid}/ignores` | Who they are ignoring |

### NPCs, holograms, staff and powertools

| Route | Answers |
|---|---|
| `GET /npcs` | Every NPC, or one player's with `?owner=` |
| `GET /npcs/{name}` | One NPC, or `404` |
| `GET /holograms` | Every hologram |
| `GET /holograms/{name}` | One hologram, or `404` |
| `GET /staff` | Everybody in staff mode right now |
| `GET /players/{uuid}/staff` | Whether they are on duty, and which mode |
| `GET /players/{uuid}/powertools` | Every bound item in their inventory, in slot order |
| `GET /players/{uuid}/powertools/held` | What the item in their main hand runs, or `null` |

Hologram text comes back as stored, before MiniMessage and before placeholders: a placeholder line reads
differently for every viewer, so there is no one rendered answer to send. The powertool routes read a live
inventory, so an offline player answers empty rather than `404`.

### The command gate and the sidebar

| Route | Answers |
|---|---|
| `GET /players/{uuid}/command-check?command=` | What the gate would do with that command, and which rule settled it |
| `GET /players/{uuid}/scoreboard` | Whether they have put their sidebar away |

The command is a query parameter rather than a path segment, because a command root can carry a `namespace:` prefix
and a colon in a path is a fight nobody needs to have. It is required: there is no useful default.

The answer carries `command`, `allowed`, `rule` (one of `BYPASS`, `WHITELISTED`, `NOT_WHITELISTED`, `BLACKLISTED`,
`NOT_BLACKLISTED`), `group` and `world`. The last two name what decided: the permission group whose own list was
read, and the world whose override applied, each null when the server-wide default did instead.

Both depend on the live player, so an offline one is a `404` for the command check and a null `hidden` for the
sidebar. The sidebar preference does survive their relog; it simply cannot be read while they are away.


## Writing

Every write is a `POST` with a JSON body, and every one of them maps onto a verb the Action API already has. There
is no `PUT` and no `DELETE`: one rule, `GET` reads and `POST` writes, is easier to hold than a taxonomy.

A write is attributed to the token that made it. A ban placed by the token labelled `panel` reads as
`uxmEssentials-rest/panel` in the audit log, so an operator can tell one caller from another.

### Economy

| Route | Body |
|---|---|
| `POST /players/{uuid}/balance/deposit` | `amount`, optional `currency` |
| `POST /players/{uuid}/balance/withdraw` | `amount`, optional `currency` |
| `POST /players/{uuid}/balance/set` | `amount`, optional `currency` |
| `POST /economy/transfer` | `from`, `to`, `amount`, optional `currency` |

```bash
curl -X POST http://127.0.0.1:8123/api/v1/players/$UUID/balance/deposit \
  -H "Authorization: Bearer $TOKEN" \
  -d '{"amount": 250.50}'
```

### Homes and warps

A location is an object: `world`, `x`, `y`, `z`, and optionally `yaw` and `pitch`, which default to zero.

| Route | Body |
|---|---|
| `POST /players/{uuid}/homes/{slot}/set` | `location` |
| `POST /players/{uuid}/homes/{slot}/move` | `location` |
| `POST /players/{uuid}/homes/{slot}/rename` | `label` |
| `POST /players/{uuid}/homes/{slot}/delete` | none |
| `POST /warps/{name}/create` | `location` |
| `POST /warps/{name}/move` | `location` |
| `POST /warps/{name}/delete` | none |

`set` writes the slot whether or not something was there; `move` fails when nothing is.

### Kits

| Route | Body | What it does |
|---|---|---|
| `POST /players/{uuid}/kits/{id}/give` | none | Hands it over, ignoring cooldowns, costs and permissions |
| `POST /players/{uuid}/kits/{id}/claim` | none | Claims it as the player would, cooldown and all |

`give` is what a reward system wants; `claim` is what a panel's claim button wants, so a player cannot use a web
page to get around a cooldown.

### Moderation

| Route | Body |
|---|---|
| `POST /players/{uuid}/ban` | optional `reason`, optional `duration-seconds` |
| `POST /players/{uuid}/unban` | none |
| `POST /players/{uuid}/mute` | optional `reason`, optional `duration-seconds` |
| `POST /players/{uuid}/unmute` | none |
| `POST /players/{uuid}/kick` | optional `reason` |
| `POST /players/{uuid}/warn` | `reason` |
| `POST /players/{uuid}/jail` | `jail`, optional `reason`, optional `duration-seconds` |
| `POST /players/{uuid}/unjail` | none |

A `duration-seconds` makes it temporary; without one it is permanent. A warning always says why: an entry on
somebody's record with no reason on it is not worth writing.

Every one of these also takes an optional `silent`, which suppresses the server-wide announcement and nothing else:
the punishment still lands, is still audited, and the player is still told. It is what an anti-cheat or a panel
writing its own announcements wants, so the server is not told twice.

### Players

| Route | Body |
|---|---|
| `POST /players/{uuid}/state/god` | `enabled`, defaulting to `true` |
| `POST /players/{uuid}/state/fly` | `enabled`, defaulting to `true` |
| `POST /players/{uuid}/state/gamemode` | `mode`: survival, creative, adventure or spectator |
| `POST /players/{uuid}/state/walk-speed` | `multiplier` |
| `POST /players/{uuid}/state/fly-speed` | `multiplier` |
| `POST /players/{uuid}/state/heal` | none |
| `POST /players/{uuid}/state/feed` | none |
| `POST /players/{uuid}/presence/afk` | `reason`, or `away` defaulting to `true` |
| `POST /players/{uuid}/vanish` | `vanished`, defaulting to `true` |

One route per switch rather than one that takes them all, because "god mode on, flight refused, speed out of range"
is not something a single answer can say honestly.

### Ranks

| Route | Body |
|---|---|
| `POST /players/{uuid}/rank/rankup` | none |
| `POST /players/{uuid}/rank/set` | `rank` |
| `POST /players/{uuid}/rank/prestige` | none |

`rankup` and `prestige` charge the cost and check the requirements the same way the commands do, and answer
`player-offline` for somebody who is not there. `set` does neither and works for an offline account.

### Discord links

| Route | Body |
|---|---|
| `POST /players/{uuid}/discord/unlink` | none |

There is no route that creates a binding. A link is only real once the player proved it in game with a one-time
code, so the only write here is the removal, which answers `not-found` for somebody who was never linked.

### Snapshots and security

| Route | Body |
|---|---|
| `POST /players/{uuid}/snapshots/restore` | `snapshot` |
| `POST /players/{uuid}/security/force` | none |
| `POST /players/{uuid}/security/unlock` | none |

A restore takes the same safety copy the command does and needs the player online. `force` forgets the account's
trusted devices so its next join proves the factor again; `unlock` ends a lockout early. There is no route that
enrols or clears a factor.

### Vaults

| Route | Body |
|---|---|
| `POST /players/{uuid}/vaults/{index}/open` | none |
| `POST /players/{uuid}/vaults/{index}/label` | `name`, `icon`, or both |
| `POST /players/{uuid}/vaults/{index}/delete` | none |

`open` puts the real vault window in front of its owner, who has to be online for there to be a window. Nothing
here moves items: an inventory rendered as JSON would be a second, worse item format, and the window is where the
item policy and the save-on-close live.

On `label`, a field left out is left alone and a field sent as `null` is cleared, so one call can name a vault and
drop its icon. A body with neither field is a `400`. The icon is a Bukkit material name; one that does not exist
answers `refused`.

### Player warps

| Route | Body |
|---|---|
| `POST /players/{uuid}/playerwarps` | `name`, `location` |
| `POST /playerwarps/{name}/relocate` | `actor`, `location` |
| `POST /playerwarps/{name}/rename` | `actor`, `new-name` |
| `POST /playerwarps/{name}/archive` | `actor` |
| `POST /playerwarps/{name}/restore` | `actor` |
| `POST /playerwarps/{name}/delete` | `actor` |

Every write names the player it acts as, in the body as `actor`, because a warp's rules are written in terms of a
person: the owner may remove it, a manager may move it, a stranger may do neither. A token is not a person, so it
has to say which person it stands for. The create route takes its actor from the path instead, since a new warp's
actor is its owner.

`archive` is the recoverable one and what a cleanup should reach for: the warp leaves the listings and nobody can
travel to it, while it and everything hanging off it survive until `restore`. `delete` frees the name and takes the
whitelist, the bans, the earnings and the history with it.

### Teleport, worlds, votes and messaging

| Route | Body |
|---|---|
| `POST /players/{uuid}/teleport` | `location` |
| `POST /players/{uuid}/back` | none |
| `POST /worlds/{name}/load` | none |
| `POST /worlds/{name}/unload` | `save`, defaulting to `true` |
| `POST /players/{uuid}/votes` | `amount`, defaulting to `1` |
| `POST /vote/party` | `votes`, defaulting to `1` |
| `POST /messaging/message` | `from`, `to`, `body` |
| `POST /players/{uuid}/mail` | `body`, optional `from` |

Mail without a `from` comes from the server: it waits in the mailbox rather than being lost to an offline player,
and no mute or ignore applies to it, because neither can be about a plugin.

### NPCs and holograms

| Route | Body |
|---|---|
| `POST /npcs` | `actor`, `name`, `location` |
| `POST /npcs/{name}/move` | `actor`, `location` |
| `POST /npcs/{name}/skin` | `actor`, `skin` (the account to copy; null takes the skin off) |
| `POST /npcs/{name}/name` | `actor`, `name` (null shows the id again) or `hidden: true` (show nothing) |
| `POST /npcs/{name}/command` | `actor`, `command` (null unbinds it) |
| `POST /npcs/{name}/delete` | `actor` |
| `POST /holograms` | `actor`, `name`, `location`, `line` |
| `POST /holograms/{name}/move` | `actor`, `location` |
| `POST /holograms/{name}/lines` | `actor`, and see below |
| `POST /holograms/{name}/command` | `actor`, `command` (null unbinds it) |
| `POST /holograms/{name}/delete` | `actor` |

Every one of these names an `actor` in its body rather than taking it from the token. The plugin runs the same use
case the command runs, and that use case is written for a person: a create is charged to the actor's NPC limit and
records them as the owner. A token is not a player, and an NPC nobody owns is one an operator cannot ask anybody
about.

The one line route says all three edits. No `line` number adds `text` to the bottom. A `line` number with `text`
replaces that line, and a `line` number without it removes that line. Numbers count from one. Removing the last
remaining line is refused: delete the hologram instead.

### The sidebar, tab list and nametags

| Route | Body |
|---|---|
| `POST /players/{uuid}/scoreboard` | `hidden` (defaults to true) |
| `POST /players/{uuid}/scoreboard/refresh` | none |
| `POST /players/{uuid}/tablist/refresh` | none |
| `POST /players/{uuid}/nametag/refresh` | none |

All three displays repaint on their own timer, so a panel that has just changed a rank or a placeholder does not
have to do anything for the change to show up. The refresh routes are for when that wait is too long. The nametag
refresh re-selects which format the player wears before redrawing, so a player who should now wear a different one
does.

Putting the sidebar away writes the same durable preference `/scoreboard` flips, and asking for the state the player
is already in comes back as `already-in-state`. The tab list and nametags have no equivalent: what they say is
authored in config, nothing outside the module owns a row or a tag to set, and anything cleared from here would be
repainted on the next pass.

**Trade, regions, staff, powertools and the command gate are read-only.**

Each publishes a query surface and no action surface, so over HTTP they are readable and nothing more. That is
the published API's shape showing through rather than a decision taken in the add-on. The command gate is the
plainest of them: the rules are the operator's config file, and a panel that wanted to change them would be
editing that file rather than posting here. For regions the reason is
worth saying out loud: editing a protection is an operator act with its own command and its own audit trail, and a
protection changed by a plugin would leave staff looking at something nobody in the logs ever did. Staff mode is
the same kind of case: entering it swaps a real inventory for a loadout, and only the module can be trusted to put
the real one back. A powertool binding is stamped onto the item a player is holding, and there is no held item in
an HTTP request.

The tab list and nametags go the other way: they publish a verb and nothing to read, so over HTTP they can be
refreshed and not queried.

## The event stream

Polling to find out whether anything happened is what this replaces. Connect a WebSocket to `/api/v1/events` with a
token carrying the `events` scope:

```javascript
const socket = new WebSocket("ws://127.0.0.1:8123/api/v1/events", {
  headers: { Authorization: "Bearer " + token }
});
```

The server greets you, and then says nothing until you subscribe:

```json
{ "event": "connected", "caller": "panel", "message": "subscribe to start: ..." }
```

```javascript
socket.send(JSON.stringify({ subscribe: ["economy.*", "moderation.player-warn"] }));
```

```json
{ "event": "subscribed", "topics": ["economy.*", "moderation.player-warn"] }
```

Silence until you ask is deliberate: a connection that forgot to subscribe and is quietly handed every event on the
server is a bandwidth bill rather than a feature. Patterns are an exact name, one context with a trailing star, or
`*` on its own. `{"unsubscribe": [...]}` takes them off again, matched as they were written.

Every event arrives in the same envelope:

```json
{
  "event": "economy.wallet-credit",
  "data": {
    "player-id": "...", "player-name": "steve",
    "amount": { "currency": "coins", "amount": 25.00 },
    "balance": { "currency": "coins", "amount": 125.00 },
    "transaction-id": "...", "occurred-at": "2026-01-02T03:04:05Z"
  }
}
```

The payload is the event's own fields, rendered by the same code the reads use, so a location is a location and a
money is a money whichever way it arrived.

### What is carried

Eighty-seven events, named `context.thing`. Every notification event uxmEssentials publishes is on the stream:

| Context | Names |
|---|---|
| `communication` | `announcer-reload`, `broadcast-opt-out` |
| `discordlink` | `account-link`, `account-unlink` |
| `economy` | `wallet-credit`, `wallet-debit`, `wallet-reject`, `bank-deposit`, `bank-withdraw`, `loan-disburse`, `loan-repay` |
| `hologram` | `create`, `delete` |
| `home` | `create`, `delete`, `relocate`, `rename`, `icon-change`, `visibility-change`, `limit-reached` |
| `invrollback` | `inventory-restore` |
| `itemworld` | `entity-purge`, `mob-spawn` |
| `kit` | `claim` |
| `messaging` | `private-message`, `mail-deliver`, `help-op` |
| `moderation` | `player-tempban`, `player-mute`, `player-unmute`, `player-jail`, `player-unjail`, `player-warn`, `ip-ban`, `alt-detected`, `jail-location-define`, `jail-location-remove` |
| `npc` | `create`, `delete`, `move` |
| `playerstate` | `player-god-toggle`, `player-fly-toggle`, `player-game-mode-change`, `player-speed-change`, `player-heal`, `player-feed` |
| `playerwarp` | `create`, `delete` |
| `pose` | `pose` |
| `presence` | `afk` |
| `trade` | `complete`, `cancel` |
| `rank` | `up`, `set`, `prestige` |
| `scoreboard` | `visibility` |
| `security` | `verification-pass`, `verification-fail`, `lockout` |
| `skin` | `change` |
| `staff` | `chat`, `mode` |
| `teleport` | `player-teleport`, `back-location-capture`, `warmup-start`, `warmup-cancel`, `request-send`, `request-accept`, `request-deny`, `request-cancel`, `request-expire` |
| `vanish` | `toggle` |
| `vault` | `open`, `contents-change` |
| `vote` | `receive`, `party` |
| `warp` | `create`, `delete` |
| `world` | `create`, `load`, `unload`, `delete`, `import`, `adopt`, `unregister`, `setting-change`, `entry-denied` |

The nine cancellable pre-events from the [Events](events.md#what-you-can-cancel) reference are deliberately not
carried. Their whole point is the chance to veto, and a subscriber on the far end of a socket cannot answer inside
the tick that asked; sending them would be a veto point this add-on cannot honour, and would report every action
twice besides. A plugin that needs to refuse something needs to be a plugin.

### Connection notes

- The server pings a connection that has been quiet for a minute. Answer with a pong, which every WebSocket
  library does for you, and reply to a ping of your own with the same.
- Only whole text messages are read. A fragmented message is refused with close code `1003`, and so is a frame over
  64 KiB.
- `max-subscribers` caps how many streams may be open at once. Past it the handshake is refused with `503` and
  `too-many-subscribers`, because every open stream is a socket and a thread held for as long as the client wants
  them.
- On shutdown every subscriber gets a close frame with code `1001` rather than a dropped connection.

## What it does not do

- **No TLS.** Put a reverse proxy in front of it.
- **No pagination beyond what is listed.** The leaderboards and the public player-warp listing are the only
  unbounded reads, and both take a `limit` capped at 100.
- **No item stacks**, in a vault or anywhere else. See the note above.
- **No veto.** Cancelling something requires being in the tick that asked, which requires being a plugin. Use the
  [Events](events.md) API.

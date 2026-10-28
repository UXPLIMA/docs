---
title: Two-factor login
order: 33
description: Approving a login from Discord, the checks before the request, the freeze, country rules and what a denial does.
icon: shield-check
---

With 2FA on, a linked player who joins is frozen and their Discord account gets a direct message
asking whether this login is theirs. **Yes** lets them play; **No** kicks or bans them; no answer
kicks them when the time runs out. It is the strongest protection against a stolen Minecraft
account, above all on staff accounts.

Everything is in `two-factor.yml`, and it ships turned off.

## Who is asked

Only linked players. A player without a link has no Discord account to ask and joins as usual, so
2FA protects exactly the accounts that are linked. Link your staff accounts before relying on it.

With `require-permission: true`, only players holding `uxmdiscordsync.2fa.required` are asked. That
is the way to use 2FA for staff alone.

## What happens on join

The player is frozen from the moment they join, while these checks run in order:

| Check | Result |
|---|---|
| The login comes from a country in `country-rules.deny` | Kicked without asking |
| The address is in `ip-whitelist` | Let in without asking |
| The same player was approved from the same address within `session.duration` | Let in without asking |
| With several servers, the network cannot be reached | Kicked, because the answer could not come back |
| A request was sent less than `request-cooldown` seconds ago | Kicked with the seconds left |
| None of the above | The request is sent and the player waits, frozen |

A login from a country in `country-rules.always-ask` skips the whitelist and the session and is
always asked.

The request shows the player, the server, the address, the city and country, and the time. The
message is updated while it waits: **Waiting**, then **Approved**, **Denied** or **Timed out**. Only
the newest request counts; pressing a button on an older one does nothing.

## Settings

| Setting | Default | Meaning |
|---|---|---|
| `enabled` | `false` | Turns 2FA on |
| `require-permission` | `false` | Ask only players with `uxmdiscordsync.2fa.required` |
| `timeout` | `120` | Seconds to answer. Checked every five seconds |
| `request-cooldown` | `30` | Seconds before the same player can get another request. `0` turns it off |
| `session.enabled` | `false` | Remember an approval for the same player and address |
| `session.duration` | `30` | Minutes an approval is remembered |
| `session.scope` | `network` | `network`: on every server of a [multi-server](multi-server.md) network. `server`: only where it was approved |
| `ip-whitelist` | `[]` | Addresses or ranges that are never asked, such as `"203.0.113.7"` or `"192.168.1.0/24"` |

Remembered sessions of a single server are lost when it restarts.

## The freeze

A waiting player is always frozen. `freeze-restrictions` choose what they cannot do:

| Setting | Blocks |
|---|---|
| `movement` | Walking, teleporting, riding. A player who joins on a mount is taken off it |
| `commands` | Commands and chat |
| `drop-items` | Dropping items |
| `pickup-items` | Picking items up |
| `damage` | Taking damage and dealing it |
| `interact` | Breaking and placing blocks, using items and entities, eating, shooting, the inventory |

Leave all six on. A frozen player who can be hurt can be killed while their phone is still in their
pocket, and one who can type commands can leave before anyone has said yes.

## Denied and timed out

```yaml
  on-deny:
    kick-player: true
    kick-message: "<red><bold>LOGIN DENIED</bold></red>"
    ban-player: false
    ban-duration: "24h"
    ban-reason: "Unauthorized login attempt"
    notify-admins: true
    admin-permission: "uxmdiscordsync.admin.notify"

  on-timeout:
    kick-player: true
    kick-message: "<yellow>Verification timeout.</yellow>"
```

**No** means the owner of the account says someone else is using it. `ban-player: true` bans the
account on this server's ban list for `ban-duration`, written as `30m`, `24h`, `7d`, `1d12h` or
`permanent`; the ban also kicks. An unreadable duration bans for 24 hours and says so in the console.
To share the ban across servers, use your ban plugin.

`notify-admins` writes the denial to the console and to every online player holding
`admin-permission`, with the player's name and address.

A timeout is usually a player who did not see the message, not an attack, so it only kicks. With
`kick-player: false` in either section, the player stays frozen until they leave.

## Location and country rules

`%location%` and the country rules need MaxMind's free GeoLite2 database. Make an account at
[MaxMind](https://dev.maxmind.com/geoip/geolite2-free-geolocation-data), download
`GeoLite2-City.mmdb` (or `GeoLite2-Country.mmdb` for the country only) and put it where
`geoip-database` points. A relative path starts from the server folder. MaxMind updates it monthly;
replace the file and run `/dsadmin reload`.

| Setting | Meaning |
|---|---|
| `use-geoip` | Look up where each login comes from |
| `country-rules.warn-on-change` | Warn in the request when the country differs from the last approved login |
| `country-rules.always-ask` | Two-letter codes such as `TR` or `DE` that are always asked |
| `country-rules.deny` | Countries refused without asking |

Without the database the location shows as unknown, and the country rules do nothing.

<Callout type="danger" title="Behind a proxy, check the address first">

On a proxy without IP forwarding, every player reaches the server from the proxy's address, often
`127.0.0.1`. A whitelist entry for it lets everyone skip 2FA, and the location and country rules see
only the proxy. Turn on IP forwarding in the proxy and in Paper before you use the whitelist.

</Callout>

## The request message

`embed` takes the keys listed in [Concepts](../getting-started/concepts.md#embeds) and these
placeholders: `%player_name%`, `%player_uuid%`, `%server_name%`, `%ip_address%`, `%location%`,
`%session_code%`, `%timestamp%`, `%timeout%` and `%status%`. The words for `%status%` are in
`status`, the button texts in `buttons`.

## Before you turn it on

Try it with your own linked account first. A member who blocks direct messages from server members
never gets the request and is kicked at the timeout, staff included. Keep `timeout` long enough for a
phone notification to arrive.

Related: [Account linking](account-linking.md), [Permissions](../permissions.md)

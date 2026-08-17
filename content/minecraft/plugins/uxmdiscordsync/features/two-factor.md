---
title: Two-factor login
order: 33
icon: shield-check
---

With 2FA on, a linked player joining the server is frozen and a Discord DM asks whether the login is
theirs. Approve and they play; deny and they are kicked or banned; ignore it and the timeout kicks
them.

It is the single strongest protection against a stolen Minecraft session on a staff account, and it
lives in `two-factor.yml`.

## Enabling it

```yaml
two-factor:
  enabled: false
  timeout: 120
  freeze-player: true
  use-geoip: true
  geoip-database: "plugins/uxmDiscordSync/GeoLite2-City.mmdb"
```

Only players who have linked an account can be challenged — the plugin needs a Discord user to ask.

## The freeze

```yaml
freeze-restrictions:
  movement: true
  commands: true
  drop-items: true
  pickup-items: true
  damage: true
  interact: true
```

Each restriction is enforced separately, and `damage: true` covers both taking and dealing it. Leave
all six on: a frozen player who can still be hurt can be killed while waiting for a phone
notification, and one who can still run commands can `/home` out of whatever situation the challenge
was protecting.

Block breaking and placing are blocked with `interact`.

## The prompt

```yaml
embed:
  color: "#ffaa00"
  title: "🔐 Login Verification Required"
  description: |
    **Player:** %player_name%
    **Server:** %server_name%
    **IP Address:** %ip_address%
    **Location:** %location%
    **Session Code:** %session_code%
    **Time:** %timestamp%
  thumbnail: "https://mc-heads.net/avatar/%player_name%/64"
  footer: "You have %timeout% seconds to respond"

buttons:
  approve: "✅ Yes, this is me"
  deny: "❌ No, this is not me"
```

`%ip_address%` and `%location%` are what make the prompt useful — a login from a city the player has
never been to is the signal they act on. `%location%` requires GeoIP.

## GeoIP

`use-geoip: true` needs MaxMind's free GeoLite2-City database, downloaded from
[MaxMind](https://dev.maxmind.com/geoip/geolite2-free-geolocation-data) and placed at the path in
`geoip-database`. Without the file, `%location%` is empty and everything else still works.

The database is updated by MaxMind periodically; a stale copy gives progressively vaguer answers
rather than wrong ones.

## Denial and timeout

```yaml
on-deny:
  kick-player: true
  kick-message: "..."
  ban-player: false
  ban-duration: "24h"
  ban-reason: "Unauthorized login attempt"
  notify-admins: true
  admin-permission: "uxmdiscordsync.admin.notify"

on-timeout:
  kick-player: true
  kick-message: "..."
```

Deny means the player is saying *this is not me* — someone else has their session. A kick stops that
login; `ban-player: true` with a duration stops the next fifty. `24h` is a reasonable default, and
`permanent` is available for staff accounts.

Timeout is treated more gently, and correctly so: the usual cause is a player who did not see the
DM, not an attacker.

`notify-admins: true` messages everyone holding `uxmdiscordsync.admin.notify`, which is how you find
out that an account is under attack while it is happening.

## IP whitelist

```yaml
ip-whitelist:
  - "127.0.0.1"
  - "192.168.0.0/16"
  - "10.0.0.0/8"
```

Addresses that skip the challenge, CIDR supported. The defaults are the private ranges, which is
what stops your own proxy or a LAN test client from being challenged on every join.

<Callout type="danger" title="Do not whitelist a public range">

Every address in this list can join a linked account without verification. The defaults are private
ranges that cannot be reached from the internet. Adding a public range, or a player's home address
as a convenience, removes the protection for anyone who can reach it.

</Callout>

## Before you enable it

Test with your own account first. A 2FA system with a wrong guild id, a bot that cannot DM, or a
`timeout` shorter than a phone notification takes to arrive will lock every linked player out of the
server, staff included — and the fix requires editing the config and restarting.

Set `freeze-player: true` and a generous `timeout` for the first day.

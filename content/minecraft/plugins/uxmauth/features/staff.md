---
title: Staff tools
order: 311
description: Info, history, perf, console-only commands, alerts, Discord, the login history and the purge.
icon: user-cog
---

## Reading an account

`/uxmauth info <player>` opens the [info window](../menus/info.md): the account, its last login, its security, its
latest `staff.recent-logins` (10) logins and up to `staff.alts-shown` (20) other accounts seen on its addresses. Dates
read in `staff.date-format`. `/uxmauth history <player>` lists the latest logins in chat. `/uxmauth perf` shows the time
spent hashing, querying the database and asking Mojang.

## Console only

`forcelogin`, `unregister`, `setpassword`, `disable2fa`, `markpremium`, `markcracked` and `resolve` run only from the
console unless `staff.in-game` lists them. A staff account somebody took over then cannot unregister the other staff,
or hand an account to whoever waits under its name.

## Alerts

Staff holding `uxmauth.admin.alerts` hear, on every server of a network:

| Key under `alerts` | Default | Hears |
|---|---|---|
| `failed-login` | `true` | A burst of wrong passwords from one address, one line each `failed-login-window-seconds` (60) |
| `ban` | `true` | The ban that ends it |
| `shared-address-accounts` | `3` | A new account logging in from an address that many accounts have used, itself among them; 0 turns it off |
| `staff-actions` | `true` | Every staff command that changed something, with who ran it |

## Discord

`discord.url` is a webhook; nothing is sent while it is empty. Each event is switched on its own with a message of yours:

| Event | Shipped message |
|---|---|
| `register` | `{player} registered on {server} from {address}.` |
| `login` | `{player} logged in on {server} by {method} from {address}.` |
| `known-address` | `{player} is a new account on {address}, which {accounts} accounts have used, on {server}.` |
| `ban` | `{address} is banned on {server} for {time} after wrong passwords.` |
| `conflict` | `Staff settled the name {player} on {server}.` |

`{address}` is masked to its first two parts, `203.0.x.x`. A message never pings anybody. Messages wait on a thread of
their own; when too many wait, the oldest is dropped with a console line. `discord.server-name` is what `{server}` says.
A ban made at a proxy is not sent; one made on a server is.

## The login history

Every login is kept with its time, address and way in for `history.keep-days` (90), deleted at start and once a day. It
holds players' addresses: mention it in the server's privacy policy.

## Notices to the player

`notices.last-login = true`: after a login the player reads the date and masked address of their previous one, so they
notice a login that was not theirs.

## Purge

| Key under `purge` | Default | Meaning |
|---|---|---|
| `enabled` | `false` | Removes accounts nobody logged in to for `after-days`, with everything under them |
| `after-days` | `180` | An account that never logged in counts from its registration |
| `keep-premium` | `true` | Spares premium accounts |
| `check-hours` | `24` | It says what it removed in the console |

On a network every server writes its logins to one table, so a login on any of them keeps the account.

Related: [Staff commands](../commands/staff.md), [Info window](../menus/info.md)

---
title: Protection
order: 306
description: Address bans, attempt limits, connection and account limits, shared-address filters and countries.
icon: shield
---

Everything here is in [protection.conf](../config/protection-conf.md), except `passwords.max-attempts`. A reload applies
it.

## Address bans

Wrong passwords ban the address they came from. Every `bans.failures` (3) wrong passwords earn the next step of
`steps-minutes` (5, 30, 60, 1440), and the last step repeats. An address quiet for `forgive-after-hours` (24), counted
from its last failure or the end of its ban, starts again from the first step. An IPv6 address counts by its /64, as a
home is handed a whole /64. `/uxmauth unban <address>` lifts a ban.

`passwords.max-attempts` (3) kicks a player after that many wrong passwords on one connection. A wrong second-factor
code costs a try the same way and counts toward the bans.

## Limits

| Block | Default | Counts |
|---|---|---|
| `connections` | 5 per address in 60 seconds | Every connection whatever comes of it, apart from wrong passwords. A bot that registers never fails a login, so only this catches it. A refused connection counts too |
| `accounts` | 3 per address in 24 hours | Password accounts made. Premium and Bedrock players are proved already and neither counted nor refused |

`exempt-addresses` in both takes one address or a range with its prefix: your own proxies, or a village, school or
mobile network that shares one IPv4 address.

## Shared addresses

Behind a filter such as TCPShield every player arrives from the filter's address.

| Key | Meaning |
|---|---|
| `shared-address-hosts` | The hosts the filter answers for: `*.tcpshield.com`; empty means every host |
| `shared-address-sources` | The addresses the filter connects from: `198.51.100.0/24` |

A player types the host, so a host alone proves nothing: a connection counts as through the filter only when one of the
sources made it. Through it, sessions and every rule that reads the address are off. With hosts and no sources nothing
counts, and the console says so at every start. When the filter hands the server each player's own address, as
TCPShield's plugin does, use that and leave both empty.

## Countries

| Key under `countries` | Default | Meaning |
|---|---|---|
| `mode` | `off` | `off`, `allow` (only listed countries), `deny` (listed countries stay out) |
| `list` | `[]` | Two-letter codes: `["TR", "DE"]` |
| `allow-addresses` | `[]` | Let in whatever their country |
| `unknown` | `allow` | An address the file cannot place |
| `account-id`, `licence-key` | `""` | Your free MaxMind account; with both, the file is fetched into the data folder |
| `refresh-days` | `7` | When the file is fetched again |

Without the key, drop `GeoLite2-Country.mmdb` into the data folder yourself. With no file at all, every country is let
in and the console says so once.

Related: [protection.conf](../config/protection-conf.md), [Captcha](captcha.md)

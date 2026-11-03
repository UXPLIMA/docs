---
title: Premium names
order: 404
description: Checking a name at Mojang, the four ownership rules, the four UUID schemes, settling a name two players want, and the backup password.
icon: badge-check
---

A name that exists at Mojang can be held by a paid player or by a player without a paid account. These settings decide
who gets it and what happens when two players want it: `premium` in `config.conf`, and `ownership.conf`.

## How a name is checked

`premium.sources` lists the services asked, in order: `mojang`, `playerdb`, `minetools`, `ashcon`. The first that can
answer decides; one that is down, slow or refusing passes the name to the next.

| Key under `premium` | Default | Meaning |
|---|---|---|
| `cache-minutes` | `30` | How long an answer is kept; an answer no source could give is never kept |
| `mojang-cap-per-10-minutes` | `550` | Mojang refuses past 600, so the mirrors answer the rest |
| `timeout-millis` | `3000` | How long one source may take |
| `addresses` | the four services | Point a source at a mirror of your own; `{name}` is the name |
| `backup-password` | `true` | A premium account with no password gets a random one at its first premium login, shown once and stored as a hash |

The handshake runs on a lone server only in offline mode and not behind a proxy. In online mode the server proves every
player, and behind a proxy the handshake belongs to the proxy; the console says which applies.

## Ownership rules

`ownership.rule`:

| Rule | Behaviour |
|---|---|
| `automatic` | A new name that exists at Mojang is asked for the handshake once. A paid player is in without a password. A player who fails joins with a password from the next connection and keeps the name until its paid owner proves it with `/claim` or the button on the login screen |
| `opt-in` | Everybody joins with a password. A player who types `/premium` is asked for the handshake from the next connection on |
| `strict` | Every name that exists at Mojang must pass the handshake. A cracked player can never hold a paid name |
| `domain` | The address a player joins through decides: `domains.premium` for paid accounts, `domains.cracked` for the rest. An address on neither counts as cracked |

| Key | Default | Meaning |
|---|---|---|
| `when-every-source-fails` | `cracked` | A new name no source could place: `cracked` (marked unverified and asked again next time), `premium`, or `refuse` |
| `failed-memory-seconds` | `300` | A connection that failed the handshake is told why when it comes back, and under `automatic` joins with a password |
| `premium-password-fallback` | `true` | A premium account that keeps a password logs in with it after a failed handshake: Mojang is down or the account is lost |
| `claim-minutes` | `10` | How long a claim waits for the next connection from the same address; only that address is asked |

## The domain rule's mark

Under `domain`, a cracked name gets `domains.cracked-mark` (`+`) at `mark-at` (`end`), so `Notch` through the cracked
address plays as `Notch+` beside the paid `Notch`. The longest cracked name is 16 characters minus the mark. Any
character the server takes in a name will do; Paper takes only letters, digits and `_` while
`perform-username-validation` is on in `config/paper-global.yml`: with `+` turn that off, or use `c_`. The start warns
when the two do not fit.

## UUID schemes

`ownership.uuid-scheme` decides the UUID a player plays under, which every other plugin keys inventories, ranks and money
by:

| Scheme | Behaviour |
|---|---|
| `keep-first` (default) | A new paid player gets the Mojang UUID, so client cosmetics show; a new cracked player the offline one. An account keeps the UUID it was made with |
| `offline` | Everybody gets the offline UUID derived from the name |
| `mojang` | Every paid player gets the Mojang UUID; a cracked account that later proves a paid name moves to it |
| `random` | Every new player gets a random UUID, kept from then on |

The scheme is written into the database at the first start. A later start under another scheme is refused, because it
would put every player into an empty inventory. To change it, run `/uxmauth uuidscheme <scheme>` from the console while
nobody plays, write the scheme in `uuid-scheme` and restart. Accounts that would share an id are named and left.

## Name conflicts

When the paid owner of a name proves it while a cracked account holds it, through `/claim`, a rename at Mojang, after a
start while Mojang was down, or after a switch to `strict`, `ownership.conflict` decides:

| Strategy | Behaviour |
|---|---|
| `rename` (default) | The cracked account moves with everything it had to the name plus `rename-mark` (`_`); the paid owner gets the name. Nobody loses anything |
| `block` | Both are stopped with a message until staff settle it with `/uxmauth resolve` |
| `cracked-keeps` | The cracked account keeps the name and the paid owner is told to change theirs at Mojang |
| `premium-takes` | The cracked account is deleted and the paid owner gets the name. Whoever buys a name takes the account of the player who held it first |

When the marked name is taken too a number follows it, and a result past 16 characters shortens the name before the
mark. The moved player types the new name to log in, so `names.pattern` must take it; the plugin refuses a mark it does
not take. A dispute staff settled for the cracked holder stays: the paid owner who proves it again is told to change
theirs.

## Switching by hand

`/premium` and `/cracked` switch a player's own account; `/uxmauth markpremium` and `markcracked` do it for staff.

Related: [ownership.conf](../config/ownership-conf.md), [Bedrock](bedrock.md)

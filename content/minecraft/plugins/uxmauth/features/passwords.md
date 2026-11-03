---
title: Passwords
order: 302
description: Name and password rules, the common list, the breach check, the hashing algorithms and the lane.
icon: key-round
---

## Names

| Key under `names` | Default | Meaning |
|---|---|---|
| `pattern` | `[A-Za-z0-9_]+` | A Java regular expression the whole name must match |
| `min-length`, `max-length` | `3`, `16` | |
| `blocked` | `[]` | Words a name may not contain, in any case: `admin` refuses `xAdMinx` |

A Bedrock name Floodgate vouches for keeps its prefix and spaces, so only the blocked words apply to it.

## Passwords

| Key under `passwords` | Default | Meaning |
|---|---|---|
| `min-length`, `max-length` | `6`, `64` | In the characters a player types |
| `allowed` | `\\S+` | A regular expression the whole password must match: any visible character, no spaces |
| `required` | `[]` | Classes every password must hold: `lower`, `upper`, `digit`, `symbol` |
| `confirm-on-register` | `true` | `/register` asks twice; off, one typo makes an account its owner cannot log into |
| `old-on-change` | `true` | A change asks for the old password; leave it on |
| `max-attempts` | `3` | Wrong passwords before a kick |
| `common-file` | `passwords/common.txt` | Passwords too common to keep, one a line; case does not count; read at every reload |
| `breach-check` | `false` | Asks Have I Been Pwned whether a new password is known from a breach |

The breach check sends only the first five characters of the password's SHA-1, with padding asked for, so the service
never learns the password; when it does not answer, the password is let through. It ships off because it reaches out to
a service you did not choose.

A player whose right password no longer meets the rules is held on the replace screen until they set a new one.

## Hashing

`hashing.algorithm` is the algorithm new passwords are written in. Every one is read whatever the setting says, and a
player who logs in on an older one is moved to the chosen one on the spot. The block applies at the next start.

| Algorithm | Settings |
|---|---|
| `argon2id` (default) | `memory-kib` 65536, `iterations` 3, `parallelism` 1 |
| `bcrypt` | `cost` 12; under 10 is weak. Reads only the first 72 bytes, so `passwords.max-length` may not pass 72 |
| `pbkdf2` | `iterations` 600000, HMAC-SHA256 |
| `authme-sha256` | Weak: a leaked table is cracked quickly on a graphics card. Only for a website that reads nothing else |

## The lane

| Key under `lane` | Default | Meaning |
|---|---|---|
| `threads`, `max-threads` | `2`, `4` | Hashes at once; `max-threads` times `memory-kib` is the most taken for hashing. Next start |
| `queue` | `256` | How many may wait; past it a login is told to try again |

Related: [config.conf](../config/config-conf.md)

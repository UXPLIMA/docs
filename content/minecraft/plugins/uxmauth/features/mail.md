---
title: Mail and recovery
order: 409
description: The SMTP server, linking an address, recovering a forgotten password, and the mail texts.
icon: mail
---

Nothing is mailed while `mail.enabled` is false.

## The mail server

| Key under `mail` | Default | Meaning |
|---|---|---|
| `host`, `port` | `""`, `587` | |
| `security` | `starttls` | `plain`, `ssl` (TLS from the first byte, usually 465) or `starttls` (usually 587; never falls back to the clear) |
| `username`, `password` | `""` | Empty username for a server that takes mail without one |
| `from-address`, `from-name` | `""`, `Minecraft` | |
| `timeout-seconds` | `10` | |
| `threads` | `2` | Mails at once; read at start |

A mail that cannot be sent is told to the player and once to the console, never with the address.

## The linked address

`/email add <address>` mails a code; `/email confirm <code>` links it; `/email remove` takes it away.

| Key under `email` | Default | Meaning |
|---|---|---|
| `allowed-domains` | `[]` | Empty takes every domain; a domain names its subdomains |
| `denied-domains` | `[]` | Refused even where the allowed list names it |
| `code-minutes`, `code-attempts`, `cooldown-seconds`, `code-length` | `15`, `5`, `60`, `6` | A code's life, tries, the wait before another, and its digits (4 to 10) |
| `required-at-registration` | `false` | A password account plays only with a confirmed address: held until it links one, and it can be replaced but not removed. Holds nobody while mail is off |

## A forgotten password

"Forgot password" on the login screen, or `/recover`, mails a code to the linked address; the recover screen, or
`/recover <code> <password>`, sets a new one.

## The mail texts

`mail/<kind>_<language>.txt`, one per kind and language: `login-code`, `recovery` and `verify`. Write
`login-code_it.txt` and Italian players read Italian.

Related: [Second factor](second-factor.md)

---
title: Second factor
order: 308
description: An authenticator app or a code by mail, backup codes, requiring it for staff, and the sealed secrets.
icon: smartphone-nfc
---

After the password, a code from an authenticator app such as Google Authenticator or Authy, or a code by mail.

## Turning it on

| Step | Command |
|---|---|
| Set up | `/2fa setup`: the key, the link and a QR code on a map, for `setup-minutes` (10) |
| Confirm | `/2fa confirm <code>` with the app's first code; the backup codes are shown |
| By mail instead | `/2fa email on`, with a confirmed linked address |
| New backup codes | `/2fa backup <code>`; the old set is void |
| Off | `/2fa off <code>` with an app or backup code; staff use `/uxmauth disable2fa` |

## Settings

| Key under `second-factor` | Default | Meaning |
|---|---|---|
| `issuer`, `label` | `Minecraft`, `{player}` | How an app lists the account |
| `digits` | `6` | 6 to 8 |
| `period-seconds` | `30` | 15 to 300 |
| `window-steps` | `1` | Steps either side of now, for a phone whose clock is off; 0 to 4 |
| `setup-minutes` | `10` | |
| `backup-codes`, `backup-code-length` | `8`, `10` | 1 to 20 codes of 8 to 16 characters; each works once |
| `required-permission` | `uxmauth.2fa.required` | A player holding it plays only with the app on |
| `premium-skips` | `true` | A proven premium or Bedrock player skips the code |

A wrong code costs a try as a wrong password does, out of `passwords.max-attempts`, and counts toward the address bans.
A code is taken once across a network.

## Required for staff

A player holding `required-permission` outright is held after the login until they set the app up, at every join, for
as long as they hold the node. Give it to staff through your permission plugin; only a node granted outright counts,
never the default the server gives operators. Empty asks it of nobody.

## Sealed secrets

App secrets are sealed with AES-GCM: on a lone server under `data/second-factor.key`, on a network under the network
secret. Back up the key file with the database.

Related: [Mail and recovery](mail.md), [Sessions](sessions.md)

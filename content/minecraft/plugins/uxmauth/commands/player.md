---
title: Player commands
order: 302
description: Every command of the door, with its arguments, node and what it does.
icon: user
---

Every node is given to everyone by default. A password typed after a command is kept out of the console and the log.
Most commands answer the dialog too: a player in the dialog never types them.

| Command | Node | What it does |
|---|---|---|
| `/login [password]` | `uxmauth.login` | Logs in |
| `/register [password] [confirm]` | `uxmauth.register` | Makes an account; the second password while `passwords.confirm-on-register` is on |
| `/changepassword [old] [new]` | `uxmauth.changepassword` | Changes the password; the old one while `passwords.old-on-change` is on. Ends every session |
| `/logout` | `uxmauth.logout` | Logs out and waits at the door again |
| `/claim` | `uxmauth.claim` | Says the name is yours; the next connection from the same address within `ownership.claim-minutes` is asked to prove it through Mojang |
| `/premium` | `uxmauth.premium` | Proves the name on the next connection and logs in without a password from then on |
| `/cracked [password]` | `uxmauth.cracked` | Turns a premium account cracked, logging in with a password from now on |
| `/unregister [proof]` | `uxmauth.unregister` | Deletes your own account: alone it asks, then again with the password or an app code within `account.delete-confirm-seconds` (60) |
| `/session on\|off` | `uxmauth.session` | Whether a join from the same address skips the password; `off` ends every session |
| `/captcha [code]` | `uxmauth.captcha` | Answers the captcha |
| `/recover [code] [password]` | `uxmauth.recover` | Alone, mails a code to the linked address; with the code and a new password, sets it |
| `/email add [address]` | `uxmauth.email` | Mails a code to the address |
| `/email confirm [code]` | `uxmauth.email` | Links the address with its code |
| `/email remove` | `uxmauth.email` | Takes the linked address away |
| `/2fa [code]` | `uxmauth.2fa` | Answers the second-factor code |
| `/2fa setup` | `uxmauth.2fa` | Shows the key, the link and a QR code on a map |
| `/2fa confirm [code]` | `uxmauth.2fa` | Turns it on with the app's first code and shows the backup codes |
| `/2fa backup [code]` | `uxmauth.2fa` | A new set of backup codes; the old set is void |
| `/2fa email on` | `uxmauth.2fa` | Mails the login code instead of asking the app |
| `/2fa email off [code]` | `uxmauth.2fa` | Stops it |
| `/2fa off [code]` | `uxmauth.2fa` | Turns it off with an app code or a backup code |
| `/uxmauth lang` | `uxmauth.language` | The language you read and what else exists |
| `/uxmauth lang set <language>` | `uxmauth.language` | Reads the plugin in one language |
| `/uxmauth lang reset` | `uxmauth.language` | Follows the client again |

With `screens.chat-input` on, a player in the world types the password, a new one and its repeat, or the app's code as
a line of chat instead of a command; the line reaches nobody.

Related: [The waiting room](../features/waiting-room.md), [Second factor](../features/second-factor.md)

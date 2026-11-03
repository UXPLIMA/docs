---
title: API
order: 903
description: Reaching uxmAuth from another plugin, and the events on Paper and Velocity.
icon: code
---

Another plugin compiles against the `uxmAuth-api` module and reaches the API through `AuthApiProvider.get()` on Paper and
Velocity, or through the services manager on Paper. It answers `isAuthenticated`, `method` and `kind` from memory. Both
jars carry the module unrelocated.

## Paper events

| Event | Fires | Thread |
|---|---|---|
| `PreLoginEvent` | Before a password is checked; `refuse(reason)` refuses, and a refused login spends no attempt | Off the server's threads |
| `PreRegisterEvent` | Before an account is made; `refuse(reason)` | Off the server's threads |
| `LoginEvent` | A login | The player's thread |
| `RegisterEvent` | A registration | The player's thread |
| `FailedLoginEvent` | A wrong password | Off the server's threads |
| `BanEvent` | An address ban | Off the server's threads |
| `PasswordChangeEvent` | A password change | Off the server's threads |
| `UnregisterEvent` | An account removed | Off the server's threads |
| `KindSwitchEvent` | Premium to cracked or back | Off the server's threads |

## Velocity events

`PreLoginEvent`, `PreRegisterEvent`, `LoginEvent`, `RegisterEvent`, `FailedLoginEvent` and `BanEvent` fire for what
happens at the proxy.

Related: [Integrations](overview.md)

---
title: Income cap and tax
order: 406
description: The soft income cap per window, the tax by node and its bypass, and the server account.
icon: gauge
---

## Income cap

```hocon
income-cap {
  window-minutes = 60
  money      { cap = 25000, softness = 1.0 }
  experience { cap = 20000, softness = 2.0 }
  points     { cap = 2000,  softness = 1.0 }
}
```

The backstop that catches the exploit nobody thought of: it bounds the outcome instead of the method. Per window and
per currency, pay is full up to `cap` and shrinks smoothly above it; a larger `softness` keeps more. A hard wall would
teach a player to stop playing for the rest of the hour. A cap of `0` is no cap.

`%uxmjobs_earned_<currency>%`, `%uxmjobs_cap_left_<currency>%` and `%uxmjobs_cap_reset%` show where a player stands.

## Tax

```hocon
tax {
  enabled = false
  rate    = 0.05
  node    = "uxmjobs.tax."
  bypass  = "uxmjobs.tax.bypass"
}
```

Off by default. On, every positive wage loses `rate`, a fraction from 0 to 1. A node under `node` carries a number that
replaces the rate when it is lower; the lowest held wins. A player holding `bypass` pays none.

The number after the prefix may not contain a full stop, so a fraction such as `uxmjobs.tax.0.02` is not read, and a
whole number is never below a rate under one. In this build only `uxmjobs.tax.0` lowers the tax, to nothing.

## Server account

```hocon
server-account {
  enabled          = false
  holder           = ""
  pay-what-is-left = true
}
```

A jobs plugin is the largest faucet on a survival server. On, every money wage is drawn from the account `holder` (a
player name or a UUID your economy knows), so the plugin does not mint money from nothing. A wage the account cannot
cover is paid in part (`pay-what-is-left = true`) or refused whole. The player is told the account is empty
(`pay.server-account-empty`) once per `display.paid.empty-account-notice-seconds` (60).

Related: [Payments](payments.md), [config.conf](../config/config-conf.md#income-cap)

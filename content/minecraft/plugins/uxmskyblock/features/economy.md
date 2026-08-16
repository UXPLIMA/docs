---
title: Bank Interest & Log
order: 700
description: The island bank can pay periodic interest and keeps a full transaction
  log.
icon: coins
---

## Interest

When enabled, every island earns interest on its bank balance at a fixed interval.

<Callout type="success" title="Cross-server safe">

Interest is applied **only by the backend server that physically hosts the island**, so on a proxy network the balance is never credited twice.

</Callout>

### Configuration

```yaml
bank:
  interest:
    # Turn periodic interest on/off.
    enabled: false
    # Fraction of the (capped) balance added each interval. 0.02 = 2%.
    rate: 0.02
    # How often interest accrues, in minutes.
    interval-minutes: 60
    # Interest is only calculated on balance up to this cap.
    max-balance: 1000000
    # Balances below this earn no interest.
    min-balance: 0
    # How often the task wakes up to check which islands are due, in seconds.
    check-interval-seconds: 60
```

- Interest is floored to two decimals and added straight to the bank.
- Each interest payment is written to the transaction log as an `INTEREST` entry.

## Transaction log

Every deposit, withdrawal and interest payment is recorded in the shared database, so the history is the same on every server.

| Command | Description |
| --- | --- |
| `/is bank log` | Show the most recent bank transactions |

Aliases for the sub-command: `gecmis`, `history`.

### Configuration

```yaml
bank:
  log:
    # How many recent transactions /island bank log shows.
    lines: 10
```

Each line shows the time, type (`DEPOSIT` / `WITHDRAW` / `INTEREST`), who did it, the amount and the resulting balance.

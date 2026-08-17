---
title: advanced.yml
order: 54
description: Threads, caching, rate limits, retries and cleanup.
icon: gauge
---

Threading, caching, retries and cleanup. The defaults are correct for almost every server; this page
exists so you know what a setting does before you change it.

## Async

```yaml
async:
  async-database: true
  async-discord: true
  thread-pool-size: 4
```

Both should stay `true`. Every Discord call crosses the internet and every database call may: doing
either on the main thread stalls the server for as long as it takes.

`thread-pool-size: 4` is enough for this plugin's workload. Raising it does not make Discord answer
faster; it only means more threads waiting.

## Cache

```yaml
cache:
  enabled: true
  cache-linked-accounts: true
  cache-expiration: 600
  max-cache-size: 1000
```

Linked accounts are read constantly (on join, on chat, on every sync) and change rarely, so they
cache well.

`max-cache-size: 1000` holds a thousand accounts. On a server with more linked players than that,
raise it: the cost is a few hundred bytes each, and the alternative is a database read on every
lookup that misses.

`cache-expiration: 600` bounds how long a stale entry can survive. Lower it if you edit links
directly in the database, though `/uxmdiscordsync forcelink` and `forceunlink` update the cache
properly, and are the right way to do it.

## Performance

```yaml
performance:
  batch-operations: true
  batch-size: 100
  collect-metrics: false
```

Batching groups database writes. Leave it on.

## Security

```yaml
security:
  code-generation-rate-limit: 3
```

Link codes a player may generate per minute. Three is enough for a player who mistypes; low enough
that codes cannot be generated in bulk to brute-force the six-digit space.

## Error handling

```yaml
error-handling:
  retry-failed-discord-calls: true
  max-retries: 3
  retry-delay: 1000
  log-stack-traces: false
```

Discord's API returns transient failures, and retrying is the correct response to them. Three
attempts a second apart covers a rate limit or a brief outage without hammering.

`log-stack-traces: true` when reporting a problem to UXPLIMA support: the error message alone is
rarely enough to identify where it came from.

## Cleanup

```yaml
cleanup:
  auto-cleanup-codes: true
  cleanup-interval: 300
  delete-old-logs-after-days: 30
```

Expired link codes are swept every `cleanup-interval` seconds. Audit records older than
`delete-old-logs-after-days` are deleted; `0` keeps them forever.

Thirty days is a reasonable balance. If you need audit history for longer than that (for appeals,
or for anything you might have to answer for months later) set it to `0` and let the database grow;
the rows are small, and the Discord webhook posts are a separate copy that this setting does not
touch.

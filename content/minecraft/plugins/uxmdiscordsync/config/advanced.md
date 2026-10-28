---
title: advanced.yml
order: 54
description: The link cache, the limit on link codes, stack traces and the cleanup of expired codes.
icon: gauge
---

The defaults suit almost every server. This page is here so you know what a setting does before you
change it. Changes apply on `/dsadmin reload`.

```yaml
cache:
  enabled: true
  cache-expiration: 600
  max-cache-size: 1000

security:
  code-generation-rate-limit: 3

error-handling:
  log-stack-traces: false

cleanup:
  auto-cleanup-codes: true
  cleanup-interval: 300
```

## cache

Links are read all the time (on every join, every sync, every placeholder) and change rarely, so the
plugin keeps them in memory.

| Setting | Meaning |
|---|---|
| `enabled` | Keep links in memory. Off, every lookup goes to the database |
| `cache-expiration` | Seconds before a link is read from the database again |
| `max-cache-size` | Links kept at most. Raise it if more players than this are linked and online |

Links made and removed through the plugin update the cache at once, on every server of a network. Only
a link changed by hand in the database waits up to `cache-expiration` to show; `/dsadmin reload`
clears the cache.

## security

`code-generation-rate-limit` is how many link codes one player can ask for in a minute, counted on
every server together. Three is enough for a player who mistyped; `0` removes the limit.

## error-handling

`log-stack-traces: true` prints the full error behind a failure in the console. Turn it on when you
report a problem to UXPLIMA, together with `plugin.debug: true`, and off again afterwards.

## cleanup

`auto-cleanup-codes` deletes expired link codes every `cleanup-interval` seconds, at least 10. Expired
codes do not work either way; this only keeps the table small.

Links, link history, boost claims and rank syncs are never deleted by the plugin.

Related: [config.yml](config-yml.md), [Database](database.md)

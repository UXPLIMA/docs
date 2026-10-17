---
title: Logs
order: 57
description: Farmer logs, the plugin's own log files, and operations the database did not confirm.
icon: scroll-text
---

uxmFarmer keeps two kinds of log. Farmer logs are what players read in a farmer's logs menu. The
plugin log files are for you: what the plugin did, and what it could not finish.

## Farmer logs

A farmer log records a collection, a sale, an automatic sale, a new member, a promotion or a
demotion. Members with the `SEE_LOGS` rank permission read them in the farmer's logs menu.

```yaml
# config.yml
farmer-logs:
  max-per-farmer: 100
  max-age-days: 30
```

```yaml
# general-settings.yml
disable-logs: false
write-farmer-logs-to-file: false
```

| Key | What it does |
|---|---|
| `max-per-farmer` | The most logs a farmer keeps; the oldest go first. `0` turns this limit off |
| `max-age-days` | Logs older than this are deleted. `0` turns this limit off |
| `disable-logs` | Stops recording farmer logs at all |
| `write-farmer-logs-to-file` | Also writes each farmer log to the plugin log file |

Farmer logs live in the database. The plugin trims them every five minutes and on
`/uxmfarmer save`, so there is nothing to clear by hand. `/uxmfarmer clearlogs` deletes every log
of the selected farmer at once.

## Plugin log files

```yaml
# general-settings.yml
log-file-retention-days: 14
```

The plugin writes to `plugins/uxmFarmer/logs/`, one file per day, named like
`2026-10-15_uxmfarmer.log`. Files older than `log-file-retention-days` are deleted; `0` keeps them
forever.

Each line carries a timestamp and a level. Besides the plugin's own notices, the file holds the
operation journal: every sale, withdrawal, upgrade, conversion and return writes
`Committed <operation> for <player>, paying` once the database accepts it, and
`Paid <operation> for <player>` once the player has received what was owed.

## Operations the database did not confirm

Money and items move in two steps: the database records the change, then the plugin pays the
player. If the database connection drops while the change is being written, the plugin asks the
database whether it went through.

| Answer | What happens |
|---|---|
| It went through | The player is paid as usual |
| It did not | Whatever was taken is given back, and the player is asked to try again |
| Unknown | What was taken (the money for an upgrade, a level item) is kept, the player sees `operation-result-unknown`, and the server console logs the operation and the farmer UUID with `may have been committed; nothing it took is given back` |

The third case is the one to act on. Select the farmer with `/uxmfarmer select <uuid>` and check
whether the change is there, for example the product level an upgrade should have raised. If it is
missing, the player paid for nothing and is owed a refund by hand.

A `Committed` line in the log file with no `Paid` line after it is the other case worth a look:
the change reached the database, and the server crashed before the player was paid. A normal
stop pays everything already committed first.

Related: [config.yml](config-yml.md), [general-settings.yml](general-settings.md),
[Database and multi-server](database.md)

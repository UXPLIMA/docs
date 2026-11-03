---
title: Your first setup
order: 204
description: A lone server in five minutes, then the five choices that matter.
icon: flag
---

## A lone offline server

1. Set `online-mode=false` in `server.properties`.
2. Put the Paper jar in `plugins/` and start. Join: a dialog asks for a password twice. Rejoin: a dialog asks for it
   once.
3. `/uxmauth doctor`.

## Five choices

| Choice | Where | Read |
|---|---|---|
| Where a waiting player stands | `screens.conf`, `waiting-room` | [The waiting room](../features/waiting-room.md) |
| Who owns a paid name | `ownership.conf`, `rule` | [Premium names](../features/premium.md) |
| The UUID players play under, once and for good | `ownership.conf`, `uuid-scheme` | [UUID schemes](../features/premium.md#uuid-schemes) |
| Whether a join from the same address skips the password | `config.conf`, `sessions.mode` | [Sessions](../features/sessions.md) |
| Whether staff need an app code | `uxmauth.2fa.required` | [Second factor](../features/second-factor.md) |

<Callout type="warning" title="Choose the UUID scheme before players join">

The scheme is written into the database at the first start, and a later start under another one is refused. Changing
it later is a console command while nobody plays.

</Callout>

## Coming from another login plugin

Name it in `import.source` before the first start; see [Moving accounts](../integrations/migration.md).

Related: [Concepts](concepts.md), [config.conf](../config/config-conf.md)

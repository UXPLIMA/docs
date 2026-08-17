---
title: Logs, analytics and webhooks
order: 9
description: The action log, what it holds, the analytics page and Discord relaying.
icon: scroll-text
---

## The action log

Every command whose metadata sets `Log = true` writes a structured entry.

| Field | Holds |
|---|---|
| `id` | A GUID |
| `userId` and `adminName` | Who ran it |
| `command` | The command name |
| `target` | Who it hit |
| `description` | The arguments that were not left at their default |
| `time`, `date`, `dateString` | When |

One entry is written per target, so a command that hits ten players writes ten entries.
That is what makes the log searchable by victim as well as by admin.

<Callout type="danger" title="Logs live in memory and are lost on restart">

The log is a table in the running server, capped at 500 entries. Passing the cap drops the
oldest ones.

Nothing is written to a DataStore, so a server restart takes the whole history with it, and
each server has its own log with no shared view.

If you need a record that survives, use the webhook: it is the only route out of the
server.

</Callout>

The Logs page reads this table live, with the usual search and filtering. It is gated on
`Permissions.LogsViewRank`, which ships as Mod.

## Analytics

The Analytics page is computed in the running server and covers:

| | |
|---|---|
| Uptime | How long this server has been running |
| Players | Current, and how many have joined in total |
| Commands | Total run, and the count in the last hour |
| Top commands | The three run most |
| Top admins | The three who ran the most |
| Instance counts | Parts and scripts, sampled |

Like the log, it is per server and starts from zero on every restart.

The part count stops after scanning five thousand instances and says so, which keeps the
page from freezing a heavily built game.

## Webhooks

```lua
CommandWebhook = {
    ["*"] = "YOUR WEBHOOK LINK HERE",
    -- ["ban"]  = "a webhook just for bans",
    -- ["kick"] = "another one",
}
```

`["*"]` is the fallback for every command. A per-command key beats it, which is how you send
bans to a private channel and everything else to a general one.

The placeholder string is recognised and skipped, so leaving the file untouched simply
means nothing is posted.

### What gets relayed

Only commands whose metadata sets `Webhook = true`. That is the consequential set: the bans,
kicks, crashes and nukes, the lock and lockdown commands, the shutdowns, migrations and
warnings.

The rest are logged in-server but not relayed, which keeps the channel readable.

### What a post contains

| | |
|---|---|
| The admin's name and user id | |
| The command | |
| The target or value | |
| The admin's avatar | Fetched from a thumbnail proxy |
| A timestamp | |

<Callout type="warning" title="The avatar is fetched through a third-party proxy">

Roblox blocks requests to its own API from inside a game, so the avatar image is fetched
from a community proxy. If that service is down or blocked, the post still goes out without
the picture.

If you would rather not depend on somebody else's service at all, the fetch is one function
in `WebhookService` and removing it costs you only the thumbnail.

</Callout>

<Callout type="danger" title="A webhook URL is a write credential">

Anybody holding it can post anything to that Discord channel, dressed as your game. It lives
in `Server/Config/Webhooks.luau`, which never reaches a client.

Do not move it into `Shared/Config`, do not paste it into a public repository, and rotate it
in Discord if it leaks.

</Callout>

### Failures are silent

The post is wrapped so a failure cannot break the command that triggered it. A dead webhook
means missing posts and nothing else, so check the channel occasionally rather than trusting
that quiet means clean.

## What to build on top

The log is a plain table and the webhook is one function. Two things worth adding for a
serious game:

| | |
|---|---|
| Persistence | Write entries to your own DataStore or an external endpoint |
| Alerting | A second webhook for the commands that should never happen unannounced |

The second one matters more than it looks. A `gshutdown` at three in the morning is
something you want to hear about, and the system will happily do it without telling anybody.

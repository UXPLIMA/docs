---
title: The arrest log
order: 8
description: Posting every arrest to a webhook, the proxy requirement, and why the URL is server side.
icon: webhook
---

Every arrest can be posted to a chat channel as a formatted message: who was arrested, by
whom, for how long, on what charges.

Off until you paste a URL.

## Where the config lives

`Server/Config/Webhook.luau`. Not `Shared/Config`.

<Callout type="danger" title="Never move the URL into Shared">

Everything under `Shared/` is copied into `ReplicatedStorage`, which every client can read.
A webhook URL placed there can be read by any player and used to post to, or delete from,
your channel.

Living under `Server/` means it never leaves the server. Edit it there.

</Callout>

## Setting it up

| Step | |
|---|---|
| 1 | Create a webhook in your chat platform and copy its URL |
| 2 | Route it through a webhook proxy service |
| 3 | Paste the **proxied** URL into `Url` |
| 4 | Set `SendWebhook = true` on the team pairs you want logged |
| 5 | Enable **Allow HTTP Requests** in Game Settings, under Security |

<Callout type="warning" title="Roblox blocks most chat platforms directly">

`HttpService` refuses requests to the common chat domains. Pasting a raw webhook URL
produces nothing, silently, because the request is wrapped and its failure discarded.

You need a proxy service that accepts the request and forwards it. Paste the proxy's URL.

</Callout>

## Per team pair

```lua
SendWebhook = false,
```

On each entry in `Config/Teams.luau` and on the free-for-all entry. Admin `/jail` uses its
own flag in `Config/Admin.luau`, which ships as `true`.

That granularity is useful: log serious arrests and leave routine ones out, or log one
department's arrests while another's are private.

## The message

```lua
Color = 16711680,
```

A coloured bar down the side of the message, as a decimal number. `16711680` is red. Any hex
to decimal converter gives you others.

The fields:

| Field | Contents |
|---|---|
| Arrested Player | The suspect's username |
| Arresting Officer | The officer's username |
| Officer Team | Their team at the time |
| Jail Time | The sentence, formatted as `1m 30s` |
| Global Arrest | `Yes` or `No` |
| Infractions | The charges, or `None` |
| Reason | The typed reason |

The first five sit inline, three to a row; the last two get their own rows.

A timestamp is attached automatically.

## Translating it

Every label is in the `Language` table:

```lua
Language = {
    Title = "Player Arrested",
    ArrestedPlayer = "Arrested Player",
    ArrestingOfficer = "Arresting Officer",
    OfficerTeam = "Officer Team",
    JailTime = "Jail Time",
    GlobalArrest = "Global Arrest",
    Infractions = "Infractions",
    Reason = "Reason",
    Yes = "Yes",
    No = "No",
    Unknown = "Unknown",
},
```

Reword or translate the right-hand side. `Unknown` fills any field that is somehow missing.

The time format itself comes from `Localization.TimeFormat`, so the log and the jail HUD
read the same way.

## Failure is silent

The post runs on its own thread and its result is discarded. A bad URL, a blocked domain, a
proxy outage or a rate limit all produce nothing in Output.

That is a deliberate choice: an arrest should never fail because a log message did not
send. It does mean that when the log is empty, the way to find out why is to work through
the checklist above rather than to look for an error.

Start by confirming HTTP requests are enabled and the URL is the proxied one.

## Turning it off

Blank the `Url`. Nothing is sent, whatever the per-pair flags say.

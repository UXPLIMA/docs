---
title: Cross-server
order: 8
description: The shared secret, the topics, the server list, and what reaches every server.
icon: network
---

Some actions have to reach servers you are not in: an announcement, a vote, a lockdown, a
shutdown. They travel over `MessagingService`.

## The envelope

Every message is wrapped:

```lua
{
    secret = Secret.value,
    origin = game.JobId,
    data   = { … },
}
```

A message whose secret does not match is dropped and a warning is logged. That is the entire
authentication model, and it is why `Server/SharedSecret.luau` must not keep its
placeholder value.

<Callout type="danger" title="The secret is the only thing protecting these topics">

Anything that can publish to your topics with a matching secret can shut your game down.
The file ships with `REPLACE_WITH_YOUR_OWN_GUID_BEFORE_SHIPPING`, which is a value every
copy of this system shares.

Generate a GUID, paste it in, and keep the file out of anything public. It lives in
`Server/` and is never replicated to clients.

</Callout>

## The topics

| Topic | Carries |
|---|---|
| `uxr.announcement.v2` | A global server message |
| `uxr.GlobalPost.v2` | A global post from the panel |
| `uxr.alert.v2` | A high-priority alert banner |
| `uxr.AdminChat.v2` | The cross-server admin chat |
| `uxr.voteStart.v2` | A global vote opening |
| `uxr.voteCast.v2` | A vote being cast |
| `uxr.lockdown.v2` | A global lockdown or its lifting |
| `uxr.shutdown.v2` | A global shutdown |
| `uxr.migrate.v2` | A global migration |
| `uxr.place.v2` and `uxr.forcePlace.v2` | Cross-server teleports |
| `uxr.serverList.v2` | The heartbeat that builds the server list |

All of them are versioned in the name, so a future protocol change can run alongside the
current one.

## The server list

Every public server publishes a heartbeat every fifteen seconds with its job id, its player
count and its region. The Servers page shows the result.

Entries that have not been heard from for 45 seconds are removed, so a server that dies
disappears within a minute.

<Callout type="info" title="Private servers stay out of the list">

The heartbeat loop only runs while `game.PrivateServerId` is empty. A paid private server
never advertises itself, which is the right default for a feature meant to show your public
servers.

</Callout>

## What crosses servers

| Command | Reach |
|---|---|
| `globalservermessage` | Every server |
| `globalAlert` | Every server |
| `globalVote` | Every server |
| `globallockdown`, `globalunlockdown` | Every server |
| `gshutdown` | Every server |
| `gmigrate` | Every server |
| `globalPlace`, `globalForcePlace` | Teleports players from any server |
| `follow`, `join` | Move you into another player's server |

Everything else is local to the server you are in.

## Votes

A vote has a question, a list of options, a duration and an id. Local votes are broadcast to
this server only; global votes go out over the topic and every server opens the same vote.

| Stage | |
|---|---|
| Start | Options are registered, every client shows the vote |
| Cast | Each vote updates the running tally for everybody |
| End | The result is broadcast after the duration, plus a quarter |
| Cleanup | The vote is dropped ten seconds later |

<Callout type="warning" title="Global vote tallies are per server">

Each server keeps its own count of the votes it hears about. Casts are published, so the
counts converge, but there is no single authority holding the true total and no ordering
guarantee between servers.

Read the result as a strong indication rather than as an election.

</Callout>

`vote` and `globalVote` are Head Admin. The vote command has a ten second rate limit of its
own, which is the highest in the shipped config.

## Reliability

`MessagingService` is best-effort. A message may be delayed, and under heavy load it may be
dropped. Everything here is designed around that:

| Feature | If a message is lost |
|---|---|
| Announcement | One server misses it |
| Server list | The entry ages out and comes back on the next heartbeat |
| Global lockdown | One server stays open |
| Global shutdown | One server stays up |

For a shutdown that has to be complete, check the Servers page afterwards and finish the
job by hand.

<Callout type="info" title="Roblox limits how much you can publish">

Publishing is rate-limited per universe, and the limit scales with player count. A publish
failure is logged as a warning rather than retried.

In practice the heartbeat is the only frequent publisher, and at one message per server
every fifteen seconds it stays well clear.

</Callout>

---
title: The RPC layer
order: 5
description: One RemoteEvent, one RemoteFunction, and a leading type string.
icon: arrow-left-right
---

Every system talks over exactly two remotes, found under the runtime copy:

```
ReplicatedStorage/<package>/Core/<xx>Events/
  RemoteEvent
  RemoteFunction
```

`<xx>` is the product's short code, for example `drEvents` for the Daily Rewards system.

## One remote, many messages

Rather than a remote per message, both remotes take a **leading type string** and
dispatch on it:

```lua
-- client asks the server a question and waits for the answer
local state = ctx:Invoke("DR_GetState")

-- client tells the server something, no answer
ctx:Fire("SomeAction", payload)
```

```lua
-- server pushes to one client
Net.fire(player, "DR_State", snapshot)

-- server pushes to everyone
Net.fireAll("DR_State", snapshot)
```

Two remotes instead of thirty keeps the replicated surface small and makes every message
visible in one dispatch table rather than scattered across the package.

## Where the handlers are

| Direction | Handler lives in |
|---|---|
| Client to server, with a reply | `Server/ApiHandlers.luau`, the `Functions` table |
| Server to client | `Client/.../Modules/EventDispatch.luau`, the `Handlers` table |

`ApiHandlers.init()` sets `RemoteFunction.OnServerInvoke` once, looks the type string up
in `Functions`, and returns `nil` for anything it does not recognise. An unknown type
string is therefore harmless: it does not error and it does not reach any product code.

Each product's own page lists its type strings and what they carry.

## Calling it from your own code

On the client, the page modules already hold a context object:

```lua
ctx:Fire(rtype, ...)
local reply = ctx:Invoke(rtype, ...)
```

On the server, require the product's own service module rather than going through the
remotes. The remotes exist for the interface; server code should call the service
directly.

## Trust

Everything arriving from a client is untrusted, and the handlers are written that way:
the client sends an intent, the server decides. The client asking to claim a reward does
not claim it, it asks the server whether the reward is claimable; the answer and the
grant both happen server-side.

<Callout type="warning" title="Do not add a handler that acts on client-supplied numbers">

The pattern to avoid when extending a system is a handler that takes an amount, a price
or a target and applies it as given. An exploiter can call any `RemoteFunction` in
`ReplicatedStorage` with any arguments. Read the identity from `player`, read the value
from the server's own state, and use the client's argument only to select between options
the server already knows about.

</Callout>

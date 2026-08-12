---
title: Server Tweaks
order: 1470
description: 'Server Tweaks is a small grab-bag of server-side infrastructure
  adjustments: the handful of low-level behaviours that don''t belong to any one feature
  but that most servers eventually want to change. It bundles a custom F3 brand, a
  console-log filter, an unsigned-chat option, and a SignedVelocity backend companion,
  so you don''t reach for a separate single-purpose plugin for each.'
---

There are **no player commands here**: every tweak is a silent, server-side effect. You
turn each one on in `modules/servertweaks/config.conf` and it simply takes effect.

The module is its own **`servertweaks`** module and ships **enabled**. Every tweak inside
it ships **off except the F3 brand**, which ships **on** so the debug screen shows this
server's brand out of the box. You opt into the rest individually by flipping each
`enabled` key. Turn the whole module off with `enabled = false`, or leave the tweaks you
don't want at their default `false`.

<Callout type="tip" title="Opt-in, one at a time">

Apart from the F3 brand, every tweak defaults to `false`, so installing or updating
uxmEssentials never silently filters your console or alters chat signing. Nothing else
here happens until you ask for it.

</Callout>

---

## Custom F3 Brand

The **server brand** is the small identifier a client shows on its F3 debug screen next to
the frame-time graph, by default *"Paper"*. This tweak re-sends the `minecraft:brand`
string on join so it reads whatever you like:

```hocon
f3-brand {
  enabled = true
  brand = "uxmEssentials"
}
```

This one ships **on**, with `uxmEssentials` as the brand, so change `brand` to your
server's name (or set `enabled = false` to leave the client's default alone). It is
**plain text only**: no MiniMessage, no colour codes. What you type is what appears.
Some clients render legacy section-sign colour codes here, but that is not guaranteed, so
plain text is the safe choice.

<Callout type="note" title="A label, not a security boundary">

The brand is cosmetic. A modified client can display anything it wants regardless of
what the server sends, so treat this as branding, not as a way to hide what software
you run.

</Callout>

---

## Console Spam Filter

Some plugins and some vanilla subsystems are chatty, and a busy console can bury the lines
you actually need to read. The console filter attaches a Log4j2 filter to the root logger
that drops any console line **containing** one of your configured substrings:

```hocon
console-filter {
  enabled = false
  patterns = [
    "Can't keep up!"
    "moved too quickly"
  ]
}
```

Each entry is matched as a plain substring against the formatted log message. A line is
suppressed only when it contains an entry exactly as written; the filter does nothing when
`patterns` is empty or the tweak is off, so leaving it enabled with an empty list costs you
nothing.

<Callout type="warning" title="You are hiding lines, not fixing them">

The filter suppresses whatever matches, including warnings and errors you may need to
see. Keep the substrings **narrow and specific** so you catch only the noise you mean
to. A pattern like `"error"` or `"WARN"` would swallow genuine problems. When something
stops logging that you expected, check this list first.

</Callout>

---

## No Chat Reports

Public chat messages a modern client sends are cryptographically **signed**, which is what
lets a player report them to Mojang. This tweak re-delivers each public chat message as an
**unsigned** copy, so the delivered message carries no signature to report. Formatting is
preserved: the message reads exactly as it otherwise would.

```hocon
no-chat-reports {
  enabled = false
}
```

<Callout type="warning" title="What this does and, importantly, does not do">

This is an **API-only** implementation (it uses no packets and no NMS) and that
bounds what it can honestly claim:

- It **cannot** stop the client from *signing its own outgoing messages*; it only
  strips the signature from the copy the server re-broadcasts to others.
- It **cannot** change `enforce-secure-profile`: that is a `server.properties` boot
  setting, unrelated to this tweak.
- It does **not** touch private messages (`/msg`), command feedback, or anything but
  public chat.
- It gives players **no client-side "green shield" indicator**. The well-known
  No Chat Reports client mod shows that shield; this server-side tweak does not, and
  cannot make it appear.

In short: it removes the reportable signature from broadcast public chat, and nothing
more. If you need the full guarantees of the client mod or a fully modified protocol,
this is not a drop-in replacement.

</Callout>

---

## SignedVelocity Backend

On a proxied network, signed-chat handling has to stay consistent across the proxy hop, or
players can hit dropped or mishandled messages when they change servers. **SignedVelocity**
is a well-known Velocity plugin that coordinates this; this tweak is the **backend
companion** that talks to it over the plugin-messaging channel `signedvelocity:main`:

```hocon
signed-velocity {
  enabled = false
}
```

<Callout type="warning" title="This needs the SignedVelocity plugin on your proxy">

This tweak is **one half of a pair**. It does nothing on its own; it only
interoperates with the real **SignedVelocity** plugin running on your Velocity proxy.
With no SignedVelocity plugin on the proxy, enabling this here is completely **inert**:
no channel traffic, no effect.

uxmEssentials ships **no Velocity code and adds no Velocity dependency** for this. You
install SignedVelocity on the proxy yourself; this switch simply lets the backend
cooperate with it. If you don't run a proxy, or don't run SignedVelocity, leave this
off.

</Callout>

---

## The `config.conf`

The complete `modules/servertweaks/config.conf`, with the shipped defaults: the module on,
the F3 brand on, every other tweak off.

```hocon
# Server Tweaks — a grab-bag of small, independent server/infrastructure toggles. The module itself is ENABLED so you
# can turn on any single tweak below without touching the others. Every individual tweak ships OFF except the F3 brand,
# which ships ON so the debug screen shows this server's brand out of the box; turn any tweak on or off with its own
# enabled flag. Turn the whole module off with enabled = false.
enabled = true

# Custom F3 brand — the "server brand" line shown on a client's F3 debug screen (the minecraft:brand the server
# reports). When on, the brand below is re-sent to each player as they join, overriding the default. The client shows
# it as PLAIN TEXT: MiniMessage and rich formatting are NOT interpreted, so keep it a short plain string. (Some
# clients render legacy section-sign colour codes here, but that is not guaranteed — plain text is the safe choice.)
f3-brand {
  enabled = true
  brand = "uxmEssentials"
}

# Console spam filter — drop known-benign, noisy lines from the console. When on, a log line is hidden only if it
# CONTAINS one of the substrings below (a plain, case-sensitive match). This is deliberately conservative: with the
# list empty, or with this tweak off, NOTHING is ever suppressed — it can only remove lines you explicitly name, never
# hide an error by default. List an exact fragment of the noisy line, e.g. "Can't keep up!".
console-filter {
  enabled = false
  patterns = [ ]
}

# No chat reports — when on, PUBLIC chat is re-delivered to everyone as an UNSIGNED (system) message and the server's
# default signed delivery is cancelled. A message with no signature carries nothing that can be reported to Mojang, so
# public chat handled this way is not reportable. Existing chat formatting (the communication module) is preserved: the
# unsigned copy is rendered through whatever format was applied. WHAT THIS CANNOT DO: it does not stop a vanilla client
# from SIGNING its own outgoing messages (signing is client-side) — the server only declines to relay the signature;
# it does not toggle the server's secure-profile enforcement (that is the enforce-secure-profile server.properties boot
# setting, not a runtime toggle); it does not touch private messages or command feedback; and it does not give clients
# the client-mod "No Chat Reports" indicator. Only messages that arrived signed are reworked; already-unsigned ones are
# left untouched.
no-chat-reports {
  enabled = false
}

# Signed Velocity (backend) — the backend half of a SignedVelocity setup. When on, this server listens on the
# "signedvelocity:main" plugin channel for the Velocity proxy's chat/command rulings and applies the same
# cancel/modify to its own events, so a message the proxy vetoed or rewrote is handled consistently across the proxy
# hop (this avoids the "expired signature"/desync kicks that a proxy cancelling signed chat can otherwise cause). This
# ONLY does something alongside a proxy-side SignedVelocity component sending those rulings — on a single server, or
# behind a proxy without it, no ruling ever arrives and this tweak is completely inert. It does not require or bundle
# any Velocity code here.
signed-velocity {
  enabled = false
}
```

After editing, apply it with `/uxmess reload servertweaks`. Config is loaded once and
swapped atomically on reload, and a deleted key falls back to the default above.

<Callout type="warning" title="HOCON, not YAML">

uxmEssentials config is HOCON (`.conf`). Keys are kebab-case, comments start with `#`,
and lists use square brackets. See [Configuration](../config/overview.md) for the full
picture.

</Callout>

---

## Permissions

Server Tweaks is **config-only**: it registers no commands, so it has **no action
permission nodes** for players to hold. The only node is the standard per-module
administrative gate:

| Node | Default | What it grants |
|------|---------|----------------|
| `uxmessentials.module.servertweaks` | `op` | Reload / inspect the module (`/uxmess reload servertweaks`) |

Everything else about the module is decided entirely by the `config.conf` above.

---

## Next Steps

- [🧩 Per-Module Config](../config/per-module.md): where `modules/servertweaks/config.conf` lives and how it's loaded
- [🧩 The Module System](../modules/index.md): turn modules on and off
- [🌐 Cross-Server: Velocity & Redis](../cross-server/overview.md): how uxmEssentials fits a proxied network
- [🔑 Permission Reference](../permissions/reference.md): every node

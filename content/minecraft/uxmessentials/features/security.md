---
title: Account Security
order: 1330
description: 'The security module hardens accounts against the theft that authentication
  alone can''t rule out: a shared computer, a session left open, a compromised launcher.
  It bundles four things most servers reach for a separate plugin (or three) to get:
  a self-service two-factor second factor, a join verification freeze, op-command
  protection, and a same-IP alt / ClientID guard. Each is an independent switch in
  one HOCON file, modules/security/config.conf.'
---

<Callout type="warning" title="This is not a login / password system">

There is **no** `/login`, `/register`, or password prompt here, and nothing that
gates a first join by itself. Security adds a *second* factor a player enrols
themselves, plus staff-side detection. The module ships **enabled but inert**:
nothing changes for anyone until a player runs `/2fa` or `/pin`, or until you make a
PIN [mandatory](#making-a-pin-mandatory). Turn the whole module off with
`enabled = false`.

</Callout>

<Callout type="info" title="Offline-mode (cracked) servers: keep your login plugin">

On an offline-mode server the name a client sends is only what they typed, so a login
plugin is what turns it into an account. This module does not replace one and does not
try to. What it does is **wait for it**: with AuthMe (or one of its forks) installed,
the join freeze stands down until that plugin reports the player authenticated, and
only then asks for the second factor. Password first, second factor second, which is
what "two factor" has always meant. See
[Working with a login plugin](#working-with-a-login-plugin).

</Callout>

Everything a player enrols is **DB-backed and never stored in plaintext**. A PIN is a
salted one-way PBKDF2 hash — the plugin can *check* a PIN but never *read* one back. A
TOTP secret has to be recoverable (the server recomputes the expected code every 30
seconds), so it is **AES-256-GCM encrypted** at rest under a server-held key-file, not
hashed. An IP address is never stored at all — only a one-way SHA-256 token of it. There
is no GeoIP and nothing reversible anywhere in the module.

---

## The four capabilities at a glance

| Capability | What it does | Who it's for |
|------------|--------------|--------------|
| **Two-factor enrolment** | `/2fa` (authenticator app) and `/pin` (numeric PIN) — one command per factor, each managing only its own | every player, self-service |
| **Join verification** | An enrolled player joining from an unrecognised device is frozen and must prove their factor on a keypad before they can play | enrolled players |
| **Op-command protection** | Dangerous commands (`op`, `stop`, `ban`, …) are held behind a fresh second-factor proof | operators / staff |
| **IP / alt + ClientID guard** | Records same-IP account links and the client brand each player reports, so staff can spot alts and cheat clients | staff (`/ipalts`, `/clientinfo`) |

---

## Setting up 2FA — a player's walkthrough

Any player can protect their own account; the two self-service nodes ship `true`. A
player may enrol **either** an authenticator app, **or** a PIN, **or both** — either one
counts as a valid second factor.

### Option A — an authenticator app (TOTP)

Works with Google Authenticator, Aegis, Authy, or any other RFC 6238 TOTP app.

1. **Run `/2fa setup`.** The server generates a fresh secret and shows it two ways: the
   raw Base32 secret, and an `otpauth://` URI you can paste into (or turn into a QR code
   for) your app. The secret is shown **only to you**, once, and is never written to a
   log.
2. **Add it to your app.** Scan the URI or type the secret in. Your app now shows a
   6-digit code that rolls over every 30 seconds, under the heading
   `uxmEssentials (YourName)` (the label is the configurable `issuer`).
3. **Confirm with `/2fa confirm <code>`.** Type the current 6-digit code from your app.
   Only a correct code enables the factor — until you confirm, nothing is active, so a
   half-finished setup never locks you out.

That's it. `/2fa` on its own tells you whether you're enrolled.

### Option B — a numeric PIN

1. **Run `/pin set <pin>`.** The PIN is digits only (the verification screen is a numeric
   keypad) and must be **4–8 digits** by default. It's validated, then hashed and stored
   — the plaintext never touches the database or a log.

A length rule cannot catch `1234`: it is a perfectly valid four-digit PIN and also the
first thing anybody guessing would try. So the module also ships a **blocked list** of
the obvious ones (runs, repeats, keypad patterns, recent years), refused outright however
well-formed they are. It is a plain config list, `two-factor.blocked-pins`: add your
server's founding year and the word its name spells on a phone keypad, or set it to `[]`
to refuse none.

That's the whole enrolment. `/pin` on its own tells you whether one is set. From now on,
if [join verification](#join-verification) is on, the next time you sign in from a device
the server doesn't recognise you'll be asked to prove your factor before you can move.

<Callout type="tip" title="`/pin set` will not overwrite a PIN you already have">

Setting a first PIN protects nothing yet, so it needs no proof. Replacing a live one
would hand your second factor to whoever typed the command, so that goes through
`/pin change <old> <new>` and needs the current PIN.

</Callout>

### Locking your session before you step away

`/pin lock` freezes you on the spot and puts the keypad up, exactly as a join would. It is
for the moment you leave the keyboard somewhere you would rather not leave an unlocked
account: type it, walk away, and whoever sits down next gets the keypad instead of your
inventory. Come back, tap your PIN, and carry on where you were.

It needs a factor to lock against, so a player with neither a PIN nor an authenticator is
told so rather than frozen with nothing to prove.

### Changing or removing a factor

Each command manages **its own factor and nothing else**:

| You want to | Run |
|---|---|
| Remove your authenticator | `/2fa disable <code>` (a current code from that app) |
| Change your PIN | `/pin change <old> <new>` |
| Remove your PIN | `/pin remove <pin>` |

Every one of them needs the factor it is touching. Requiring the proof is the point: it
stops someone who walks up to an unlocked session from simply switching the protection
off.

<Callout type="info" title="One factor can never remove the other">

Your PIN will not remove your authenticator, and your authenticator will not remove
your PIN. If they could, holding two factors would be no stronger than holding the
weaker of them. It also means losing your phone does not lock you out of your PIN,
and vice versa: only the lost factor needs an operator's help.

</Callout>

### Lost your authenticator, or forgot your PIN?

Ask an operator. `/security reset <player> [totp|pin|all]` clears a factor without a
proof, which is exactly why it is staff-only and why every use is logged. Naming the
factor matters: `totp` clears only the authenticator, so a PIN you still know keeps
working.

---

## Commands

| Command | What it does | Permission |
|---------|--------------|------------|
**Your own authenticator** — `/2fa`:

| Command | What it does | Permission |
|---------|--------------|------------|
| `/2fa` | Show whether you have an authenticator | `uxmessentials.security.2fa` |
| `/2fa setup` | Generate a secret and show the `otpauth://` URI | `uxmessentials.security.2fa` |
| `/2fa confirm <code>` | Prove a code to enable it | `uxmessentials.security.2fa` |
| `/2fa disable <code>` | Remove it (needs a current code) | `uxmessentials.security.2fa` |

**Your own PIN** — `/pin`:

| Command | What it does | Permission |
|---------|--------------|------------|
| `/pin` | Show whether you have a PIN | `uxmessentials.security.pin` |
| `/pin set <pin>` | Set a first PIN (4–8 digits) | `uxmessentials.security.pin` |
| `/pin change <old> <new>` | Replace it (needs the current PIN) | `uxmessentials.security.pin` |
| `/pin remove <pin>` | Remove it (needs the current PIN) | `uxmessentials.security.pin` |
| `/pin lock` | Lock your own session before you step away | `uxmessentials.security.pin` |

**Other players** — `/security` and the two guard reads, all operator-only:

| Command | What it does | Permission |
|---------|--------------|------------|
| `/security status <player>` | Show which factors a player holds | `uxmessentials.security.admin` |
| `/security force <player>` | Make a player re-verify on their next action or join | `uxmessentials.security.force` |
| `/security reset <player> [totp\|pin\|all]` | Clear a factor a player can no longer prove | `uxmessentials.security.reset` |
| `/ipalts <player>` | List the accounts that share an IP with a player | `uxmessentials.security.alts` |
| `/clientinfo <player>` | Show the client brand a player reported this session | `uxmessentials.security.clientinfo` |

<Callout type="warning" title="`/security reset` is the one unproven removal">

It exists so a player who lost their authenticator or forgot their PIN is not locked
out of their own account forever. Because it skips the proof, it needs
`uxmessentials.security.reset` on top of `uxmessentials.security.admin`, and every use
is written to the log as `event=security_factor_reset` with the operator, the target
and the scope. Omitting the factor clears everything.

</Callout>

In the `/2fa setup` message, both the TOTP secret and the `otpauth://` link are
**click-to-copy**: click either in chat and it goes straight to the clipboard, with no
colour codes in the copied text, so it can be pasted into an authenticator app as-is.

`/2fa` is registered under that literal, with **`/twofactor`** as an alias. `/ipalts` and
`/clientinfo` resolve their target online-first and otherwise from the profile cache, so
an offline account is still checkable. All of the crypto and database work runs off the
tick thread — no command blocks the server.

---

## Join verification

When an enrolled player joins from a device they haven't verified from recently, they are
**frozen**: the only thing that responds is the keypad. (Head movement is left alone, so
they can still look around.) A **non-enrolled** player is never frozen, unless you have
made a PIN [mandatory](#making-a-pin-mandatory).

The freeze presents a chest-window **numeric keypad**: ten numbered player heads laid out
`0 1 2 3 4` over `5 6 7 8 9`, a masked entry display on the top row that shows one asterisk
per digit typed, and clear / submit on the bottom row. The player taps their PIN in and
submits; if they enrolled an authenticator, a "type a code" button hands off to a text
prompt where they enter their 6-digit code instead. On success the freeze lifts, the
window closes, and — because a fresh proof also opens the op-command re-auth window — they
aren't immediately re-asked to verify a protected command.

<Callout type="tip" title="The keypad is a menu spec you can re-skin">

The pad lives at `modules/security/gui/pin-keypad.conf`, like every other menu in the
plugin. The digit buttons are player heads carrying a numbered texture, declared as
`material = "basehead:<textures payload>"`, so swapping the ten values for another
numbered head set re-themes the whole pad and nothing else has to change. Slots, the
frame and the materials are all yours to move; the button actions are what make it
work, so keep those attached to whichever slots you use.

</Callout>

```hocon
join-verification {
  enabled              = true
  trust-devices        = true
  trust-duration-hours = 24
  max-attempts         = 3
  lockout-seconds      = 300
  lockout-bans         = true
  entry-timeout-seconds = 60
  spectator-mode       = adventure
  safety-net           = kick
  on-access-revoked    = reverify
}
```

| Key | Default | What it does |
|-----|---------|--------------|
| `enabled` | `true` | Master switch for the join freeze. Off, and enrolment still works but nobody is frozen on join — useful if you only want the second factor for op-command protection. |
| `trust-devices` | `true` | After a successful verification, remember the device so the **next** join from the same address skips the keypad until the trust lapses. The address is stored only as a one-way hash, never in the clear. |
| `trust-duration-hours` | `24` | How long a remembered device skips the keypad. `0` disables the trust window entirely (same as `trust-devices = false`). |
| `max-attempts` | `3` | How many wrong PINs/codes a player may enter before they're kicked. |
| `lockout-seconds` | `300` | How long a locked-out player is kept out — a rejoin inside this window is refused immediately. |
| `lockout-bans` | `true` | Make that lockout a **real tempban** on the server's own ban list rather than an in-memory cooldown a restart would clear. See [Lockouts are real bans](#lockouts-are-real-bans). |
| `lockout-ban-reason` | `Too many failed verification attempts` | The reason recorded against that ban, shown to the player and in the punishment history. |
| `entry-timeout-seconds` | `60` | How long a frozen player has to prove their factor before they're kicked. Without a limit, someone who walks away mid-prompt sits at the keypad forever, holding a slot on a full server and leaving their account signed in. `0` removes the limit. |
| `spectator-mode` | `adventure` | What to do with a player who has to verify while in **spectator**. A spectator cannot click any window the server opens, so a spectator shown the keypad can see every button and press none of them. This moves them into a mode that can click for the length of the freeze and puts their own mode back the moment they verify. `adventure` \| `survival` \| `none` (leave them stuck). |
| `safety-net` | `kick` | What happens when the verification decision **itself** fails: database unreachable, key-file unreadable, lookup throws. Somebody has to decide, because the freeze goes on before the decision runs. `kick` refuses the join with a "try again" message, so an outage keeps enrolled accounts shut rather than open. `allow` lifts the freeze and lets them in unverified, so an outage keeps the server playable and accepts that for its duration an enrolled account is protected by its password alone. |
| `on-access-revoked` | `reverify` | What happens to an online player at the moment staff clear their factor with `/security reset`. See [When staff clear a factor](#when-staff-clear-a-factor). |
| `holding-area` | *(blank)* | Where to park a verifying player for the length of the freeze, as `world,x,y,z` or `world,x,y,z,yaw,pitch`. See [Holding area](#holding-area-and-transfer). |
| `transfer-to` | *(blank)* | A proxy backend to send the player to once they verify. See [Holding area and transfer](#holding-area-and-transfer). |
| `wait-for-login-plugin` | `true` | Let an installed login plugin authenticate first. See [Working with a login plugin](#working-with-a-login-plugin). |

### What a frozen player can't do

Every one of these ships **on**; each is one line in `join-verification.restrictions`, so
you opt out one at a time rather than listing the whole set to get the safe default.

```hocon
join-verification {
  restrictions {
    move          = true
    commands      = true
    chat          = true
    interact      = true
    drop          = true
    pickup        = true
    block-edit    = true
    damage-taken  = true
    damage-dealt  = true
    mob-targeting = true
    teleport      = true
    inventory     = true
    consume       = true
    hunger        = true
  }
}
```

Four of them are **protection rather than restraint**. Somebody staring at a keypad cannot
fight back, run away or eat: `damage-taken` and `mob-targeting` stop the world killing an
account holder while they prove they own the account, `hunger` stops a slow verification
starving them, and `teleport` stops anyone else moving a player who is in no position to
object. (The module's own two moves, into and out of a holding area, are exempt.)

### Lockouts are real bans

With `lockout-bans = true`, running out of attempts is a **tempban through the moderation
module**, not a private list this module keeps to itself. That means it lands in the same
ban table and punishment history as every other ban, staff see it in `/history`, and
`/unban` lifts it — there is no second ban system to learn and no `/security unban` to
remember.

With the moderation module disabled there is nothing to write to, so the lockout stays the
in-memory cooldown and `lockout-seconds` still applies.

### When staff clear a factor

`/security reset` is the recovery door: the one path that removes a factor without proving
it, for a player who lost their authenticator or forgot their PIN. From the other side, it
is also exactly what an account theft would want. So what it does to a player who is
standing in the world at that moment is your call, not ours.

| `on-access-revoked` | What happens |
|---|---|
| `reverify` *(default)* | They go straight back through verification. With a factor left they prove it again; with none left and a [required PIN](#making-a-pin-mandatory) they're shown the create pad, so a reset ends with a factor rather than without one. |
| `kick` | They're disconnected, so nothing of the revoked session carries on and their next join decides afresh. |
| `nothing` | They carry on playing. Right when a reset is routine help-desk work. |

---

## Making a PIN mandatory

Give a player `uxmessentials.security.pin.required` and they cannot play without a PIN.
The node ships `false`, so this is entirely opt-in; grant it to a rank, to your staff, or
to everyone.

The point is that requiring a factor is only half a feature if the player then has to find
a command and read its syntax to comply — especially since they're frozen and can't use
commands. So a player who holds the node and has no factor is shown a **create pad**: the
same keypad they'll use every day, but asking them to set the PIN rather than prove it.
Tap it, submit, tap it again to confirm, submit. Asking twice is not ceremony; it is the
only thing standing between a mistyped PIN and an account whose owner can't get past the
keypad tomorrow.

Nothing is stored until both entries agree **and** the policy accepts them, so a player who
gets it wrong loses the attempt, not the account. A refusal (too short, on the blocked
list) explains itself and puts them back at the first step. The write goes through the same
code path `/pin set` uses, so a PIN created here obeys exactly the same rules.

The pad is its own menu spec at `modules/security/gui/pin-create.conf`, re-skinnable the
same way as the verification pad.

---

## Holding area and transfer

Two optional moves, both blank by default, for servers whose layout wants them.

**`holding-area`** parks the unverified session somewhere deliberately empty for the length
of the freeze. The freeze already stops a player acting, but they're still standing
wherever they logged out, in view of whoever walks past, in a world whose mobs and players
carry on around them. Name a spot as `world,x,y,z` (or `world,x,y,z,yaw,pitch`) and they go
there while they verify and come back to **exactly where they were** the moment they do —
including on a disconnect mid-verification, so nobody is left logged out inside the holding
room. A destination that can't be reached is a logged no-op: losing the move must never
cost anyone their verification.

**`transfer-to`** hands a **verified** player to another proxy backend, by its name in your
proxy config. This is what a dedicated authentication server is for: the whole of an
unverified session happens on a lobby with nothing on it worth reaching, and only a proved
account ever lands on the server that has something. It needs a proxy (Velocity or
BungeeCord) in front; with none, there's nothing to hand them to and the transfer is a
logged no-op. A **failed** proof transfers nobody.

---

## Working with a login plugin

On an offline-mode server, `wait-for-login-plugin = true` (the default) makes the join
freeze stand down until an installed login plugin reports the player authenticated.

The ordering is the whole point. Until the login plugin says so, the name is only what the
client typed, and asking that stranger for the account holder's PIN is both wrong and
useless — wrong because they're not the account holder, useless because the login plugin
has them frozen for its own prompt anyway and the two would fight over the same screen.

Detection is automatic and needs no configuration. These are recognised:

| Plugin | Event waited on |
|---|---|
| **AuthMe Reloaded** | `fr.xephi.authme.events.LoginEvent` |
| **nLogin** | `com.nickuc.login.api.events.bukkit.LoginEvent` |

The list is deliberately short: a name here is a support claim, so an unmaintained login
plugin does not belong on it. An AuthMe fork that keeps AuthMe's own event package is
covered by the first row.

With none of them installed nothing changes: the ordinary join hook stays in charge, which
is the premium-server behaviour. Set the key to `false` to force that even when a login
plugin is present.

---

## Titles and sounds

Chat is a poor channel at the moment of verification: the player has a window open and
their chat is scrolled away behind it. So the keypad also speaks in **titles**, where a
player with a window open is actually looking, and **sounds**, which is what makes a pad
feel like a pad.

```hocon
feedback {
  titles = true

  sounds {
    prompt  = "block.note_block.pling"
    key     = "ui.button.click"
    success = "entity.player.levelup"
    failure = "entity.villager.no"
  }
}
```

Any vanilla or resource-pack sound key works; leave one blank (`""`) to play nothing. An
unusable key plays nothing and logs a line rather than breaking the verification it
decorates. `titles = false` turns the keypad title and the two outcome titles off and
leaves the chat lines alone.

---

## Op-command protection

Dangerous commands are held behind a fresh second-factor proof. When a player runs a
protected command and hasn't verified within the re-auth window, the command is **blocked
before it runs** and they're shown the same keypad to prove their PIN or authenticator
code; on success the command is retried. A burst of protected commands only prompts once,
because a successful proof stamps the re-auth window.

Only players who have **enrolled a factor** are gated — a re-auth needs something to prove
against — a holder of `uxmessentials.security.bypass` is exempt, and the **console is
never gated**.

```hocon
op-protection {
  enabled               = true
  reauth-window-seconds = 60
  protected-commands = [
    "op", "deop", "stop", "restart", "reload",
    "gamemode", "ban", "ban-ip", "pardon", "kick", "whitelist"
  ]
}
```

| Key | Default | What it does |
|-----|---------|--------------|
| `enabled` | `true` | Master switch. Off, and protected commands run with no re-auth check. |
| `reauth-window-seconds` | `60` | How long a successful verification (a join proof or an earlier re-auth) counts as recent. `0` forces a fresh proof for **every** protected command. |
| `protected-commands` | the 11 shown | The commands to protect, by root name. Matching ignores a leading slash, any arguments, and a namespace prefix — so `op` also catches `/op Steve` and `minecraft:op`. Add your server's own dangerous commands here. |

---

## IP / alt guard

On join, the player's address is recorded as a one-way SHA-256 token — **the raw IP is
never stored**, there is no GeoIP, and nothing is reversible — so staff can spot accounts
that share an address. `/ipalts <player>` lists the accounts linked to a target by a shared
address.

```hocon
ip-guard {
  enabled             = true
  max-accounts-per-ip = 0
  notify-staff        = true
}
```

| Key | Default | What it does |
|-----|---------|--------------|
| `enabled` | `true` | Master switch. Off, and nothing is recorded and `/ipalts` has nothing to show. |
| `max-accounts-per-ip` | `0` | The most distinct accounts allowed from one address; a join that would exceed it is kicked. `0` means **no cap** — the guard only observes and notifies. Set it to, say, `3` to hold each household to three accounts. |
| `notify-staff` | `true` | Notify online staff (holders of `uxmessentials.security.alts.notify`) when a joining player shares an address with other accounts. The notice carries the account names and a count — never the address itself. |

---

## ClientID

On join the module reads the client brand a player reports (the `minecraft:brand`
channel — `vanilla`, `fabric`, a known cheat client, …) and acts on it.
`/clientinfo <player>` shows the recorded brand for the current session.

```hocon
client-id {
  enabled = true
  mode    = flag
  brands  = []
}
```

| Key | Default | What it does |
|-----|---------|--------------|
| `enabled` | `true` | Master switch. Off, and no brand is read and `/clientinfo` has nothing to show. |
| `mode` | `flag` | How the `brands` list is applied (see below). An unrecognised value falls back to `flag`, the mode that never kicks. |
| `brands` | `[]` | The brands the mode acts on, matched case-insensitively. Empty by default, so `flag` mode simply records every brand for `/clientinfo` without flagging anything. |

The three modes:

| `mode` | Behaviour |
|--------|-----------|
| `block-list` | Deny the listed brands, allow everything else — keep known cheat clients out. |
| `allow-list` | Allow **only** the listed brands, deny everything else — admit a fixed set of approved clients. |
| `flag` | Never deny; a listed brand is only flagged to staff and logged — observe before you enforce. |

---

## Where the secrets live

Nothing sensitive in this module is stored in the clear.

| Secret | How it's stored |
|--------|-----------------|
| **PIN** | A salted, one-way **PBKDF2** hash in the `security_2fa` table, serialised as `algorithm:salt:hash`. The plugin verifies against it with a constant-time compare and can never reconstruct the PIN. |
| **TOTP secret** | **AES-256-GCM** encrypted at rest, keyed by a random key-file the module creates on first run at `modules/security/secret.key` (owner-only permissions on POSIX). It's authenticated encryption, so a tampered column fails to decrypt rather than mis-verifying. |
| **Device trust** | The address is a **SHA-256** token, never the raw IP — enough to make two connections from the same address collide, but nothing reversible. A trust match only *skips the keypad*; it's never treated as proof of identity on its own. |
| **Alt links** | Same one-way IP token; account UUIDs keyed by it. No address, no GeoIP. |

<Callout type="tip" title="Back up the key-file separately">

`modules/security/secret.key` decrypts every stored TOTP secret. It lives in a file —
not the config or the database — precisely so you can back it up and permission it
apart from the world data a rollback would touch. Lose it and enrolled players re-run
`/2fa setup`; leak it and TOTP secrets become readable, so treat it like a private key.

</Callout>

---

## The `config.conf`

The complete `modules/security/config.conf`, with the shipped defaults:

```hocon
enabled = true

# Two-factor enrolment. /2fa (authenticator app) or /pin set <pin> (numeric PIN);
# either is a valid second factor.
two-factor {
  enabled = true

  # Which factors a player may enrol. Turn either off to offer only the other.
  totp = true
  pin  = true

  # The issuer label shown next to the account in the authenticator app.
  issuer = "uxmEssentials"

  # 30-second time-steps of tolerance on either side of the current step when checking a
  # code. 1 (±30s) absorbs modest clock drift; 0 is strictest. Capped at 5.
  code-window = 1

  # The allowed PIN length range, in digits. A PIN is always digits only.
  pin-min-length = 4
  pin-max-length = 8

  # PINs refused outright, however well-formed they are. Add your own, or [] to refuse none.
  blocked-pins = [
    "0000", "1111", "2222", "3333", "4444", "5555", "6666", "7777", "8888", "9999",
    "1234", "4321", "12345", "123456", "654321", "1212", "2580", "0852", "1122",
    "2000", "1990", "2024", "2025"
  ]
}

# Join verification. An enrolled player joining from an unverified device is frozen and
# shown a keypad. A non-enrolled player is never frozen.
join-verification {
  enabled = true

  # Remember a verified device so the next join from the same address skips the keypad.
  # The address is stored only as a one-way hash.
  trust-devices = true

  # How long a remembered device skips the keypad, in hours. 0 disables the trust window.
  trust-duration-hours = 24

  # Wrong PINs/codes allowed before a kick.
  max-attempts = 3

  # How long, in seconds, a locked-out player is kept out.
  lockout-seconds = 300

  # Whether that lockout is also a real tempban through the moderation module, so it lands
  # in the same ban table, history and /unban as every other ban. With moderation disabled
  # there is nothing to write to and the lockout stays in memory.
  lockout-bans       = true
  lockout-ban-reason = "Too many failed verification attempts"

  # How long, in seconds, a frozen player has to verify before being kicked. 0 = no limit.
  entry-timeout-seconds = 60

  # What to do with a player who has to verify while in spectator (a spectator cannot click
  # the keypad). adventure | survival | none.
  spectator-mode = adventure

  # What happens when the verification decision itself fails (DB down, key-file unreadable).
  # kick = refuse the join; allow = let them in unverified.
  safety-net = kick

  # What a frozen player is stopped from doing. All ship on; turn one off to allow it.
  restrictions {
    move          = true
    commands      = true
    chat          = true
    interact      = true
    drop          = true
    pickup        = true
    block-edit    = true
    damage-taken  = true
    damage-dealt  = true
    mob-targeting = true
    teleport      = true
    inventory     = true
    consume       = true
    hunger        = true
  }

  # Where to park a verifying player, world,x,y,z[,yaw,pitch]. Blank leaves them put; they
  # are returned to exactly where they were the moment they verify.
  holding-area = ""

  # A proxy backend to send the player to once they verify. Blank leaves them here.
  transfer-to = ""

  # What happens to an online player whose factor staff just cleared with /security reset.
  # reverify | kick | nothing.
  on-access-revoked = reverify

  # Let an installed login plugin (AuthMe and its forks) authenticate first, before this
  # module asks for a second factor. No login plugin installed = nothing changes.
  wait-for-login-plugin = true
}

# How verification talks back to the player, beyond the chat lines. Any vanilla or
# resource-pack sound key works; leave one blank ("") to play nothing.
feedback {
  titles = true

  sounds {
    prompt  = "block.note_block.pling"
    key     = "ui.button.click"
    success = "entity.player.levelup"
    failure = "entity.villager.no"
  }
}

# Op-command protection. Dangerous commands are held behind a fresh second-factor proof.
# Only enrolled players are gated; uxmessentials.security.bypass is exempt; the console
# is never gated.
op-protection {
  enabled = true

  # How long, in seconds, a successful verification counts as recent. 0 forces a fresh
  # proof for every protected command.
  reauth-window-seconds = 60

  # The commands to protect, by root name (ignores a leading slash, arguments, and a
  # namespace prefix).
  protected-commands = [
    "op",
    "deop",
    "stop",
    "restart",
    "reload",
    "gamemode",
    "ban",
    "ban-ip",
    "pardon",
    "kick",
    "whitelist"
  ]
}

# IP / alt guard. On join the address is recorded as a one-way token (the raw IP is NEVER
# stored — no GeoIP, nothing reversible). /ipalts <player> lists same-IP accounts.
ip-guard {
  enabled = true

  # The greatest number of distinct accounts allowed from one address. 0 means no cap —
  # the guard only observes and notifies, never kicks.
  max-accounts-per-ip = 0

  # Notify online staff (uxmessentials.security.alts.notify) when a join shares an address.
  # The notice carries names and a count — never the address.
  notify-staff = true
}

# ClientID. Read the client brand a joining player reports and act on it.
# /clientinfo <player> shows the recorded brand.
client-id {
  enabled = true

  # How the brand list is applied:
  #   block-list — deny the listed brands, allow everything else
  #   allow-list — allow ONLY the listed brands, deny everything else
  #   flag       — never deny; a listed brand is only flagged to staff and logged
  # An unrecognised value falls back to flag, the safe mode that never kicks.
  mode = flag

  # The brands the mode acts on, matched case-insensitively.
  brands = []
}
```

Config is loaded once on enable and swapped atomically on `/uxmess reload security`, so a
command or listener that reads it mid-reload always sees one coherent snapshot. Delete a
line and it falls back to the shipped default above.

---

## Permissions

| Node | Default | What it grants |
|------|---------|----------------|
| `uxmessentials.security.2fa` | `true` | `/2fa` — set up, confirm or disable the authenticator second factor on your own account |
| `uxmessentials.security.pin` | `true` | `/pin` — set, change, remove or lock the numeric PIN second factor on your own account |
| `uxmessentials.security.pin.required` | `false` | Holders **must** have a PIN: a holder with no factor is shown the create pad on join and cannot play until they set one |
| `uxmessentials.security.admin` | `op` | `/security` — inspect and manage another player's second factors. Gates the whole tree |
| `uxmessentials.security.bypass` | `op` | Exempt from the join-verification freeze and the op-command re-auth checks |
| `uxmessentials.security.force` | `op` | `/security force <player>` — make a player re-verify their second factor |
| `uxmessentials.security.reset` | `op` | `/security reset <player> [totp\|pin\|all]` — clear a factor a player can no longer prove |
| `uxmessentials.security.alts` | `op` | `/ipalts` — list the accounts that share an IP with a player |
| `uxmessentials.security.clientinfo` | `op` | `/clientinfo` — show the client brand a player reported |
| `uxmessentials.security.alts.notify` | `op` | Receive the staff notice when a join shares an IP with other accounts or reports a flagged client |
| `uxmessentials.module.security` | `op` | Reload / inspect the module (`/uxmess reload security`) |

The two self-service nodes ship `true` so every player can protect their own account; the
staff reads and the bypass default to `op`. `pin.required` ships `false` and is entirely
opt-in — grant it to a rank, to your staff, or to everyone.

---

## Next Steps

- [🔑 Permission Reference](../permissions/reference.md) — the full `uxmessentials.security.*` node list
- [🧩 Per-Module Config](../config/per-module.md) — where `modules/security/config.conf` lives and how it's loaded
- [🕵️ Staff Mode](staff-mode.md) — the on-duty toolkit for the staff who hold the bypass node
- [🛡️ Moderation](moderation.md) — bans, mutes and warnings, several of which are protected commands

---
title: Communication
order: 1440
description: 'The communication module owns the server''s "voice"; everything the server
  says to players that isn''t a private message. The public chat format, join and quit
  lines, the rotating announcer, info pages like /rules and /motd, chat control, and
  one-off broadcasts all live here. It''s driven by five HOCON sub-files under
  modules/communication/, and every line of text is MiniMessage-formatted.'
---

---

## Public Chat Format

Everything a player types in public chat is rendered through the format in
`chat.conf` instead of the vanilla `Name: message`. The shipped default is
deliberately near-vanilla, so turning the module on changes nothing surprising until
you author your own format.

```hocon
chat {
  enabled = true
  format = "<prefix><display_name><suffix><gray>:</gray> <message>"

  # Per-rank overrides — each key is a LuckPerms primary group (matched case-insensitively).
  group-formats {
    # admin = "<red>[Admin] <prefix><display_name><suffix><gray>:</gray> <message>"
  }

  allow-player-format = false
  name-hover = ""
  name-click = ""
}
```

| Key | What it does |
|-----|--------------|
| `enabled` | `false` leaves chat entirely to the server (vanilla format). Default `true`. |
| `format` | The MiniMessage format applied to every speaker with no group override. |
| `group-formats` | Optional per-rank formats, keyed by the speaker's **LuckPerms primary group** (matched case-insensitively). A group with no entry uses `format`. |
| `allow-player-format` | When `true`, a speaker who holds the format permission may write MiniMessage in their own message. Left `false`, a player's message is always inserted as plain text, so any tags they type show up literally. |
| `name-hover` | Optional MiniMessage hover text shown on the name span. Blank = none. |
| `name-click` | Optional click target for the name span: clicking suggests it in the player's chat box. Supports `{name}`. Blank = none. |

The format is MiniMessage; these placeholders are filled in per speaker before it is
parsed:

| Placeholder | Resolves to |
|-------------|-------------|
| `<prefix>` | The speaker's LuckPerms chat prefix (empty when unset / LuckPerms absent) |
| `<suffix>` | The speaker's LuckPerms chat suffix |
| `<display_name>` | The speaker's display-name component (carries the `name-hover` / `name-click` decoration) |
| `<name>` | The speaker's plain account name |
| `<world>` | The world the speaker is in |
| `<message>` | What the speaker typed |

On top of these MiniMessage tokens, the whole format is **PlaceholderAPI-expanded**
when PlaceholderAPI is installed, so any `%...%` placeholder: `%luckperms_prefix%`, a
player's balance, anything a PAPI expansion publishes: is filled in per speaker as
well. With PlaceholderAPI absent, `%...%` is simply left untouched.

<Callout type="info" title="LuckPerms prefix and suffix: MiniMessage or legacy codes">

`<prefix>` and `<suffix>` are pulled straight from LuckPerms. A MiniMessage prefix
(`<red>[VIP]`) is parsed as-is, so the colours and gradients you set on a group
carry through unchanged. A prefix written with **legacy `&` / `§` colour codes**
(`&c[VIP]`) is now detected and rendered as colour too, so older LuckPerms setups
no longer show raw codes in chat. Legacy **hex** (`&#rrggbb`) is not supported,
only the standard `&0-9a-fk-orx` / `§` codes, and a prefix that mixes both `&` and
`§` renders in whichever dialect dominates. Without LuckPerms both resolve to empty
and the rest of the line still renders.

</Callout>

<Callout type="warning" title="Player-written formatting is gated twice">

A speaker's own `<message>` is only parsed as MiniMessage when **both**
`allow-player-format = true` **and** the speaker holds
`uxmessentials.communication.chat.format`. Otherwise the message is inserted as
plain text: this is what stops ordinary players from smuggling colour tags or
click events into chat.

</Callout>

---

## Connection Messages

The join, quit, death, and first-join lines are templated in `join-quit.conf`.
Because they're MiniMessage templates with placeholders, you can colour them by
rank, welcome first-timers differently, or make a death message name the killer.

Every channel shares the same two switches:

- `mode`: `DEFAULT` keeps the vanilla message, `DISABLE` hides it entirely, and
  `CUSTOM` renders one of your templates.
- `ordering`: `SEQUENTIAL` (author order, wrapping) or `RANDOM` selection from the
  `templates` list.

These placeholders are available in every connection template:

| Placeholder | Resolves to |
|-------------|-------------|
| `{player}` | The account name |
| `{displayname}` | The player's display name (flattened to plain text) |
| `{count}` | The online player count |
| `{world}` | The player's world name |

### Per-rank join messages

The `join` block is the **default** policy; it greets every player whose LuckPerms
primary group has no entry under `groups`. A `groups` map lets one rank read
differently: each key is a permission group (matched case-insensitively) and its
value a list of templates chosen with the block's `ordering`.

```hocon
join {
  mode = CUSTOM
  ordering = SEQUENTIAL
  templates = [
    "<yellow>{player}</yellow> <gray>joined — <white>{count}</white> online."
  ]
  groups {
    admin = [ "<red><bold>{player}</bold></red> <gray>joined." ]
    vip   = [ "<gold>★ {player}</gold> <gray>joined — <white>{count}</white> online." ]
  }
}
```

The `quit` block has the same shape (minus per-group overrides) and greets a
disconnecting player.

### First-join welcome

The `first-join` block fires **once**: the first time the server has ever seen a
player (`!hasPlayedBefore()`). When a welcome template is authored (`mode = CUSTOM`),
it **replaces the ordinary join line for that one join**, so every viewer sees the
welcome instead of the normal join broadcast. Leave it `DEFAULT` to disable.

```hocon
first-join {
  mode = CUSTOM
  ordering = SEQUENTIAL
  templates = [
    "<gold>Welcome <white>{player}</white> to the server! Player #<white>{count}</white>.</gold>"
  ]
}
```

<Callout type="tip" title="Legacy single-string form still works">

The older `first-join = "<gold>Welcome {player}!</gold>"` single-string form is
still accepted alongside the block form above.

</Callout>

### Per-cause death messages

The `death` block is the **default** policy; it applies to every death whose cause
has no entry under `causes`. A `causes` map lets one cause read differently: each key
is a damage cause and its value a list of templates chosen with the block's
`ordering`. A cause with no override simply uses the default death templates.

```hocon
death {
  mode = CUSTOM
  ordering = SEQUENTIAL
  templates = [
    "<red>{player}</red> <gray>died: <white>{message}</white>"
  ]
  causes {
    PVP  = [ "<red>{player}</red> <gray>was slain by <red>{killer}</red> <gray>using <white>{killer_weapon}</white>" ]
    FALL = [ "<gray>{player} <white>fell from a high place" ]
    MOB  = [ "<gray>{player} <white>was cut down by a monster" ]
  }
}
```

Death templates expose four placeholders on top of the shared connection tokens:

| Placeholder | Resolves to |
|-------------|-------------|
| `{message}` | The vanilla death line as plain text |
| `{cause}` | The classified death cause as a **lowercase** key: `fall`, `pvp`, `lava` and so on (the same causes listed below, lowercased) |
| `{killer}` | The killer's name when a **player** dealt the blow (empty otherwise) |
| `{killer_weapon}` | The killer's held item; its custom name if renamed, else a readable material like `Diamond Sword` (empty otherwise) |

The recognised cause keys are:

```
PVP  MOB  PROJECTILE  FALL  FIRE  LAVA  DROWNING  EXPLOSION  VOID  LIGHTNING
SUFFOCATION  STARVATION  POISON  MAGIC  WITHER  FALLING_BLOCK  THORNS
DRAGON_BREATH  FLY_INTO_WALL  HOT_FLOOR  CONTACT  FREEZE  SONIC_BOOM
```

Each of these does double duty: it's the key that **selects** a per-cause template
under `causes`, and the very same cause is available **inside** any death template as
the `{cause}` token, lowercased (`PVP` → `pvp`, `FALL` → `fall`). So a single default
template like `<red>{player}</red> <gray>died from <white>{cause}</white>` reads the
cause without needing a `causes` entry per kind.

`PVP` and `MOB` are decided by **who** struck the final blow (another player, or a
creature) rather than the raw damage type; the rest mirror the environmental damage
kinds. Any cause the plugin doesn't translate simply falls through to the default
death templates. An unknown key in `causes` is ignored.

You can also point a player at an info page the moment they die with
`death-info-page = "<page-name>"` (leave it blank to disable).

### Personal MOTD on join

The root `motd` list is a personal message sent **only to the joining player**:
never broadcast, after the join line is settled. Lines are MiniMessage and are
PlaceholderAPI-expanded when PlaceholderAPI is installed.

```hocon
motd = [
  "<gray>Welcome back, <white>{player}</white>!"
  "<gray>Type <yellow>/help</yellow> for commands."
]
```

<Callout type="info" title="How the join messages layer up">

On a first-ever join, the player triggers the **first-join welcome** (broadcast
to everyone, in place of the normal join line) *and* receives their personal
**MOTD** (only they see it). On a returning join, the **per-rank join line** is
broadcast and the MOTD is sent to the joiner. The join broadcast and the MOTD are
independent: a server can show one, both, or neither.

When the `motd` list is left empty, the plugin falls back to the legacy `motd`
info page from `info-pages.conf` (gated by `motd-on-join`): the same body
`/motd` prints.

</Callout>

---

## The Rotating Announcer

The announcer posts messages to chat on a timer: tips, rules reminders, event
teasers. It's configured in `announcer.conf`:

- `announcements[]`: the pool of messages.
- `default-interval-seconds`: how often to post.
- `min-players`: don't announce to an empty server.
- `ordering`: sequential or random rotation.

Staff manage it live with `/announce`:

| Command | Effect |
|---------|--------|
| `/announce editor` | Open the announcer editor GUI |
| `/announce list` | List the configured announcements |
| `/announce preview <id>` | Preview one message |
| `/announce toggle` | Turn the rotation on/off |
| `/announce reload` | Re-read the announcer config |

`/announce` is gated by `uxmessentials.announce.admin`. Players who'd rather not see
the rotation can opt out with `/broadcasttoggle`.

---

## Info Pages

Info pages are static, paginated text screens. Three ship by default:
`/info`, `/rules`, and `/motd`: configured in `info-pages.conf` under
`info-pages { info, rules, motd }`, with `motd-on-join` deciding whether the MOTD
shows automatically on login.

<Callout type="tip" title="Add your own pages, get a command for free">

Define a new page in `info-pages.conf` and the module **auto-registers a
`/<name>` command** for it (gated by `uxmessentials.communication.info.<name>`).
Add a `staffguide` page and `/staffguide` exists instantly: no code, no restart
beyond a reload.

</Callout>

---

## Chat Control

| Command | Aliases | Effect |
|---------|---------|--------|
| `/clearchat` | `chatclear` | Flush the chat window for everyone |
| `/togglechat` | `mutechat` | Lock public chat for non-staff |
| `/me <action>` | - | Post a third-person action message |

`uxmessentials.communication.clearchat.exempt` lets a player's view survive a
`/clearchat`, and `uxmessentials.communication.chat.bypass` lets staff still speak
while chat is locked with `/togglechat`.

---

## Broadcasts

For one-off announcements:

| Command | Aliases | Scope |
|---------|---------|-------|
| `/broadcast <message>` | - | The whole server |
| `/broadcastworld <message>` | `bcw` | Only players in your world |

These are immediate, MiniMessage-formatted messages: distinct from the timed
announcer.

---

## Achievement Notifications

The fourth file, `advancements.conf` (off by default), controls custom advancement /
achievement broadcasts: an allow/deny list of which advancements are announced, the
template used, and which channels they post to. Turn it on to celebrate milestones
in chat with your own wording instead of the vanilla toast alone.

---

## Config at a Glance

| File | Owns |
|------|------|
| `chat.conf` | The public chat format, per-rank overrides and name decoration |
| `join-quit.conf` | Join / quit / death / first-join templates and the personal MOTD |
| `announcer.conf` | The rotating announcer pool and timing |
| `info-pages.conf` | `/info`, `/rules`, `/motd` and custom pages |
| `advancements.conf` | Achievement broadcasts (off by default) |

---

## Next Steps

- [💬 Chat & Messaging Commands](../commands/chat-messaging.md): the full command reference
- [🎛️ Custom Menu Engine](../menus/engine.md): how the announcer editor GUI is built
- [🧩 Per-Module Config](../config/per-module.md): the four `communication/*.conf` files
- [🔑 Permission Reference](../permissions/reference.md): the `uxmessentials.communication.*` nodes

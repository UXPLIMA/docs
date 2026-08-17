---
title: Punishment sync
order: 39
description: Mapping bans, mutes and kicks between Minecraft and Discord, both ways.
icon: gavel
---

A ban on one side becomes a ban on the other. Works with LiteBans, AdvancedBan and vanilla Minecraft
punishments.

Off by default, in `punishment-sync.yml`.

```yaml
punishment-sync:
  enabled: false

  discord-actions:
    ban: "ban"
    mute: "timeout"
    kick: "kick"
    warn: "none"

  minecraft-actions:
    ban: "ban"
    kick: "kick"
    timeout: "mute"
    warn: "none"
```

`discord-actions` maps a Minecraft punishment to what happens in Discord; `minecraft-actions` maps
the other direction.

| Direction | Values |
|---|---|
| Discord actions | `ban`, `kick`, `timeout`, `mute`, `none` |
| Minecraft actions | `ban`, `kick`, `mute`, `warn`, `none` |

`none` disables that mapping. The default configuration syncs bans, mutes and kicks and leaves
warnings alone, which is the right starting point — a warning is a conversation, not a sanction.

## Timeouts have a ceiling

Discord's timeout maxes out at 28 days. A longer Minecraft mute maps to a 28-day timeout, not to a
longer one — Discord will not accept it. For a permanent mute, either accept the 28 days or map
`mute` to `"mute"`, which uses a role instead and has no limit.

## The bypass list

```yaml
bypass:
  players:
    - "AdminPlayer"
    - "OwnerPlayer"
  discord-roles:
    - "123456789012345678"
  discord-users:
    - "123456789012345678"
```

Nobody in these lists is affected by the sync in either direction. Put your staff here.

Note that `players` takes **names**, not UUIDs — a renamed account falls out of the list, and a
player who takes the old name falls into it. Review this list when someone changes their name.

<Callout type="danger" title="Test the direction you care about, on a test account">

Punishment sync acts automatically and both ways. A misconfigured `minecraft-actions.ban` means a
Discord ban — including one handed out by another bot, or by a moderator who does not know this
plugin exists — bans that person from the game server. Confirm which side your moderators actually
work on, and set the other direction to `none` if you do not want it.

</Callout>

## What it does not do

It does not import history. Enabling the plugin does not go back and apply existing bans on either
side; only punishments issued after it is on are synchronised. Existing bans stay where they were
issued.

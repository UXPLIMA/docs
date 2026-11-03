---
title: commands.conf
order: 602
description: The name, aliases and switch of every command and subcommand, and words per language.
icon: square-terminal
---

```hocon
commands {
  channel {
    name    = "channel"
    aliases = ["ch"]
    enabled = true
    subcommands {
      switch { name = "switch" }
      lock   { name = "lock" }
    }
  }
}
```

| Key | What it does |
|---|---|
| the block name | The command's key. It never changes |
| `name` | The label a player types |
| `aliases` | Other labels |
| `enabled` | `false` and the command is never registered |
| `localized-aliases` | Words for the readers of one language: `localized-aliases { tr = ["kanal"] }` |
| `subcommands` | One block per subcommand; `enabled = false` takes that word out and leaves the rest |

A word under a word is written with a hyphen on the left and a space on the right: `lang-set` is the key under which
`lang set` is named. The operator command also answers to `<player command>admin`: `/chatadmin`. A change needs a
restart. A renamed subcommand keeps its node.

| Key | Ships as | Subcommand keys |
|---|---|---|
| `bossbar` | `/bossbar` | |
| `broadcast` | `/broadcast`, `/bc` | |
| `channel` | `/channel`, `/ch` | `switch`, `join`, `leave`, `list`, `info`, `lock`, `unlock`, `slowmode`, `force`, `kick` |
| `chat` | `/chat` | `settings`, `lang`, `lang-set`, `lang-reset` |
| `chatcolor` | `/chatcolor`, `/chatcolour` | `set`, `style`, `clear`, `list` |
| `chatdelete` | `/chatdelete` | |
| `chateditor` | `/chateditor` | `channels`, `tags`, `announcements`, `announcement` |
| `announcements` | `/announcements` | `preview`, `say` |
| `chatgame` | `/chatgame`, `/cg` | `top`, `answer`, `start` |
| `chatlock` | `/chatlock`, `/muteall`, `/lockchat` | |
| `chatlog` | `/chatlog` | `player`, `search`, `audit` |
| `snapshot` | `/chatsnapshot` | |
| `clearchat` | `/clearchat` | `self` |
| `conversation` | `/conversation`, `/convo` | |
| `emoji` | `/emoji`, `/emojis` | |
| `friend` | `/friend`, `/friends`, `/f` | `add`, `accept`, `deny`, `remove`, `favourite`, `list`, `requests` |
| `helpop` | `/helpop` | |
| `ignore` | `/ignore` | `remove`, `list` |
| `macro` | `/macro` | |
| `mail` | `/mail` | `read`, `send`, `clear` |
| `me` | `/me` | |
| `msg` | `/msg`, `/tell`, `/w`, `/whisper`, `/m`, `/pm` | |
| `mute` | `/mute` | |
| `nick` | `/nick`, `/nickname` | `off`, `player`, `realname` |
| `poll` | `/poll` | `start`, `end` |
| `quickreply` | `/quickreply`, `/qr` | `add`, `remove`, `list` |
| `quote` | `/quote` | |
| `recent` | `/recent` | |
| `reply` | `/reply`, `/r` | |
| `rules` | `/rules` | `accept` |
| `staff-list` | `/stafflist`, `/staff` | |
| `tag` | `/tag`, `/tags` | `list`, `wear`, `clear`, `buy`, `favourite`, `search`, `make`, `pending`, `approve`, `deny`, `give`, `take` |
| `unignore` | `/unignore` | |
| `unmute` | `/unmute` | |
| `uxmchat` | `/uxmchat`, `/chatadmin` | `reload`, `version`, `doctor`, `lang-set-player`, `lang-reset-player` |
| `vote` | `/vote` | |

## Words per language

A word in `localized-aliases` on a command works for anyone who types it, but only a player who reads that language
finds it in their command list. A word under a subcommand is offered to, and works for, the readers of that language
and the console only. The language a player reads is the one they chose, or their client's.

Related: [Commands](../commands/overview.md)

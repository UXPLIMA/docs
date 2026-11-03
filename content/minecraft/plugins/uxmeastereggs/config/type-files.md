---
title: Type files
order: 502
description: The file of one egg type under types/, every key, and what a missing or wrong key does.
icon: file-type
---

Every egg type is one file, `types/<type>.conf`, and the file name is its name. The name holds lower-case letters,
digits, `-` and `_`, up to 32; a file with any other name is skipped with a warning. `/uxmeastereggs reload` reads
every type again. The shipped `golden.conf`:

```hocon
display-name = "@type.golden.name"
texture = ""
cooldown = ""
reward {
  money    = ["exp 500"]
  commands = []
}
particle {
  name   = "END_ROD"
  count  = 5
  spread = 0.25
  speed  = 0.01
  height = 0.9
}
effects = [
  "[sound] entity.player.levelup 1.0 1.4",
  "[message] @egg.found",
  "[subtitle] @type.golden.name | @type.golden.hint 0.3 2.5 0.5"
]
```

## The keys

| Key | Left out | What it is |
|---|---|---|
| `display-name` | the type's name | The name players read. `@key` for a line of the messages files, read in each player's language, or your own MiniMessage with the theme roles: `<warn>Golden Egg` |
| `texture` | `""` | The head drawn in the menus: a skin URL `https://textures.minecraft.net/texture/<hash>` or a base64 texture value. Anything else, empty included, draws a plain head. A player name is not read |
| `cooldown` | `""` | Empty: found once and never again. A time such as `30m`, `12h` or `1d` makes the type repeatable. An unreadable value is treated as empty, with a warning |
| `reward.money` | `[]` | Money lines paid at every find. See [Rewards](../hunting/rewards.md#the-money-lines) |
| `reward.commands` | `[]` | Console commands run at every find; `%player%` is the finder's name |
| `particle` | none | The idle particle. See below |
| `effects` | `[]` | What the finder sees and hears at every find. See below |

`texture` is for the menus only. The block on the map is whatever you built there.

## particle

| Key | Shipped in `common` | What it does |
|---|---|---|
| `name` | `"HAPPY_VILLAGER"` | A vanilla particle name, such as `FLAME`, `END_ROD`, `SOUL_FIRE_FLAME`. One that needs data takes it after a space: `"dust #ff0000"`, `"block oak_log"`. Empty draws nothing |
| `count` | `3` | How many per draw. `0` draws nothing. Not negative |
| `spread` | `0.2` | How far they scatter, in blocks. Not negative |
| `speed` | `0.0` | The vanilla particle speed; `0` keeps them still. Not negative |
| `height` | `0.8` | How far above the middle of the block they are drawn |

A name the server does not know, or data it cannot read, draws nothing and the console says so once.

## effects

A list of action lines, run at every find of an egg of this type, first or repeat. The values `<player>`, `<type>`,
`<found>` and `<total>` are filled into its `@key` lines. Every verb is on [Moments](../hunting/moments.md#writing-a-line).

```hocon
effects = [
  "[sound] entity.experience_orb.pickup 1.0 1.2",
  "[message] @egg.found",
  "[actionbar] @egg.count"
]
```

An empty list makes the type silent; the `progress` moment and the milestones still run.

## The old feedback block

A type file an older release wrote may carry `feedback { sound, volume, pitch, chat, action-bar, title, subtitle,
fade-in-ms, stay-ms, fade-out-ms }` instead of `effects`. It is still read, as the matching action lines, while the
file has no `effects`. Write `effects` to replace it.

## When a file is wrong

A file that cannot be read, such as a negative particle count or a money line that names no amount, is skipped with
the reason in the console, and the other types load. Its eggs answer *This egg has no type* until it is fixed.

Related: [Types](../hunting/types.md), [Moments](../hunting/moments.md), [Rewards](../hunting/rewards.md)

---
title: Day files
order: 302
description: Every key of a day file, the lines a reward is written in, the values they can use, and the shipped 28 days.
icon: file-text
---

Every day is a file in `days/`, named after its number: `days/7.conf` is day 7.

```hocon
icon {
  material   = "DIAMOND"
  model-data = 0
  glow       = true
}

reward = [
  "[console] give %player% diamond 2",
  "[give-money] exp 110"
]

ranks {
  vip { reward = [ "[console] give %player% diamond 4", "[give-money] exp 220" ] }
  mvp { reward = [ "[console] give %player% diamond 6", "[give-money] exp 330" ] }
}
```

## The keys

| Key | Default | What it does |
|---|---|---|
| `icon.material` | `"CHEST"` | The day's own icon. The shipped window draws a day by its state instead; see [Calendar window](../menus/calendar.md#icons) |
| `icon.model-data` | `0` | A resource pack model for the day's tile; `0` for none |
| `icon.glow` | `false` | The day's tile shines, whatever its state |
| `reward` | `[]` | What everybody gets, as action lines |
| `ranks.<rank>.reward` | | What that rank gets instead of `reward`. See [Ranks](ranks.md) |

The name and the description of a day are not in the file: they are lines of the messages files, so they can be
translated. See [Messages](../config/messages.md).

## Writing a line

A line is `[verb] payload`, the same lines every UXM plugin runs.

| Verb | Payload | Does |
|---|---|---|
| `[console]` | `<command>` | Runs a command as the server, without the slash |
| `[player]` | `<command>` | Runs a command as the player |
| `[give-money]` | `<economy> <amount>` | Pays into an economy: `exp`, `levels`, `vault`, `vault-unlocked`, `playerpoints`, `ecobits:<currency>`, `treasury`. See [Economies](../integrations/economies.md) |
| `[take-money]` | `<economy> <amount>` | Takes from one |
| `[message]` | `<text>` | Sends the text to the player |
| `[broadcast]` | `<text>` | Sends the text to everybody; an `@` line is read by each in their own language |
| `[actionbar]` | `<text>` | Shows the text over the hotbar |
| `[title]` | `<text>` | Shows the text across the middle of the screen |
| `[subtitle]` | `<title> \| <subtitle> [in stay out]` | A title with a line under it. The times are seconds, `0.5 3.5 1` when left out |
| `[bossbar]` | `<seconds> [colour] [overlay] [drain] \| <text>` | A bar across the top of the screen for that long |
| `[sound]` | `<key> [volume] [pitch]` | Plays a sound to the player |
| `[effect]` | `<effect> <seconds> [level] [hidden]` | Gives a potion effect |
| `[particle]` | `<name> [count] [spread] [data]` | Particles where the player stands; data is a colour `#ff0000`, a block or an item |
| `[take-item]` | `<item> [amount]` | Takes items from the player's inventory |
| `[close]` | | Closes the window the player has open |

Text that starts with `@` names a line of the messages files, read in the player's language with `<day>` and
`<streak>` filled in. Any other text is MiniMessage with the [theme](../config/theme.md) roles.

A brace block after the verb says when a line runs: `{delay=20}` in ticks, `{repeat=3, every=10}`, `{chance=0.1}`,
or `{if=<left> <operator> <right>}` with `==`, `!=`, `>=`, `<=`, `>`, `<` or `?=`, where a side may be a
PlaceholderAPI placeholder or `%mcmmo_level_<skill>%`.

## Values

| Token | Becomes |
|---|---|
| `%player%` | The player's name; `%player_name%`, `<player>` and `{player}` work too |
| `%day%` | The day claimed |
| `%streak%` | The streak after this claim |

They are filled in before the line runs, in every verb. For a player whose name is not a plain account name (letters,
digits and `_`, up to 16, with one leading mark such as a Bedrock `.`), the lines that name the player are skipped
with a warning, so a command never runs against the wrong account.

## The older form

A day written for an earlier release, with lines such as `console: give %player% bread 8` or `money: exp 50`, still
works: `console:`, `player:`, `message:`, `money: <economy> <amount>`, `sound: <key> [volume] [pitch]`,
`title: <title> | <subtitle> | <in> | <stay> | <out>` (spans such as `500ms` or `3s`) and
`particle: <name> [count] [spread] [data]` are read as the lines above. A `money:` amount may not be negative.

## When a line is wrong

A line the plugin cannot read is left out and named in the console with its file, at start and on reload; the rest
of the day still pays. A line naming an economy the server does not run is reported once at start and pays nothing.
A line that fails while it runs is logged, and the lines after it still run.

## The shipped days

Every shipped day gives an item with a `give` command and experience points; `vip` gets twice and `mvp` three
times the everyday reward. Days 7, 14, 21 and 28 glow.

| Day | Icon | Everybody | `vip` | `mvp` |
|---|---|---|---|---|
| 1 | `BREAD` | 8 `bread`, 50 exp | 16, 100 exp | 24, 150 exp |
| 2 | `IRON_INGOT` | 4 `iron_ingot`, 60 exp | 8, 120 exp | 12, 180 exp |
| 3 | `COAL` | 16 `coal`, 70 exp | 32, 140 exp | 48, 210 exp |
| 4 | `COOKED_BEEF` | 8 `cooked_beef`, 80 exp | 16, 160 exp | 24, 240 exp |
| 5 | `IRON_INGOT` | 8 `iron_ingot`, 90 exp | 16, 180 exp | 24, 270 exp |
| 6 | `GOLD_INGOT` | 4 `gold_ingot`, 100 exp | 8, 200 exp | 12, 300 exp |
| 7 | `DIAMOND`, glowing | 2 `diamond`, 110 exp | 4, 220 exp | 6, 330 exp |
| 8 | `IRON_INGOT` | 6 `iron_ingot`, 120 exp | 12, 240 exp | 18, 360 exp |
| 9 | `COAL` | 18 `coal`, 130 exp | 36, 260 exp | 54, 390 exp |
| 10 | `COOKED_BEEF` | 10 `cooked_beef`, 140 exp | 20, 280 exp | 30, 420 exp |
| 11 | `IRON_INGOT` | 10 `iron_ingot`, 150 exp | 20, 300 exp | 30, 450 exp |
| 12 | `GOLD_INGOT` | 6 `gold_ingot`, 160 exp | 12, 320 exp | 18, 480 exp |
| 13 | `BREAD` | 10 `bread`, 170 exp | 20, 340 exp | 30, 510 exp |
| 14 | `EMERALD`, glowing | 8 `emerald`, 180 exp | 16, 360 exp | 24, 540 exp |
| 15 | `COAL` | 20 `coal`, 190 exp | 40, 380 exp | 60, 570 exp |
| 16 | `COOKED_BEEF` | 12 `cooked_beef`, 200 exp | 24, 400 exp | 36, 600 exp |
| 17 | `IRON_INGOT` | 12 `iron_ingot`, 210 exp | 24, 420 exp | 36, 630 exp |
| 18 | `GOLD_INGOT` | 8 `gold_ingot`, 220 exp | 16, 440 exp | 24, 660 exp |
| 19 | `BREAD` | 12 `bread`, 230 exp | 24, 460 exp | 36, 690 exp |
| 20 | `IRON_INGOT` | 8 `iron_ingot`, 240 exp | 16, 480 exp | 24, 720 exp |
| 21 | `NETHERITE_SCRAP`, glowing | 2 `netherite_scrap`, 250 exp | 4, 500 exp | 6, 750 exp |
| 22 | `COOKED_BEEF` | 14 `cooked_beef`, 260 exp | 28, 520 exp | 42, 780 exp |
| 23 | `IRON_INGOT` | 14 `iron_ingot`, 270 exp | 28, 540 exp | 42, 810 exp |
| 24 | `GOLD_INGOT` | 10 `gold_ingot`, 280 exp | 20, 560 exp | 30, 840 exp |
| 25 | `BREAD` | 14 `bread`, 290 exp | 28, 580 exp | 42, 870 exp |
| 26 | `IRON_INGOT` | 10 `iron_ingot`, 300 exp | 20, 600 exp | 30, 900 exp |
| 27 | `COAL` | 22 `coal`, 310 exp | 44, 620 exp | 66, 930 exp |
| 28 | `NETHERITE_INGOT`, glowing | 1 `netherite_ingot`, 320 exp | 2, 640 exp | 3, 960 exp |

Related: [Ranks](ranks.md), [The calendar](calendar.md), [Calendar window](../menus/calendar.md)

---
title: Your first crate
order: 204
description: Writing a crate with a key and two groups, placing it, giving keys, and opening it.
icon: footprints
---

This walkthrough writes a vote crate with its own key, places it in the world and opens it.

## 1. The key

Create `keys/vote-key.conf`:

```hocon
name { en = "Vote Key", tr = "Oy Anahtarı" }
item {
  material = "TRIPWIRE_HOOK"
  glow     = true
  lore     = [ "<subtext>Opens the vote crate." ]
}
tradeable = true
menu {
  order = 20
  store = [ "<subtext>Vote with /vote to earn one." ]
}
```

`tradeable = true` lets players pass the key on with `/crates send`.

## 2. The crate

Create `crates/vote.conf`:

```hocon
name { en = "Vote Crate", tr = "Oy Kasası" }
key { id = "vote-key", amount = 1, kind = "either" }
shape { type = "block", material = "ENDER_CHEST" }
animation = "reel"
cooldown  = 0
broadcast { enabled = true, threshold = 3 }
hologram {
  lines = [ "<accent>Vote Crate", "<subtext>Right click to open" ]
  height = 1.6
}
rarities {
  common { name = "Common", colour = "muted", order = 1 }
  rare   { name = "Rare",   colour = "info",  order = 3 }
}
groups {
  loot {
    name = "Loot"
    rewards {
      steak   { weight = 70, display { name = "Steak",    rarity = "common" }, items = ["COOKED_BEEF 16"] }
      diamond { weight = 30, display { name = "Diamonds", rarity = "rare" },   items = ["DIAMOND 2"] }
    }
  }
  bonus {
    name = "Bonus"
    rewards {
      exp  { weight = 80, display { name = "Experience", rarity = "common", icon { material = "EXPERIENCE_BOTTLE" } }, money = ["exp 200"] }
      levels { weight = 20, display { name = "Five levels", rarity = "rare", icon { material = "ENCHANTED_BOOK" } }, money = ["levels 5"] }
    }
  }
}
```

Each opening gives one reward from `loot` and one from `bonus`. A rare win is announced because `rare` has
`order = 3`, at the `threshold`.

## 3. Load it

Run `/uxmcrates reload`. The console names any line it refused. `/uxmcrates doctor` counts the crates that loaded.

## 4. Place it

Look at a block and run `/uxmcrates place vote`, or take the link tool with `/uxmcrates tool vote` and right click
the block. The block becomes an ender chest with the hologram over it, and its position is written into
`positions` in the crate file.

## 5. Give a key and open it

```
/uxmcrates key give Steve vote-key 3
```

Steve right clicks the crate: the key is spent, the two rewards are decided, the reel turns and stops on them. A left
click on the crate opens the [preview](../menus/preview.md), with Steve's chance next to each reward.

## 6. Hook it to votes

A vote plugin runs a console command per vote. Point it at the key balance, which works for a player who is away:

```
uxmcrates key give %player% vote-key 1
```

## Next

- Add a [guarantee](../features/rewards.md#guarantees) so every 25th opening gives the diamond.
- Try another [reveal](../features/reveals.md): `animation = "helix"` draws a world reveal around the block.
- Show the keys on a scoreboard with `%uxmcrates_virtual_keys_vote-key%`.

Related: [Crate files](../config/crate-files.md), [Key files](../config/key-files.md)

---
title: Auto Kill
order: 33
icon: swords
---

Auto Kill kills the mobs you list and gives the farmer their drops. It turns an animal pen into a
production line, and it is the module most likely to need limiting.

```yaml
auto-kill:
  name: "Automatic Kill"
  status: true
  default-status: false
  permission-required: true
  only-kill-spawner-mobs: false
  mobs:
    - SHEEP
    - COW
    - IRON_GOLEM
```

| Option | What it does |
|---|---|
| `status` | Whether the module exists on this server |
| `default-status` | Whether a new farmer has it switched on |
| `permission-required` | Requires the owner to hold `uxmfarmer.autokill` |
| `only-kill-spawner-mobs` | Server-wide default for ignoring naturally bred mobs |
| `mobs` | Every entity type the module may kill |

## Which mobs

`mobs` is the whole list. Anything not in it is safe from the farmer no matter what a player does,
which makes this the one setting to get right before opening the module to players. Adding
`IRON_GOLEM` — as the default does — means iron farms work; adding a hostile mob means the farmer
does the fighting.

Players can narrow the list further in their own auto-kill settings menu, but never widen it.

## Spawner mobs only

`only-kill-spawner-mobs` sets the server default and `AUTO_KILL_SPAWNER` is the per-farmer version
of the same switch, which players toggle themselves. With it on, mobs that were bred or that spawned
naturally are left alone and only spawner output is killed.

This is how you separate two economies: spawners feed the farmer, breeding does not.

## Modifying the settings

Changing auto-kill settings needs `uxmfarmer.autokill.modify` in addition to `uxmfarmer.autokill`.
Splitting them lets you give a rank the module while keeping its mob list fixed at whatever you set.

## Player drops

Drops from mobs a *player* killed are governed separately by `collect-player-drops`:

```yaml
collect-player-dropped-items: true
collect-player-dropped-items-by-default: true
```

and the owner permission `uxmfarmer.collectplayerdrops`. Leave this on and a player fighting in
their own base feeds their farmer; turn it off and only the farmer's own kills count.

<Callout type="warning" title="Auto Kill and stacker plugins">

If you run a stacking plugin, a single kill can represent dozens of mobs. Check that its "kill the
whole stack" behaviour is what you expect before enabling this module — otherwise the farmer's
throughput is a multiple of what your prices were tuned for.

</Callout>

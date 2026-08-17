---
title: Auto Harvest
order: 32
icon: wheat
---

Auto Harvest is the module that does the farming. When a crop in the list reaches maturity the
farmer harvests it, replants it, and puts the yield into storage instead of on the ground.

```yaml
auto-harvest:
  name: "Automatic Harvest"
  status: true
  default-status: false
  permission-required: true
  required-piston: false
  extended-piston-check: false
  drop-item-if-stock-full: true
```

| Option | What it does |
|---|---|
| `status` | Whether the module exists on this server at all |
| `default-status` | Whether a new farmer has it switched on |
| `permission-required` | Requires the owner to hold `uxmfarmer.autoharvest` |
| `required-piston` | Crops in `piston-check-items` are only harvested if a piston is aimed at them |
| `extended-piston-check` | Changes how that piston search works — see below |
| `drop-item-if-stock-full` | Drops the yield on the ground when storage cannot take it |

## The crop list

`items` is everything the module will harvest:

```yaml
items:
  - WHEAT
  - COCOA
  - CACTUS
  - SUGAR_CANE
  - NETHER_WART
  - PUMPKIN
  - BAMBOO
  - SWEET_BERRY_BUSH
  - BEETROOTS
  - MELON
  - CARROTS
  - POTATOES
```

A material here still needs an entry in `collected-materials.yml` to have a price, a capacity and a
level ladder. Harvesting a product with no entry gives the player something they cannot sell.

## Piston requirement

`required-piston: true` makes the module refuse to harvest anything in `piston-check-items` unless
there is a piston next to it. This exists so players still have to *build* a farm rather than
placing a farmer next to a field and walking away.

The two search modes differ in where they look:

| | Where it searches | Does aim matter |
|---|---|---|
| `extended-piston-check: false` | Above, below, west, east, north and south of the crop | Yes — the piston must face the crop |
| `extended-piston-check: true` | Above, and the upper block west, east, north and south | No |

The extended mode is the forgiving one: it looks a block higher, which is where players usually put
pistons in a real harvester design, and it does not care which way they face.

## Growth and storage

`disabled-products-grow-physically` in `general-settings.yml` decides what happens to a product the
player has switched off. Left at `false`, those crops do not grow at all — the farmer simply skips
them and the server does no work. Setting it to `true` makes them grow as vanilla crops, which
looks better and costs real performance on a busy island world.

When storage is full, `farm-manager` takes over:

```yaml
farm-manager:
  disable-growing-if-stock-is-full: true
  remove-remaining-drops-from-ground: true
  remove-drops-if-stock-is-full: true
  drop-items-to-ground-if-not-enabled-or-full: false
```

The defaults stop growth at a full stock and clean up anything that reached the ground anyway. This
is the configuration that costs the least; turning on
`drop-items-to-ground-if-not-enabled-or-full` produces a floor covered in item entities, which is
exactly the problem this plugin exists to remove.

<Callout type="tip" title="Tell players when they are full">

`notify-when-stock-gets-full` in `general-settings.yml` messages the owner — or every active
member, with `notify-members: true` — when a product fills up. Without it the first sign a player
gets is that their farm has quietly stopped producing.

</Callout>

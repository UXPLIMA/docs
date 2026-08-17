---
title: Speed radar
order: 8
description: Roadside speed boards, fines, flashing lights and per radar limits.
icon: gauge
---

A radar is a roadside board that measures passing cars, shows their speed, fines whoever
breaks the limit and flashes its lights while it does.

## Building one

A radar is a `Model` under the world folder containing at least a detection part.

| Child | Named | Does |
|---|---|---|
| Detection zone | `Sensor` | A `BasePart`. Required |
| Speed board | `Speed` | Its first `TextLabel` shows the live speed |
| Warning lights | `SpeedingWarningLights` | Any `ImageLabel` inside flashes on a catch |
| Overrides | `Configuration` | Attributes that beat the global settings |

The sensor is a flat pad laid on the road. The detection volume is a column above it,
`SensorHeight` studs tall, matching the pad's footprint. A car counts as inside when its
driver's seat is in that column.

<Callout type="warning" title="The sensor name has to be unique to radars">

Every part named `Sensor` anywhere under the world folder is registered as a radar,
including one inside a car model that happens to be parked there.

Rename the setting, or make sure nothing else in your build uses that name.

</Callout>

## Settings

```lua
Radar = {
    Enabled = true,
    SampleInterval = 0.15,
    IdleSpeed = 8,
    SensorHeight = 14,
    MinSpeed = 0,
    MaxSpeed = 60,
    Fine = 250,
    Cooldown = 5,
    BannerDuration = 3.5,
    SensorPartName = "Sensor",
    BoardPartName = "Speed",
    LightsModelName = "SpeedingWarningLights",
    ConfigName = "Configuration",
    BoardEmptyText = "--",
},
```

| Field | Does |
|---|---|
| `SampleInterval` | Seconds between scans. Smaller catches faster cars and costs more |
| `IdleSpeed` | Below this the car counts as parked: blank board, no fine |
| `SensorHeight` | How far above the pad the column reaches |
| `MinSpeed` | A minimum. `0` disables it |
| `MaxSpeed` | The limit |
| `Fine` | Coins per catch |
| `Cooldown` | Seconds before the same radar can fine the same driver again |
| `BoardEmptyText` | Shown when nothing is passing |

Speeds use the same unit as the driving HUD: `DriveHud.Unit` decides whether the board and
the limits are in km/h or mph. Change the unit and every limit you have set changes
meaning, so set it before you tune anything.

<Callout type="info" title="A fine never puts a player in debt">

The charge is clamped to what they have. A driver with 40 coins pays 40 of a 250 fine, and
a driver with nothing pays nothing.

</Callout>

## Per radar overrides

Add a `Configuration` child to the radar model and set attributes on it. Anything present
wins over the global value.

| Attribute | Type |
|---|---|
| `Enabled` | Boolean |
| `MinSpeed` | Number |
| `MaxSpeed` | Number |
| `Fine` | Number |
| `Cooldown` | Number |

That is what makes a road network work: 30 through town, 60 on the ring road, a heavy fine
outside the school, and one radar switched off while you rebuild that junction.

## Minimum speed radars

`MinSpeed` above zero fines drivers who are **too slow**, as long as they are above
`IdleSpeed`. A motorway radar with `MinSpeed = 40` and `MaxSpeed = 90` punishes crawling and
speeding both.

The banner tells the driver which one they did and what the limit was.

## The board and the lights

| Element | Behaviour |
|---|---|
| Board | Shows the fastest qualifying car in the zone, or `BoardEmptyText` |
| Lights | Alternate every quarter second while somebody is over the limit |

The lights alternate rather than blink together: odd numbered `ImageLabel` instances swap
with even ones. Two lights give the classic police pattern, and one light gives a plain
blink.

## The banner

A caught driver gets a banner at the top of the screen with the fine, the speed and the
limit. It slides away after `BannerDuration`.

The banner is created by the client from the values the server sends, so a player who has
disabled their UI still pays the fine.

## Turning it off

`Radar.Enabled = false` skips registration entirely, so radars already built in the world
sit there inert. That is the switch to reach for if fines turn out to be more annoying than
fun, rather than deleting the models.

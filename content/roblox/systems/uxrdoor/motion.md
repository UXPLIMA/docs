---
title: Motion
order: 3
description: Swing, slide and double, easing, auto close, and the delay before opening.
icon: rotate-3d
---

`motion` is the only required field on a door. It says how the panels move from where you
built them.

Every panel's closed position is its position when the server binds the door, so the door
always returns exactly where you placed it.

## Swing

```lua
motion = { type = "swing", angle = 110, seconds = 2 },
```

Rotates the panel about an axis through its own origin.

| Field | Default | Meaning |
|---|---|---|
| `angle` | `90` | Degrees. A negative angle swings the other way |
| `axis` | `"Y"` | `"X"`, `"Y"` or `"Z"`, in the panel's local space |
| `seconds` | `1` | How long the movement takes |

<Callout type="warning" title="The hinge is the panel part's own centre">

The rotation is applied to the panel's `CFrame`, so a plain `Part` swings around its middle
rather than around an edge.

Build the leaf as a `MeshPart` or a union whose visible geometry sits to one side of the
part's centre, and put the part's centre on the hinge line. That is the usual way a hinged
door is built in Roblox, and it is what the shipped angles assume.

</Callout>

## Slide

```lua
motion = { type = "slide", move = Vector3.new(0, 0, 4), seconds = 2 },
```

Moves the panel by `move` in its **local** axes, so a rotated door still slides along its
own length.

| Field | Meaning |
|---|---|
| `move` | The offset in studs |
| `seconds` | How long the movement takes |

Vertical shutters are slides on Y. The shipped door `22` rises 6.5 studs, door `10` rises
15.45 studs over 8 seconds.

## Double

```lua
motion = {
    type = "double",
    seconds = 2,
    panels = {
        { type = "swing", angle = 100 },
        { type = "swing", angle = -100 },
    },
},
```

One entry per panel, applied in `PanelIndex` order. Each entry is its own swing or slide,
so a door can have one panel swing while the other slides.

`seconds` lives on the motion, not on the panels: both panels move over the same duration.

<Callout type="tip" title="Both panels can move the same way">

Mirrored panels are the usual case, so the two angles or offsets are usually opposites. They
do not have to be. The shipped door `29` slides both panels 3.5 studs in the same direction,
which is a barn or pocket door.

</Callout>

## More than two panels

`type = "double"` is a name, not a limit. The panel list is indexed against the sorted
panels, so a four-panel door works with four entries.

A panel with no matching entry stays put.

## Easing

```lua
motion = { type = "swing", angle = 110, seconds = 2, easing = "Sine" },
```

| Value | Feel |
|---|---|
| `Quad` | The default. A gentle ease out |
| `Linear` | Constant speed. Right for machinery and shutters |
| `Sine` | Softer than `Quad` |
| `Cubic` | Sharper |
| `Quart` | Sharpest |

All of them ease out, so a door decelerates into its open position. An unrecognised name
falls back to `Quad`.

## Auto close

```lua
autoCloseSeconds = 10,
```

The default. The door closes itself that many seconds after it finishes opening.

`0` means it stays open until somebody closes it.

The timer does not fire if the door has been locked down, is already closed, or is
mid-animation. A player who closes the door by hand before the timer expires simply gets a
timer that finds nothing to do.

Large doors ship with longer values: 20 seconds for the big shutters, 30 for the slowest.

## Delay before opening

```lua
delaySeconds = 2,
extraSound = true,
```

`delaySeconds` waits after access is granted before the panels start moving. The green light
and the granted sound happen immediately.

That gap is where `extraSound` lives: `Extra1` plays first, then the wait, then the door
moves. It is how a hydraulic or warning-klaxon door is built.

## While a door is moving

A door in motion is `animating`, and:

| | |
|---|---|
| Further requests | Refused with `Busy` |
| Zone open and close | Skip it |
| A lockdown | Waits for the movement to finish, then closes it |

The system waits for every panel's tween to complete before marking the door open or
closed, so a double door is never half-registered.

## Choosing durations

| Door | Suggested |
|---|---|
| An interior door | 0.5 to 1 second |
| A front door or gate | 2 seconds |
| A vault or hangar | 8 to 16 seconds, with a delay and an extra sound |

The shipped values are worth reading as a reference: they were tuned against real geometry
rather than picked for a config file.

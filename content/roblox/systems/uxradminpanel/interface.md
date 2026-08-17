---
title: The interface
order: 10
description: The pages, the hotkeys, the template pattern and the placeholder convention.
icon: layout-panel-left
---

The panel is `uxrAdminPanelGui`, a designed ScreenGui you edit in Studio. The code fills it
in; the layout and the styling are yours.

## Hotkeys

| Key | Does |
|---|---|
| `/` | Toggle the panel |
| `;` | Toggle the command bar |
| `Escape` | Close the top dialog |
| `Shift` and `,` | Toggle the command bar |
| `Shift` and `7` | Toggle the panel |
| `u!` in chat | Toggle the panel |
| `u!cmd` in chat | Toggle the command bar |

The backslash key also opens the command bar, and the numeric keypad divide also opens the
panel.

<Callout type="info" title="Hotkeys follow the keyboard layout">

The client asks Roblox which key produces `/` and which produces `;` on the player's actual
layout, and binds those, rather than binding the physical US positions.

An AZERTY or Turkish keyboard therefore gets the same two characters under the same two
fingers. The Shift combinations exist as a fallback for layouts where the lookup fails.

Both can be overridden per player, and the panel's Settings page is where that lives.

</Callout>

## The pages

| Page | Shows |
|---|---|
| Home | The dashboard |
| Players | Everybody in the server, with profiles and actions |
| Commands | The full command browser, filtered to what you may run |
| Logs | The action log |
| Analytics | Server statistics |
| Servers | Every public server, from the cross-server heartbeat |
| Chat | The admin chat, which crosses servers |
| Settings | Per-player preferences, including hotkeys |

Plus the command popup, which is the bar rather than a page.

Pages are gated by rank, and a page a player may not see is not built. See
[Ranks and permissions](ranks.md).

## The template pattern

Every list in the panel clones a prototype you design. The prototype lives in an unusual
place:

```
ScrollingFrame
  UIGridLayout
    TemplateFrame
```

The template is a child of the layout object rather than of the scrolling frame, which keeps
it out of the layout's own flow. Clones are parented to the scrolling frame.

<Callout type="warning" title="Do not move the template out of the UIGridLayout">

It looks misplaced in the explorer and it is not. Moving it up a level makes it appear as a
real row in every list.

</Callout>

## Placeholders

Every search box has a sibling label, named `SearchTextLabel` or `TypeLabel`, that is shown
and hidden as the box gains focus and text.

<Callout type="warning" title="Do not set PlaceholderText">

The system manages the sibling label instead, so setting Roblox's own placeholder property
gives you two placeholders at once.

The reason for the convention is styling: a sibling label can carry any font, colour,
stroke and animation, and `PlaceholderText` cannot.

</Callout>

## What the code fills in

| Element | Filled with |
|---|---|
| Rank badges | The rank's colour and display name |
| Command cards | Name, description, required rank, argument hints |
| Player rows | Name, rank, and the actions you may take |
| Log rows | The structured log fields |
| Notifications | Title, description, kind colour, and an optional click action |

Colours for the four notification kinds are fixed: blue for information, teal for success,
yellow for warning and red for error.

## The composer

A command run from the panel with a required argument missing opens a composer instead of
failing: a small form built from the command's own argument list, with a picker for colours,
assets and skyboxes where the type calls for one.

The punishment commands open a dedicated composer with tabs, which is what makes a ban with
a reason and a duration a two-field form rather than a line of syntax.

<Callout type="info" title="The composer is why the argument metadata matters">

Every `name`, `type`, `min`, `max` and `default` in `Commands.luau` is read by the composer
to build its form.

A command you add with careless metadata still runs from chat and gives a poor form in the
panel.

</Callout>

## Deploying your changes

Edit the copy inside
`ServerScriptService/uxrAdminPanelSystem/Client/uxrAdminPanelGui`. That is the design-time
copy.

<Callout type="warning" title="StarterGui is only seeded once">

The bootstrap copies the ScreenGui into `StarterGui` only if a ScreenGui of that name is not
already there. It never overwrites.

A stale copy in `StarterGui` from an earlier run silently wins over every change you make.
If your edits are not showing up, delete the `StarterGui` copy and press Play again.

</Callout>

## Missing instances

Every lookup is by name with a ten second wait, and a failure logs a warning naming the
exact path. The rest of the panel keeps working.

Watch Output while restyling and it tells you precisely what you renamed.

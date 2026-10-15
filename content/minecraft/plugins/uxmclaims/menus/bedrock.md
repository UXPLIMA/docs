---
title: Bedrock menus
order: 411
description: Native Bedrock forms in place of chests, anvils and chat prompts.
icon: smartphone
---

Players joining from Bedrock Edition get native forms instead of the Java screens, which are hard to use
on phones and consoles. Each chest menu becomes a list of buttons built from the items of its Java file,
so a re-skin of a Java menu carries over.

## Requirements

Geyser-Spigot or Floodgate must be installed on the same server as uxmClaims. With Geyser on a proxy,
install Floodgate on this server. The forms work on every server version uxmClaims supports. Without
either plugin, Bedrock players get the Java screens and nothing else changes.

At startup the console prints `Bedrock forms: enabled through Geyser/Floodgate.` when the forms are
active.

## Settings

In [config.yml](../config/config-yml.md):

| Key | Default | Meaning |
|---|---|---|
| `bedrock.enabled` | `true` | Forms for Bedrock players. `false` gives them the Java screens. |
| `bedrock.menus` | `true` | Also turn the chest menus into button lists. `false` keeps forms for typing and confirming, and chests for the rest. |
| `dialog.bedrockFallback` | `true` | Only used when forms are off or unavailable: Bedrock players get anvils, chat and chests instead of Java dialogs. |

## What each screen becomes

| Java screen | Bedrock form |
|---|---|
| A chest menu, with `bedrock.menus` on | Simple form: a title, text, and one button per item |
| Text input (rename, create a role or warp, invite, ban) | Custom form with a text box, always, whatever the input file's `type` |
| Warp creation | The same, plus the **Visible to everyone** switch |
| Confirmation, with its `dialog.enabled` on | Modal form with two buttons |
| Claim time, with its `dialog.enabled` on | Custom form with sliders, then a modal form with the price |
| The vault | Always the chest |

A custom form has a single submit button of Bedrock's own, so the `submit` and `cancel` labels of the
input files are not shown there. Closing the form cancels.

## How a chest becomes buttons

Every item of the Java menu file becomes a button, in slot order. Decoration under `otherItems` is left
out, and so is any item listed with `hidden: true`. The button text is the item's `name`, the image an
icon picked from its material.

Where the chest reacts to the click type, the Bedrock file lists the choices. Pressing such a button
opens a second form with one button per choice and a **Back** button. The shipped files do this for:

| Menu | Choices |
|---|---|
| `claim_main` | Teleport, or move the spawn here |
| `common_list`, `admin_claim_view` | Open, select, or clear the selection |
| `claim_member_list` | Manage, promote, or demote |
| `claim_role_list` | Manage, raise priority, or lower priority |
| `common_invite_inbox` | Accept or decline |
| `claim_map` | Claim or remove the chunk, or show its border |

When a button only sends a message, the form comes back half a second later, the way a chest would have
stayed open.

The chunk map lists only the chunks around its centre, a 3 by 3 area by default (`gridRadius`), plus
the arrows. The block style screen has one button per claim block style, all under the item key
`blockItem`. The flag, member and role editors add a second line with each entry's status.

## The files

`menu/bedrock/` holds one file per chest menu, with the same name: `claim_member_list.yml` belongs to
`menu/claim_member_list.yml`. There are 21, one for each chest menu except the vault, plus
`admin_claim_view.yml` for `/claim admin view`. The input screens have none; their form uses the
`dialog` section of their own file.

```yaml
enabled: true               # false: this menu stays a chest on Bedrock
title: ''                   # empty: the chest menu's title
content: [ ]                # lines above the buttons
back: '<gray>« Back</gray>' # the button that leaves a list of choices
detail: ''                  # a second line on buttons whose item has the detailFor placeholder
detailFor: 'status'
gridRadius: 1               # claim_map only: 1 is 3 by 3, 2 is 5 by 5

buttons:
  memberHead:               # the item's key in the Java file
    label: ''               # empty: the item's name
    image: 'https://mc-heads.net/avatar/%member.name%/64'
    hidden: false
    clicks:
      LEFT: '<white>Manage</white>'
      SHIFT_LEFT: '<green>Promote</green>'
      SHIFT_RIGHT: '<red>Demote</red>'
```

| Key | Meaning |
|---|---|
| `label` | Button text, MiniMessage. The item's own placeholders work. |
| `image` | A texture path of the Bedrock resource pack such as `textures/items/compass_item`, an `https` URL, or `none`. Placeholders work. |
| `hidden` | `true` leaves the item out. |
| `clicks` | The chest click each choice stands for (`LEFT`, `RIGHT`, `SHIFT_LEFT`, `SHIFT_RIGHT`) and its label. One entry runs that click directly; two or more ask which one. |

List only the items that should look different on Bedrock. Each file's header comment names the items
its menu has. Texts are MiniMessage, rendered as Bedrock colour codes, and are translated along with the
other files when you change `language:`. `/claim reload` rereads the files.

<Callout type="warning" title="Clicks follow the Java item keys">

A button finds its action through the item key in the Java menu file. Rename a key in `menu/`, and its
entry under `buttons:` stops applying. A choice does exactly what that click does on the chest, so
offer only clicks the Java item reacts to.

</Callout>

Related: [Menu layouts](../config/menus.md), [Overview](overview.md)

---
title: Bedrock Forms
order: 1110
description: 'Bedrock players — those joining a Java server through Geyser with Floodgate
  installed — do not see chest GUIs the way Java players do. A chest menu is clunky
  on a touch screen. So when uxmEssentials detects a Bedrock viewer, it renders the
  very same menu as a native Bedrock form instead: the clean, scrollable, tappable
  UI Bedrock players already know.'
---

This is automatic. You author one menu; Java players get the chest, Bedrock players get
the form. You only reach for the options below when you want to tune what the form looks
like.

---

## How a menu becomes a form

When a Bedrock viewer opens a menu, the engine picks a Cumulus form type to match its
shape:

| Form | Used for | Looks like |
|---|---|---|
| **SimpleForm** | A menu whose tiles are buttons (the common case) | A vertical list of tappable buttons, one per clickable item. |
| **ModalForm** | A two-choice menu | Two buttons (confirm / cancel). |
| **CustomForm** | A menu with an explicit `bedrock { }` block | A form built from dropdowns, sliders, toggles and text inputs. |

For a **SimpleForm**, the degradation is done for you: each item that binds at least one
click action becomes a button carrying that item's name. Decorative tiles — the filler
panes, blank borders, display-only info items — bind no action, so they are left off the
form (a button that does nothing on tap would be meaningless). Tap a button and its
action list runs exactly as a click would on Java.

<Callout type="info" title="Floodgate is what makes this happen">

The redirect only fires when Floodgate is present and reports the viewer as a Bedrock
player. On a Java-only server, Floodgate loads none of its classes and every player
takes the chest path. See [Bedrock (Floodgate)](../integrations/floodgate.md).

</Callout>

---

## When the chest is kept anyway: `chest-only`

Some menus can't be a form. A form is a flat list of buttons — it cannot display or edit
real item stacks. So a menu that shows an inventory (the vault, the inventory viewer, a
storage-style grid) stays a chest even for a Bedrock viewer.

The built-in storage menus already do this. For a custom menu that displays item stacks,
set the flag yourself:

```hocon
chest-only = true
```

With `chest-only`, a Bedrock viewer gets the chest GUI instead of a form. A menu that
uses `bottom-inventory` is chest-only automatically, since its raw-slot geometry only
makes sense in a chest.

---

## An explicit form: the `bedrock { }` block

The automatic SimpleForm is a button list — it can't offer a dropdown, a slider or a
text field. When you want those form-native controls, add a `bedrock { }` block. A
Bedrock viewer then gets that explicit **CustomForm**; a Java viewer never sees it and
keeps the chest.

```hocon
title = "<gold>Warp Creator"
rows = 3

# The chest tiles a Java player sees:
items {
  # … your normal item map …
}

# The native form a Bedrock player sees instead:
bedrock {
  title = "Create a Warp"
  content = "Fill in the details, then submit."
  widgets = [
    { type = input,    name = warp-name, label = "Warp name", placeholder = "spawn", default = "" }
    { type = dropdown, name = category,  label = "Category",  options = ["Public", "Staff", "VIP"], default = 0 }
    { type = slider,   name = cost,      label = "Cost",      min = 0, max = 1000, step = 50, default = 100 }
    { type = toggle,   name = locked,    label = "Locked?",   default = false }
    { type = label,    text = "You can edit this later with /warp editor." }
  ]
  on-submit = [
    "console:uxmwarp create %warp-name% %cost%",
    "message:<green>Created warp %warp-name%."
  ]
}
```

Each value widget carries a `name`. When the player submits, its value is bound to a
`%name%` placeholder — `%warp-name%`, `%cost%`, `%locked%` above — and the `on-submit`
action list runs with those values in scope.

### Widget types

| `type` | Keys | Value bound to `%name%` |
|---|---|---|
| `label` | `text` | (none — display only) |
| `input` | `name`, `label`, `placeholder`, `default` | The typed text |
| `dropdown` | `name`, `label`, `options`, `default` (index) | The selected option string |
| `slider` | `name`, `label`, `min`, `max`, `step`, `default` | The chosen number |
| `toggle` | `name`, `label`, `default` | `true` / `false` |

Widget labels, the form title and `content` are verbatim MiniMessage (or `@catalog.key`),
just like item names — they may hold placeholders and are resolved per open.

---

## Graceful fallback

Everything degrades to something sensible:

- **No Floodgate** → the form path never engages; everyone gets the chest.
- **`chest-only`** → this menu is a chest for Bedrock viewers too.
- **No `bedrock { }` block** → the automatic SimpleForm/ModalForm degradation is used.
- **A `bedrock { }` block** → Bedrock viewers get that CustomForm; Java viewers ignore it.

You never end up with a broken menu — the worst case is a Bedrock player seeing the same
chest a Java player would.

---

## Next Steps

- [Bedrock (Floodgate)](../integrations/floodgate.md) — enabling Floodgate detection.
- [Custom Menu Engine](engine.md) — the `chest-only` and `bedrock` keys in context.
- [Actions & Requirements](actions-requirements.md) — what `on-submit` actions can do.

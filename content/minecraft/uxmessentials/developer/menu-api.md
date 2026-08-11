---
title: Menu API
order: 1660
description: 'The menu engine behind every GUI in uxmEssentials is extensible: teach
  it your own actions, requirements, placeholders, list sources and icons, and operators
  can write your ids in their own menu files.'
---

The [menu engine](../menus/engine.md) behind every GUI in uxmEssentials is extensible. Through `MenuApi` you teach it
your own actions, requirements, placeholders, list sources and icons, and operators can then write your ids in their
own `menus/*.conf` files exactly like the built-in vocabulary.

## Getting it

`MenuApi` comes from the front door:

```java
import com.uxplima.uxmessentials.api.bukkit.UxmEssentialsApi;
import com.uxplima.uxmessentials.api.bukkit.menu.MenuApi;

@Override
public void onEnable() {
    UxmEssentialsApi.whenReady(api -> registerMenuExtensions(api.menus()));
}
```

`whenReady` rather than a service lookup, because it runs again after uxmEssentials reloads: a reload rebuilds the
engine's registries, and a plugin that registered once and never again would be silently gone with no way to notice.

Add the dependency first, see [Adding the dependency](dependency.md).

## What you can register

| Method | Registers | Written in a menu spec as |
|---|---|---|
| `registerAction(String, Consumer<MenuClick>)` | a click, open or close action | `click = ["my-award"]` or `my-award:<value>` |
| `registerRequirement(String, BiPredicate<MenuView, Map<String, String>>)` | a `view` or `click` condition | `my-cond:<value>`, prefix `!` to invert |
| `registerPlaceholder(String, Function<MenuView, String>)` | a text token | `%my-token%` in a title, name or lore |
| `registerListSource(String, Function<MenuView, List<?>>)` | a source for paginated items | `list { source = my-src }` |
| `registerIconProvider(MenuIconProvider)` | a `material` prefix resolver | `material = "myplugin:some-id"` |
| `buildItem(String, String, List<String>, Player)` | nothing: builds an `ItemStack` from a spec | not applicable |

A custom button is just a menu item whose click runs a registered action, so `registerAction` covers buttons too.

`buildItem` works the other way round from the rest: instead of teaching the engine something, it lets your own code
produce an icon identical to the one the engine would render from the same spec, so a hand-built inventory stays
visually consistent with the menus.

## Example

```java
private void registerMenuExtensions(MenuApi menus) {
    // An action operators can call as "myaddon-reward:<amount>".
    menus.registerAction("myaddon-reward", click -> {
        int amount = Integer.parseInt(click.arg());
        grantReward(click.player(), amount);
    });

    // A requirement, usable as "myaddon-vip" or "!myaddon-vip" to invert.
    menus.registerRequirement("myaddon-vip", (view, args) -> isVip(view.viewerId()));

    // A placeholder, usable as %myaddon_rank% in any title, name or lore.
    menus.registerPlaceholder("myaddon_rank", view -> rankOf(view.viewerId()));
}
```

`MenuClick` gives you `player()`, `view()`, `kind()` (which mouse button), `arg()` for the single value form and
`args()` for the rest. `MenuView` gives you the viewer, the page, the arguments the menu was opened with, and
`entry(Class)` to read back the element a list source produced for the clicked row.

## Two rules

<Callout type="warning" title="Register each id exactly once">

A duplicate id throws `IllegalStateException`, on purpose: a wiring mistake should fail loudly rather than let a
second handler quietly win. Register inside `whenReady` and nowhere else.

</Callout>

<Callout type="warning" title="A menu is validated when it loads">

A spec that names an id the engine does not know is a loud load-time failure, not a broken menu a player
discovers. That is the right behaviour, but it means a menu already loaded when your plugin enables has to be
re-validated: run `/uxmess reload` or `/menu reload` once your registrations are in place.

</Callout>

## Threading

Action, requirement and placeholder handlers run on the viewer's own region thread, which on Folia is that player's
region rather than one main thread. You can act on the viewing player inline; anything slower belongs on a scheduler.

**List sources are different.** They are queried off the tick thread so a slow query never stalls a region, and they
must not touch the Bukkit API at all. Return your data and read it back in a placeholder through `view.entry(...)`.

## Next steps

- [Custom menu engine](../menus/engine.md) for the spec format your extensions sit alongside
- [Overview](overview.md) for the front door
- [Events](events.md) for observing rather than extending

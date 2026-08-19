---
title: Holograms
order: 262
description: Holograms on native Display entities, with appearance, lifecycle management, distance-driven visibility, following and animation.
icon: sparkle
---

Built on 1.21 `Display` entities: text, item and block. No packets, no per-version NMS, so they keep
working across point releases.

## Spawning one

```java
HologramManager holograms = new HologramManager();
holograms.installLifecycleListener(plugin);

Hologram spawn = holograms.spawn(
        Holograms.builder()
                .line(Text.mini("<yellow><bold>Spawn"))
                .line(Text.mini("<gray>Welcome to the server"))
                .billboard(Display.Billboard.CENTER)
                .glow(Color.YELLOW),
        location);
```

The manager tracks what you spawned so it can be cleaned up, and the lifecycle listener resets
per-player state on quit, world change and respawn, which is the source of most "ghost hologram"
reports in other implementations.

## Appearance

Everything on the builder, and on the `Appearance` record it produces:

| Method | Controls |
|---|---|
| `billboard(Billboard)` | How it faces the viewer |
| `seeThrough(boolean)` | Whether it renders through blocks |
| `glow(Color)` | Outline colour |
| `background(Color)` | Text background |
| `textOpacity(byte)` | Text alpha |
| `lineWidth(int)` | Wrap width |
| `textShadow(boolean)` | Drop shadow |
| `viewRange(float)` | Client render distance multiplier |
| `brightness(Brightness)` | Fixed block and sky light |
| `alignment(TextAlignment)` | Left, centre or right |
| `shadowRadius` / `shadowStrength` | The ground shadow |
| `scale(f)` / `scale(x, y, z)` | Size |
| `translation(x, y, z)` | Offset from the anchor |
| `rotation(degrees)` / `rotation(yaw, pitch)` | Orientation |
| `transform(Transform)` | All of scale, translation and rotation at once |

```java
Transform transform = Transform.scale(1.5f).withTranslation(0f, 0.3f, 0f).withYaw(45f);
```

`Appearance` is a record with `with...` copies, so a shared base appearance can be varied per
hologram without mutation.

## Item and block holograms

```java
ItemHologram icon = Holograms.item(sword).scale(1.2f).spawnAt(location);
BlockHologram block = Holograms.block(Material.DIAMOND_BLOCK.createBlockData()).spawnAt(location);

icon.setItem(newSword);
block.setBlock(newData);
```

Shortcuts for the display-only case:

```java
Holograms.spawnItem(location, sword);
Holograms.spawnBlock(location, blockData);
Holograms.spawnPlayerHead(location, uuid);
Holograms.spawnPlayerHeadTexture(location, base64);
Holograms.spawnPlayerHeadSkinUrl(location, url);
```

`MojangSkinResolver` resolves a name to a texture asynchronously when you need one you do not have:

```java
new MojangSkinResolver().resolveTexture("Notch")
        .thenAccept(texture -> texture.ifPresent(t -> Holograms.spawnPlayerHeadTexture(loc, t)));
```

## Mixed holograms

Text, item and block lines stacked in one hologram.

```java
MixedHologram board = Holograms.spawnMixed(
        MixedHologramSpec.builder()
                .text(Text.mini("<gold>Top player"))
                .item(trophy)
                .text(Text.mini("<gray>Notch"))
                .lineGap(0.3)
                .build(),
        location);
```

## Controlling a hologram

```java
hologram.setText(Text.mini("<green>Updated"));
hologram.moveTo(newLocation, 20);       // interpolated over 20 ticks
hologram.setTransform(transform);
hologram.attachTo(entity);
hologram.remove();
```

```java
hologram.restrictToViewers();           // hidden from everyone until shown explicitly
hologram.show(plugin, viewer);
hologram.hide(plugin, viewer);
hologram.isVisibleTo(viewer);
hologram.forgetViewer(uuid);
```

`restrictToViewers` is what turns a world-visible hologram into a per-player one.

## Distance-driven visibility

Doing the show and hide bookkeeping yourself is where per-player holograms usually go wrong. The pool
does it.

```java
HologramPool pool = new HologramPool(plugin, scheduler);

pool.register(hologram, 32.0);                   // visible within 32 blocks
pool.register(hologram, customGate);             // your own VisibilityGate
pool.unregister(hologram);
```

On a repeating task the pool snapshots online players in each hologram's world, asks the gate which
should see it, diffs that against the set it last showed, and calls show or hide only on the
transition.

The visibility pass runs on the global region, where reading locations is safe. Every show and hide
is then bounced onto the hologram entity's own region thread. That bounce is the difference between
this and the single-async-loop pattern it is modelled on, which is not Folia-safe.

Registering starts the timer if it was idle, and the timer stops once nothing is registered, so an
empty pool costs nothing.

```java
LineOfSight.rayTrace();   // a gate predicate that also requires clear line of sight
```

## Clicking a hologram

```java
HologramInteractions interactions = new HologramInteractions(plugin);
interactions.install();

ClickableHologram clickable = interactions.clickable(
        HologramSpec.of(List.of(Text.mini("<gold>Click me"))),
        location,
        1.5f,
        0.6f,
        click -> {
            if (click.type() == HologramClick.Type.RIGHT) openMenu(click.player());
        });

clickable.text();
clickable.resize(2f, 1f);
clickable.remove();
interactions.forget(clickable);
```

An invisible `Interaction` entity sized to the text, since a `Display` entity is not clickable on its
own.

## Following an entity

```java
new HologramFollow(scheduler).follow(hologram, player, new Vector(0, 2.4, 0), Duration.ofMillis(100));
```

Returns a `TaskHandle` you cancel when the hologram or the entity goes away.

`SelfHiddenNameplate` is the specific case of a nameplate above a player that the wearer themselves
should not see:

```java
SelfHiddenNameplate plate = new SelfHiddenNameplate(hologram, wearer);
plate.refreshViewers(plugin, Bukkit.getOnlinePlayers());
```

## Animation

```java
HologramAnimation animation = new HologramAnimation(scheduler);

animation.animate(hologram, TextAnimation.typewriter("Welcome!"), Duration.ofMillis(80));
animation.animate(hologram, TextAnimation.scroll("A very long message", 20), Duration.ofMillis(120));
animation.animate(hologram, TextAnimation.frames(myFrames), Duration.ofMillis(250));
```

## Per-viewer text

```java
PerViewerHologram personal = Holograms.perViewer(location, Appearance.DEFAULT);

personal.setText(viewer -> Text.mini("<gray>Hello, " + viewer.getName()));
personal.update(plugin, viewer);
personal.updateAll();
personal.removeViewer(viewer);
personal.viewerCount();
```

One entity, a different text per viewer. This is a text-override technique, not a packet layer, so it
stays on the public API.

## Observing show and hide

```java
ObservableHologram observed = ObservableHologram.wrapping(hologram)
        .onShow(viewer -> log("shown to " + viewer.getName()))
        .onHide(viewer -> log("hidden from " + viewer.getName()));
```

A decorator, so it works with anything that is a `Hologram`.

## Holograms from config

```hocon
lines = [ "<gold>Spawn", "<gray>Welcome!" ]
appearance {
  billboard = CENTER
  glow = "#ff5555"
  scale = 1.5
}
```

```java
HologramSpec spec = HologramConfig.load(node);

spec.lines();
spec.appearance();
spec.asText();

HologramConfig.save(spec, node);
```

This one is two-way: `save` writes a spec back out, so an in-game editor can persist what an admin
positioned.

A `HologramSpec` is pure, with no world and no entity, so it can be assembled and asserted without a
running server. `HologramInteractions.clickable` takes one directly. To spawn a plain hologram from a
loaded spec, feed its lines and appearance fields into `Holograms.builder()`, which is what
`Holograms.Builder.spec()` produces in the other direction.

## Cleanup

```java
holograms.count();
holograms.remove(hologram);
holograms.removeAll();                 // in onDisable
int orphans = holograms.sweepOrphans(world);
holograms.invalidateViewer(uuid);
```

`sweepOrphans` removes display entities tagged as this plugin's holograms that the manager no longer
tracks, which is how you recover from a crash that left entities in the world.

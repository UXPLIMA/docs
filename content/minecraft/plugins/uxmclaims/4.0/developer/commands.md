---
title: Commands
order: 184
description: The command annotations of the SDK, argument types, suggestions, aliases and commands.yml labels.
icon: square-terminal
---

A module declares its commands as annotated methods and hands the object to `context.commands()`.
uxmClaims builds one command tree from every module, registers it once all modules have started, and
takes a module's commands out again when the module stops. The annotations are in
`com.uxplima.uxmclaims.sdk.command`.

## Example

```java
import com.uxplima.uxmclaims.sdk.command.Command;
import com.uxplima.uxmclaims.sdk.command.CommandPermission;
import com.uxplima.uxmclaims.sdk.command.CommandPlaceholder;
import com.uxplima.uxmclaims.sdk.command.Description;
import com.uxplima.uxmclaims.sdk.command.Greedy;
import com.uxplima.uxmclaims.sdk.command.Optional;
import com.uxplima.uxmclaims.sdk.command.Single;
import com.uxplima.uxmclaims.sdk.command.Subcommand;
import com.uxplima.uxmclaims.sdk.command.Suggest;
import com.uxplima.uxmclaims.sdk.module.ModuleContext;
import org.bukkit.command.CommandSender;
import org.bukkit.entity.Player;

import java.util.Map;

@Command("shop")
@CommandPermission("uxmclaims.command.example-shops.open")
public final class ShopCommands {

    private final ModuleContext ctx;

    public ShopCommands(ModuleContext ctx) {
        this.ctx = ctx;
    }

    @CommandPlaceholder
    @Description("Open the shop menu")
    public void open(Player sender) {
        ctx.menus().open(sender, "main", Map.of());
    }

    @Subcommand("give")
    @Description("Give a player shop credit")
    @CommandPermission("uxmclaims.admin.example-shops")
    public void give(CommandSender sender, Player target, int amount, @Optional @Single String note) {
        sender.sendMessage("Gave " + amount + " to " + target.getName() + (note == null ? "" : " (" + note + ")"));
    }

    @Subcommand("open")
    @Description("Open the shop of a land")
    public void openOf(Player sender, @Greedy @Suggest(source = Suggest.LANDS) String land, @Optional @Single String page) {
        ctx.menus().open(sender, "main", Map.of("land", land, "page", page == null ? "1" : page));
    }
}
```

Register it in `onEnable` with `ctx.commands().register(new ShopCommands(ctx))`. This gives `/shop`,
`/shop give <player> <amount> [note]` and `/shop open <land name...> [page]`.

## Annotations

| Annotation | On | What it does |
|---|---|---|
| `@Command("a b c")` | Method | The full path of the command; several values give several paths for one method. A path starting with another module's root (`"claim shop"`) adds a subcommand under that root |
| `@Command("root")` | Class | The roots that the class's `@Subcommand` and `@CommandPlaceholder` methods hang under |
| `@Subcommand("a b")` | Method | A path below the class's roots: `@Subcommand("reload")` in `@Command("uxm")` is `/uxm reload` |
| `@CommandPlaceholder` | Method | Runs for the class's root itself when no subcommand follows (`/shop`) |
| `@Description("...")` | Method | The English line in the help list; a `description` in `commands.yml` replaces it |
| `@CommandPermission("node")` | Method or class | The node a sender needs; on a class it covers every method that names none. Commands a sender may not use are hidden from tab completion, the help list and their command list |
| `@SecretCommand` | Method or class | The command works but is never suggested nor listed in the help |
| `@Optional` | Parameter | May be left out; the method gets `null`, `0` or `false` |
| `@Single` | Parameter | A last `String` that reads one word (or one `"quoted text"`) instead of the rest of the line |
| `@Greedy` | Parameter | A `String` with spaces that is followed by more arguments, such as a land name |
| `@Suggest(value, source)` | Parameter | Tab completion: fixed words, a named source, or both |

Import `com.uxplima.uxmclaims.sdk.command.Optional`, not `java.util.Optional`, on a class that uses both.

A word in angle brackets inside a path, `@Command("uxm bank <holder> balance")`, is an argument in the
middle of the path. It is passed to the method's first parameters after the sender; literal words win over it.

## Parameters

The first parameter is the sender: `CommandSender` (anyone), `Player` (players only) or
`ConsoleCommandSender` (console only). The rest are read in order:

| Type | Reads |
|---|---|
| `String` | One word or a `"quoted text"`; the last `String` reads the rest of the line unless it is `@Single` |
| `int`, `long`, `double` and the other numbers | A number |
| `boolean` | `true` or `false` |
| `Duration` | `30m`, `1d12h`, `2w`; a bare number is days |
| `Player` | An online player by name; `me` is the sender |
| `World` | A loaded world |
| An enum | One of its constants |

A `@Greedy` name is resolved this way: a `"quoted name"` is taken as written; otherwise, with suggestions, the
longest run of leading words that is one of them; otherwise every word but one per following argument. An
unknown name followed by a rest-of-line argument therefore needs quotes. `CommandText` has the same splitting
helpers (`peel`, `peelLast`, `knownPrefix`) for a module that reads a raw line itself.

## Suggestions

Without `@Suggest`, a parameter gets the suggestions of its type, and a `String` named `player`, `land`, `town`,
`nation` or `world` gets that source.

| Source constant | Value | Suggests |
|---|---|---|
| `Suggest.PLAYERS` | `players` | Online players the sender can see |
| `Suggest.WORLDS` | `worlds` | Loaded worlds |
| `Suggest.LANDS`, `Suggest.TOWNS`, `Suggest.NATIONS` | `lands`, `towns`, `nations` | Names |
| `Suggest.HOLDERS` | `holders` | Land, town or nation names, chosen by a `land`, `town` or `nation` argument just before |

Add your own with `ctx.commands().suggestions("shop-items", context -> List.of("sword", "bow"))`.
The kernel keeps the values that start with what was typed, ignoring case, so return everything you know.
The source runs on the tab-completion thread: read memory only. `SuggestionContext` gives the sender, the
arguments before this one, `previous()` and the typed `input()`. A name another owner already has is refused.

## Refusals and errors

When a command method throws `ActionException`, the player sees the text of its message key, looked up
in your module's messages first and the kernel's after; without either, the exception's own message. Any other exception is reported through the error
reporter: the player gets a short id, the console the stack trace. A failed future is yours to handle; see
[The API](api.md).

## Aliases

`ctx.commands().alias("sh", "shop")` adds a new root label: `/sh give Alex 5` runs `/shop give Alex 5`. For a
target that changes, pass a `CommandAlias` instead: it returns the command line for the typed arguments on
every use, or `null` when it leads nowhere (then `unavailable(...)` tells the sender why). Register aliases in
`onEnable` or `onAllEnabled`; a label an uxmClaims command or another plugin already has is skipped with a
warning. A label is one lower-case word of letters, digits, `_` and `-`.

## commands.yml labels

Server owners give commands local names in `plugins/uxmClaims/modules/<id>/lang/<code>/commands.yml`, in the
server's default language. Ship the file in your jar at `uxm/modules/<id>/lang/en/commands.yml` with empty
labels:

```yaml
config-version: 1
shop:
  label: ""
  aliases: []
  children:
    give:
      label: ""
    open:
      label: ""
      description: ""
```

| Key | What it does |
|---|---|
| `label` | A local name next to the English one; the English name always keeps working |
| `aliases` | More names |
| `children` | The words below, keyed by their English name |
| `usage`, `description` | Replace the generated usage and the `@Description` line in the help list |

The file is read once before commands are registered; changes need a restart. See
[Languages](../configuration/languages.md) for how server owners edit it.

Related: [Writing a module](sdk.md), [Menus](menus.md), [Aliases](../modules/aliases.md)

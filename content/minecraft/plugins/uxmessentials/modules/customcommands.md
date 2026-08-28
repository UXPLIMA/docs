---
title: Custom Commands
order: 9025
description: 'Your own commands, declared in a file and running the actions the menu engine already speaks.'
icon: terminal
---

A custom command is a HOCON file in `plugins/uxmEssentials/commands/custom/`. It declares a command word, who may
run it, what arguments it takes and what it does, and the plugin registers it as a real command: tab completion,
typed arguments, a usage line, and a place in `commands.conf` beside the built-in ones. The steps it runs are the
same actions a menu button runs, so nothing new has to be learned to write one.

Module `customcommands` · enabled by default · `modules/customcommands/config.conf`

## Commands

{/* generated:commands */}
| Command | What it does | Permission |
|---|---|---|
| `/customcmd` (`/customcommand`) | Manage operator-defined custom commands | `uxmessentials.customcommand.admin` |
{/* /generated */}

## Permissions

{/* generated:permissions */}
| Node | Default | Grants |
|---|---|---|
| `uxmessentials.customcommand.admin` | op | /customcmd to list, inspect, reload, test and run operator-defined commands. |
| `uxmessentials.customcommand.cooldown.bypass` | op | Run an operator-defined command without waiting out its cooldown. |
| `uxmessentials.customcommand.cost.bypass` | op | Run an operator-defined command that costs money without paying for it. |
| `uxmessentials.customcommand.run.others` | op | /customcmd run \<id> \<player>: run an operator-defined command for somebody else. |
| `uxmessentials.module.customcommands` | op | Hot-reload / inspect the customcommands module (operator-defined commands behind /customcmd). |
{/* /generated */}

## Settings

{/* generated:settings */}
| Key | Default | What it does |
|---|---|---|
| `allow-console-actions` | `true` | Whether a definition may run a command from the CONSOLE (the `console:` action head). A console command runs with full server permissions no matter who typed the custom command, which is exactly what you want for a reward that calls `give`, and exactly what you do not want in a file you have not read. It is ON here because a custom command is written by an operator rather than clicked by a player, but turn it off if more than one person can drop files into the commands/custom/ folder. When false, a definition using `console:` still loads and its other steps still run; the console step is skipped and logged with the command it would have run, so the gap is visible instead of silent. |
| `allow-op-actions` | `false` | Whether a definition may run a step with operator rights temporarily granted to the player (the `command-as-op:` action head). This is the sharpest tool in the file: it makes the PLAYER momentarily an operator for one command, so a mistake hands out real power rather than merely running something as the server. It is OFF by default and should stay off unless you have a command that cannot be expressed with `console:` instead. |
| `max-chain-depth` | `5` | How deep custom commands may call each other. A custom command whose action runs another custom command is useful (a menu of shortcuts, a command that shares a reward step), and a command that calls itself is a server freeze. The depth counter stops the chain here and tells the player the command is misconfigured rather than hanging the tick. |
| `max-delay` | `"60s"` | The longest a single `delay:` step may wait before the rest of the chain continues. A definition asking for more is clamped to this value at load with a warning, so a typo of "10m" where "10s" was meant cannot park a reward an operator is waiting on for ten minutes. |
| `max-delayed-steps` | `20` | How many delayed steps one chain may schedule in total. Every delay costs a scheduled task, so this bounds what a single run can leave behind on the scheduler; steps past the limit are dropped at load with a warning. |
| `log-privileged-actions` | `true` | Whether a console or op step writes an audit line to the server log naming the command, who ran it, and what it executed. This is how a privileged step stays reviewable after the fact; leave it on unless the log volume is a genuine problem. |
| `default-currency` | `"vault"` | The currency a `cost` uses when a definition names none of its own. Same short specs the rest of the plugin takes: `vault` (the server economy), `exp` (native experience points), `playerpoints`, `coinsengine` / `coinsengine:<name>`, and `zessentials` / `zessentials:<name>`. A back-end whose plugin is absent charges nothing, so a priced command still runs rather than becoming unusable when the economy is down. |
{/* /generated */}

## Writing a definition

The file name is the command's id, which is what `/customcmd info`, `/customcmd test` and `/customcmd reload` take.
The `name` inside the file is the word players type. A shipped `commands/custom/example.conf` walks through every
key; the short version:

```hocon
command {
  name = "welcome"
  aliases = ["hello"]
  permission = "uxmessentials.customcommand.example"
  deny-message = "<red>You are not allowed to welcome people."
  console = true
  description = "Greet a player"
  usage = "/welcome [name]"
  cooldown = "5s"
  warmup = "0s"
  cost = 0
}

arguments = [
  { name = "name", type = string, optional = true }
]

actions = [
  "message:<green>Welcome message sent."
  "delay:1s"
  "broadcast:<gold>%player% <yellow>welcomes <gold>%arg_name%<yellow> to the server!"
]
```

Argument types are `string`, `rest` (the whole remaining line), `int`, `double`, `bool`, `material`, `world`,
`online-player` and `player`. An `int` or `double` may carry `min` and `max`, which turns an out-of-range value
into a syntax error before a single action runs. An optional argument must come after every required one, and an
omitted one reaches the actions as an empty value.

Inside an action, `%arg_<name>%` is one parsed argument and `%args%` is everything the player typed after the
command word. Every placeholder and every action the menu engine knows works here too.

To rename an existing command instead, skip the `actions` list and write one line:

```hocon
alias = "/gamemode creative"
```

Everything typed after your command is passed through. A file declares `actions` or `alias`, never both.

## Gates

A run passes through the gates in a fixed order, and the first one to close is the one the player hears about:
sender kind (`console`), the declared `permission`, chain depth, the `requirements` list, the cooldown, the warmup,
then the cost. The cost is charged after the warmup completes, so a warmup a player walks out of costs nothing and
there is no refund to get wrong. The cooldown is stamped when the chain starts, so a run refused at any gate leaves
the clock alone.

`requirements` takes the same conditions a menu item takes (`has-money:100`, `perm:some.node`, `world:nether`, and
the rest), and `requirement-deny` is the chain that runs when one of them fails, which is how an else branch is
written.

## Managing definitions in game

`/customcmd` is the operator surface, and every subcommand takes the file name as the id:

| Subcommand | What it does |
|---|---|
| `/customcmd list` | Every loaded definition, its command word and whether it parsed cleanly |
| `/customcmd info <id>` | One definition in full: gates, arguments and the action chain |
| `/customcmd test <id>` | Re-read that one file and report what it would load, without applying it |
| `/customcmd reload [id]` | Re-read the whole folder, or one file |
| `/customcmd run <id> [player]` | Run a definition now, which is how a brand new one is tried before the restart |
| `/customcmd create <id>` | Write a definition by answering questions in chat |
| `/customcmd delete <id> confirm` | Delete a definition's file and re-read the folder |

`create` asks for the command word, its aliases, the permission node, whether the console may run it, then each
argument and each action in turn, and shows the finished file before it writes anything. `done` ends a list,
`none` leaves a key out, `keep` accepts the suggestion, and closing the prompt at any point cancels the whole run
without writing a file. The definition it saves is loaded straight away, so `/customcmd run` and `/customcmd info`
answer for it immediately; its own command word still waits for the next restart.

## Notes

- **A bad definition never takes the others down.** A file that fails to parse is skipped with a console warning
  naming it, and everything else still loads.
- **The first claim of a command word keeps it.** A definition whose word another command already took is dropped
  with a warning; a merely colliding alias is dropped and the command keeps the rest.
- **A reload refreshes a definition, not the command list.** Commands register while the server starts, so
  `/customcmd reload` picks up new gates, requirements and actions immediately, but a brand new command word waits
  for the next restart. `/customcmd run <id>` runs a freshly written definition in the meantime.
- **Console and op steps are opt-in and audited.** `allow-console-actions` and `allow-op-actions` gate the two
  privileged heads; a chain using a disabled one skips that step and says so, and with `log-privileged-actions` on,
  every privileged step records who triggered it.
- **A console run is a partial run.** A command with `console = true` carries out the steps that mean something
  without a player (`console:`, `broadcast:`, `message:`) and names the ones it had to skip.

Related: [Custom Menus](custommenus.md), [Actions and requirements](../menus/actions-requirements.md), [commands.conf](../config/commands-conf.md)

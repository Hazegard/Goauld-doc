---
title: TUI
description: Goauld client TUI
weight: 2
---

The client provides a text-based user interface (TUI) to monitor and manage connected agents.

![Goauld TUI](TUI.png)

## TUI keybinds

|        Key        | Action                                          |
|:------------------:| ----------------------------------------------- |
| `[ctrl+r]`  | Reset the agent (press twice to confirm)        |
| `[ctrl+k]`  | Stop the agent (press twice to confirm)         |
| `[ctrl+d]`  | Stop the agent and attempt to delete the binary (press twice to confirm) |
|  `[Enter]`  | Start an SSH session                            |
| `[ctrl+e]`  | Launch VS Code (press twice to confirm)         |
|    `[+]`    | Toggle agent details                            |
|    `[r]`    | Refresh the agent list                          |
| `[↑]`/`[↓]`/`[←]`/`[→]` | Navigate the agent list             |
| `[q]`/`[ctrl+c]` | Quit the TUI                               |

> [!NOTE]
> `ctrl+r`, `ctrl+k`, `ctrl+d`, and `ctrl+e` require the same key to be pressed twice in a row to confirm the action. The first press shows a confirmation prompt; any other key in between cancels it.

> [!NOTE]
> If the selected agent has a static password configured that isn't already cached, confirming the action switches the TUI into a password prompt instead of running immediately; the action completes once the password is entered.



Equivalent functionality for the kill, reset and delete actions is also available as standalone commands:

```bash
tealc (kill|reset|delete) <AGENT_NAME>
```

The `kill` command also accepts a `--delete` flag to delete the agent's binary after termination, equivalent to the TUI's `ctrl+d` action:

```bash
tealc kill <AGENT_NAME> --delete
```

> [!NOTE]
> When SSH ControlMaster is enabled (see [client/controlmaster]({{< ref "04-client/10-controlmaster" >}})), both the TUI actions and the standalone `tealc kill`/`tealc reset` commands reuse an existing master connection if available, avoiding the password prompt. If no master connection exists, a new connection is automatically created, which will also establish the ControlMaster socket for future use.
> 
> `tealc delete` is unaffected by ControlMaster since it uses a separate HTTP-based API path instead.

All known agents can be listed with:

```bash
tealc list
```


### Extended TUI

Some information is hidden by default. Press `+` to toggle the details.
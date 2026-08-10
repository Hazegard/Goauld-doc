---
title: Tui
description:
weight: 2
---

The client provides a text-based user interface (TUI) to monitor and manage connected agents.

![Goauld TUI](TUI.png)

## TUI keybinds

|     Key     | Action                                          |
|:-----------:| ----------------------------------------------- |
| `[ctrl+r]`  | Reset the agent                                 |
| `[ctrl+k]`  | Stop the agent                                  |
| `[ctrl+d]`  | Stop the agent and attempt to delete the binary |
|  `[Enter]`  | Start an SSH session                            |
| `[ctrl+e]`  | Launch VSCode                                   |
|    `[+]`    | Toggle agent details                            |



The kill, reset and delete actions are also available as standalone commands:

```bash
tealc (kill|reset|delete) [AGENT_NAME]
```


### Extended TUI

Some information is hidden by default. Press `+` to toggle the details.
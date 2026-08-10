---
title: Configuration file
description: Goauld configuration file lookup
weight: 4
---

The server, agent, and client all look for a configuration file in the current directory, then fall back to the `$HOME/.config` directory. If a configuration key is set in both, the value from the current directory takes precedence.

The configuration filenames for each component are:

|        Server         |                Agent                |    Client    |
|:---------------------:|:-----------------------------------:|:------------:|
| `goauld_server.yaml`  | `goauld_agent.yaml` / `goauld.yaml` | `tealc.yaml` |

> [!NOTE]
> The agent checks both filenames in each directory, for up to 4 files total. From highest to lowest precedence:
> 1. `./goauld_agent.yaml`
> 2. `./goauld.yaml`
> 3. `$HOME/.config/goauld_agent.yaml`
> 4. `$HOME/.config/goauld.yaml`
>
> For a given key, whichever of these files sets it first in this order wins.
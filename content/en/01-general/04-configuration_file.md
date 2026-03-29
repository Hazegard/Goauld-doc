---
title: Configuration file
weight: 4
---

The server, agent, and client share the same behavior when loading configuration files.

Each component looks for a configuration file in the `$HOME/.config` directory, then in the current directory.

The configuration filenames for each component are:

|        Server         |                Agent                |    Client    |
|:---------------------:|:-----------------------------------:|:------------:|
| `goauld_server.yaml`  | `goauld_agent.yaml` / `goauld.yaml` | `tealc.yaml` |
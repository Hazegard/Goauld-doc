---
title: Configuration file
weight: 4
---

The server, agent, and client have the same behavior regarding loading config file.

Each component looks for a config file in the `$HOME/.config` directory, then in the current directory.

The configuration filenames for each component are:

|        Server         |                Agent                |    Client    |
|:---------------------:|:-----------------------------------:|:------------:|
| `goauld_server.yaml`  | `goauld_agent.yaml` / `goauld.yaml` | `tealc.yaml` |
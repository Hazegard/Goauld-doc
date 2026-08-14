---
title: VS Code
description: Goauld client VS Code remote mode
weight: 18
---

`tealc vscode` launches [VS Code](https://code.visualstudio.com/) in remote mode against an agent, using the same SSH tunneling as `tealc ssh` under the hood. It requires the `code` CLI to be installed and available in `$PATH` locally.

```bash
tealc vscode [AGENT_NAME] [REMOTE_PATH]
```

`REMOTE_PATH` is the directory to open on the agent (defaults to the current directory).

> [!NOTE]
> The local `ssh_config`/`settings.json` generated for the remote session are stored per agent (under the OS cache directory, in a subfolder named after the target agent), so running `tealc vscode` against different agents at the same time doesn't overwrite one session's config with another's.

> [!WARNING]
> This downloads and executes the VS Code remote server on the agent, in the agent's working directory, which may trigger security tooling on the target. The cleanup mechanism that should remove the VS Code server folder when the agent exits may not work reliably: plan to manually clean up that folder.

> [!NOTE]
> VS Code Remote-SSH makes many separate connections to the agent (initial connect, server install, port forwards), which benefits significantly from SSH ControlMaster (see [client/controlmaster]({{< ref "04-client/10-controlmaster" >}})). Since `tealc vscode` has no `-M` flag of its own, this only kicks in if `control-master: true` is set as a persisted default in your configuration file.

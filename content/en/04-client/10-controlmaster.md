---
title: ControlMaster
description: Goauld client SSH ControlMaster
weight: 10
---


## SSH ControlMaster

> [!NOTE]
> This feature is not supported on Windows, this is an OpenSSH-for-Windows limitation, not something `tealc` itself restricts.


The client can leverage SSH ControlMaster mode to share multiple SSH sessions over a single network connection.

This improves connection speed and avoids repeating the authentication process for each session.

### Flag

- `-M`/`--control-master`: enable SSH ControlMaster to reuse the SSH connection.

> [!NOTE]
> `-M`/`--control-master` applies to:
> - `tealc ssh`
> - `tealc scp`/`rsync`/`rclone`
> - `tealc jump`
> - `tealc clip get`/`tealc clip set`
> - `tealc kill`/`tealc reset`
>
> For `tealc ssh`, this includes its default behavior, since `--ssh` is enabled by default for that command. It does not apply when explicitly running `tealc ssh AGENT --no-ssh` (pure port-forwarding without an interactive shell).

> [!NOTE]
> `tealc vscode` has no `-M` flag of its own - VS Code's Remote-SSH extension makes many separate connections behind the scenes (initial connect, server install, port forwards), each via the symlinked `ssh`/`scp` binaries `tealc vscode` sets up, and each one only knows about `-M` if `control-master: true` is set as a persisted default in your configuration file. There's no way to opt in per-invocation the way `tealc ssh AGENT -M` works for other commands.

> [!NOTE]
> `tealc delete` is unaffected by `-M`, since it uses a separate HTTP-based API path instead.

### Establishing the master

The master connection is created automatically on first use: the first `-M`
command run against an agent while no master is active establishes it, and every
subsequent `-M` command reuses that shared connection, skipping the
authentication and password prompt.

This applies to every command listed above, not just `tealc ssh`. For example,
running `tealc clip get AGENT -M` with no master already open will start one and
leave its socket in place for later commands to reuse.

Once established, the master stays alive in the background for a short idle
period (`ControlPersist`) after the last session ends, then exits on its own. The
next `-M` command after it has expired simply establishes a new one.

---
title: SCP
description: Goauld client file transfer (SCP, Rsync, Rclone)
weight: 5
---



> [!IMPORTANT]
> For Windows paths, `/` must be used instead of `\`:
> ```bash
> tealc scp [AGENT_NAME]:C:/PATH/TO/FOLDER .
> ```

## SCP

> [!NOTE]
> The `-r` flag is included by default.



### Transfer files from the client to the agent

```bash
tealc scp /PATH/TO/SOURCE/FILE [AGENT_NAME]:/PATH/TO/TARGET/FILE
```



### Transfer files from the agent to the client

```bash
tealc scp [AGENT_NAME]:/PATH/TO/TARGET/FILE /PATH/TO/SOURCE/FILE
```

## Rsync

> [!NOTE]
> Requires the `rsync` binary to be installed locally.

> [!NOTE]
> The `-r -v -P` flags are automatically included by default, along with any flags you provide. Additional flags like `-a` can be added as needed.

```bash
tealc rsync -a [AGENT_NAME]:C:/Path1 [AGENT_NAME]:C:/Windows/PATH2  .
```

> [!WARNING]
> On Windows agents, you can only copy from or to one drive per command.
> You can copy multiple directories from the same drive, but cannot copy from C: and D: in the same command.

## Rclone

`rclone` can be used to mount a folder from the agent to the local machine.

> [!NOTE]
> Requires the `rclone` binary to be installed locally. On macOS, `tealc` uses `rclone nfsmount` instead of `rclone mount` (NFS-based mounting rather than FUSE), which has different local requirements.

```bash
tealc rclone [AGENT_NAME]:/remote/path /local/path
```

Like `rsync`, `rclone` accepts additional arguments appended after the paths, passed straight through to the underlying `rclone` command:

```bash
tealc rclone [AGENT_NAME]:/remote/path /local/path --vfs-cache-mode writes
```

## Common flags

`scp`, `rsync` and `rclone` all support:

- `--log`: record the session to a log file
- `--[no-]print`: print the generated command instead of running it

`scp` additionally supports:

- `-o`/`--ssh-opts`: extra options passed to the underlying SSH command
- `-F`/`--ssh-config-file`: path to an SSH configuration file, passed straight through to the underlying `scp` process (`scp -F ...`).

> [!NOTE]
> This is different from `tealc ssh`'s own `-F`/`--ssh-config-file` flag. There, the file is only used internally to resolve an agent-name alias and is never passed to the underlying `ssh` command (see [client/ssh]({{< ref "04-client/03-ssh" >}})).
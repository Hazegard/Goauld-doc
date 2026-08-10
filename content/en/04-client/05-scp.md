---
title: SCP
description: Transfer files from/to the agent
weight: 5
---



> [!IMPORTANT]
> For windows paths, `/` must be used instead of `\`:
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

## RSYNC

> [!NOTE]
> The `-r -v -P` flags are included by default and prepended to any flags you pass; the example below only needs to add flags beyond those (e.g. `-a`).

```bash
tealc rsync -a [AGENT_NAME]:C:/Path1 [AGENT_NAME]:C:/Windows/PATH2  .
```

> [!WARNING]
> On Windows agents, you can only copy from or to one drive per command.
> You can copy multiple directories from the same drive, but cannot copy from C: and D: in the same command.

## RCLONE

`rclone` can be used to mount a folder from the agent to the local machine.

```bash
tealc rclone [AGENT_NAME]:/remote/path /local/path
```

## Common flags

`scp`, `rsync` and `rclone` all support:

- `--log`: record the session to a log file
- `--[no-]print`: print the generated command instead of running it

`scp` additionally supports:

- `-o`/`--ssh-opts`: extra options passed to the underlying SSH command
- `-F`/`--ssh-config-file`: path to an SSH configuration file to use
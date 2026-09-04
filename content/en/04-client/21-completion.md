---
title: Shell completion
description: Generating and installing tealc shell completion for zsh, bash, and fish
weight: 21
---

## Shell completion

`tealc completion [SHELL]` prints a completion script to standard output. The script
covers every command and subcommand, their flags, and — where it makes sense — the
values those flags and arguments expect.

```bash
tealc completion zsh
tealc completion bash
tealc completion fish
```

The supported shells are `zsh`, `bash`, and `fish`.

## What gets completed

- **Commands and subcommands**: `tealc <TAB>` lists every command; nested commands such
  as `tealc clip <TAB>` and `tealc wireguard <TAB>` list their subcommands.
- **Flags**: after a command, both long and short flags complete, e.g.
  `tealc ssh --<TAB>` or `tealc compile --<TAB>`.
- **Agent names**: any argument that takes an agent (for example `tealc ssh <TAB>`,
  `tealc logs <TAB>`, `tealc kill <TAB>`) is completed by running `tealc list`, so the
  suggestions are your live agents.
- **Enum values**: flags with a fixed set of values complete them, for example
  `tealc compile --id <TAB>` (`all client server agent agent-mini agent.dll`),
  `tealc compile --goos <TAB>` (`windows linux darwin`), and
  `tealc compile --goarch <TAB>` (`amd64 arm64 arm 386`).
- **Files and directories**: path-taking flags complete filesystem entries, for example
  `-F/--ssh-config-file`, `-c/--config-file`, `tealc logs --output`, and the compile
  flags `--env`, `--wordlist`, `--source`, and `-o/--output`.

> [!NOTE]
> Agent-name completion runs `tealc list` each time it is triggered, which contacts the
> server. It therefore needs a working client configuration (server address and access
> token) just like any other command that talks to the server.

## Installing

### zsh

Write the script to a directory on your `$fpath`. A common system-wide location:

```bash
tealc completion zsh | sudo tee /usr/share/zsh/site-functions/_tealc
```

Then restart your shell (or run `compinit`). To install it for your user only, place it
in a personal completion directory that is on your `$fpath`, for example:

```bash
mkdir -p ~/.zsh/completions
tealc completion zsh > ~/.zsh/completions/_tealc
# ensure this is in your ~/.zshrc before `compinit`:
#   fpath=(~/.zsh/completions $fpath)
```

### bash

```bash
tealc completion bash | sudo tee /etc/bash_completion.d/tealc
```

Or source it from your `~/.bashrc` for a per-user install:

```bash
tealc completion bash > ~/.tealc-completion.bash
echo 'source ~/.tealc-completion.bash' >> ~/.bashrc
```

### fish

```bash
tealc completion fish > ~/.config/fish/completions/tealc.fish
```

## Quick test without installing

You can load the completion into the current shell to try it out.

```bash
# zsh
eval "$(tealc completion zsh)"

# bash
source <(tealc completion bash)

# fish
tealc completion fish | source
```

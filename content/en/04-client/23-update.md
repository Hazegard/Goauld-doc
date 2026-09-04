---
title: Update
description: Remote agent upgrading via SSH
weight: 23
---

## Remote agent upgrading

`tealc update [AGENT_NAME] [BINARY_PATH]` sends a new agent binary to a connected agent and instructs it to restart with the new version. This allows you to upgrade agents in place without manual intervention.

```bash
tealc update [AGENT_NAME] [BINARY_PATH]
```

The client transfers the binary over SSH, waits for the agent's receipt confirmation, and the agent then restarts with the uploaded binary.

## Example

```bash
# Upgrade agent 'production-001' with a new binary
tealc update production-001 ./goauld-agent-linux-amd64

# Upgrade agent 'target-machine' with a binary from a different path
tealc update target-machine /opt/binaries/goauld-agent-v2.0
```

## Flags

```bash
  -h, --help                                 Show context-sensitive help.
  -s, --server=""                            HTTP server address to connect to ($TEALC_SERVER).
  -S, --ssh-server=""                        SSH server address to connect to ($TEALC_SSH_SERVER).
      --[no-]ws                              Tunnel SSH traffic over a WebSocket to --server instead of a raw TCP connection to --ssh-server ($TEALC_WS).
      --access-token=""                      Access token for the /manage/ API endpoint ($TEALC_ACCESS_TOKEN).
      --admin-token=""                       Admin token for the /admin/ API endpoint ($TEALC_ADMIN_TOKEN).
  -q, --quiet                                Suppress all log output ($TEALC_QUIET).
  -v, --verbose                              Increase log verbosity. Repeat for more detail ($TEALC_VERBOSE).
  -k, --insecure                             Allow insecure connections (skip TLS certificate verification) ($TEALC_INSECURE).
  -V, --version                              Display version information and exit ($TEALC_VERSION).
      --generate-config                      Generate a configuration file based on the current options ($TEALC_GENERATE_CONFIG).
  -c, --config-file=""                       Path to configuration file ($TEALC_CONFIG_FILE).
  -P, --password=""                          Agent static password ($TEALC_PASSWORD).
  -Q, --prompt                               Prompt for the agent's static password ($TEALC_PROMPT).
      --[no-]save-password                   Save the prompted password in the configuration file ($TEALC_SAVE_PASSWORD).
  -M, --control-master                       Enable SSH control master to reuse the SSH connection ($TEALC_CONTROL_MASTER).
      --wait-timeout=SECONDS                 Seconds to wait for agent to come online (negative or 0 = indefinite, positive = wait N seconds) ($TEALC_WAIT_TIMEOUT).
```

> [!NOTE]
> The agent must be online and reachable via SSH for the upgrade to succeed. If the agent is not currently connected, you can use `--wait-timeout` to wait for it to come online before attempting the upgrade.

> [!NOTE]
> If the agent has a static password configured, you will be prompted for it unless the password is cached via `--save-password` or explicitly provided with `-P`.

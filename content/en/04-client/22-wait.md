---
title: Wait
description: Waiting for an agent to come online
weight: 22
---

## Wait for an agent to come online

`tealc wait [AGENT_NAME]` blocks until an agent connects to the server and comes online. This is useful in scripts and automation that need to wait for an agent before proceeding.

```bash
# Wait indefinitely for the agent to come online
tealc wait [AGENT_NAME] --wait-timeout=0

# Wait for at most 30 seconds
tealc wait [AGENT_NAME] --wait-timeout=30
```

## Timeout behavior

The `--wait-timeout` flag controls how long to wait:

- **Zero or negative** (default `-1`): Wait indefinitely until the agent is online.
- **Positive** (e.g., `30`): Wait for at most N seconds, polling every second until the agent comes online or the timeout expires.

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
      --wait-timeout=SECONDS                 Seconds to wait for agent to come online (negative or 0 = indefinite, positive = wait N seconds) ($TEALC_WAIT_TIMEOUT).
```

---
title: SSH
description: Goauld client SSH connections
weight: 3
---

## Connect to an agent

`tealc ssh [AGENT_NAME]` is a wrapper around SSH that automatically handles authentication and proxy setup to connect you to an agent through the server.

### How tealc ssh Works

**Example underlying SSH command:** this is the full command `tealc ssh` builds and executes on your behalf, shown here for reference; you don't need to type this yourself, see "Simple usage" below.

```bash
# Environment setup and authentication
env SSH_ASKPASS_REQUIRE=force \
SSH_ASKPASS=/usr/local/bin/tealc \
TEALC_TYPE=agent \
TEALC_SERVER=http://localhost \
TEALC_SSH_SERVER=localhost:2222 \
TEALC_ACCESS_TOKEN=[ACCESS_TOKEN] \
TEALC_ADMIN_TOKEN= \
TEALC_AGENT=user@hostname1 \
TEALC_VERBOSE=0 \
TEALC_QUIET=false \
TEALC_CONFIG_FILE= \
TEALC_PROMPT=true \

# Main SSH invocation with security options and proxy forwarding
ssh -oStrictHostKeyChecking=no -oUserKnownHostsFile=/dev/null \
-oPubkeyAuthentication=no -oPreferredAuthentications=password \
-oLogLevel=ERROR -oExitOnForwardFailure=no \
-oNumberOfPasswordPrompts=1 \

# ProxyCommand for agent connection through server
-oProxyCommand='env SSH_ASKPASS_REQUIRE=force \
SSH_ASKPASS=/usr/local/bin/tealc \
TEALC_TYPE=otp \
TEALC_SERVER=http://localhost \
TEALC_SSH_SERVER=localhost:2222 \
TEALC_ACCESS_TOKEN=[ACCESS_TOKEN] \
TEALC_ADMIN_TOKEN= \
TEALC_AGENT=user@hostname1 \
TEALC_VERBOSE=0 \
TEALC_QUIET=false \
TEALC_CONFIG_FILE= \
TEALC_PROMPT=true \
ssh -oClearAllForwardings=no -oStrictHostKeyChecking=no \
-oUserKnownHostsFile=/dev/null -oPubkeyAuthentication=no \
-oPreferredAuthentications=password -oLogLevel=ERROR \
-oExitOnForwardFailure=no -oNumberOfPasswordPrompts=1 \
-p2222 -W127.0.0.1:49521 -L1080:127.0.0.1:49524 \
-L3128:127.0.0.1:54079 user@hostname1@localhost' \

# Target agent connection
user@hostname1@39fce32832f3375a409d99a13a4f0c77
```

**Simple usage:**

```bash
tealc ssh [AGENT_NAME]
```

## Default Proxy Forwarding

By default, `tealc ssh` automatically forwards the agent's proxies (SOCKS, HTTP, MITM HTTP, and WireGuard) locally if they are exposed by the agent. See [client/proxies]({{< ref "04-client/04-proxies" >}}) for details.

## Custom SSH Options

You can pass additional arguments to the underlying SSH command. These must be placed at the end of the command line, after the agent name:

```bash
tealc ssh [AGENT_NAME] -L "127.0.0.1:8080:127.0.0.1:8080"
```

> [!NOTE]
> By default, `tealc ssh` makes two chained SSH connections. The first connects to the Goauld server and sets up the proxy forwards. The second, the actual `ssh` process you see run, uses the first as its `ProxyCommand` and connects through it to the agent's own SSHD. Additional arguments are appended to this second connection, so they only apply when it's actually established, i.e. when `--ssh` is active (enabled by default for `tealc ssh`).
>
> With `--no-ssh`, only the first connection (to the server) is made, purely to forward the proxy ports. The agent's SSHD is never contacted directly. Since there is no second connection, additional arguments are dropped.

## Connecting over WebSocket

If the server is behind a reverse proxy or firewall that only allows HTTP(S) egress, pass `--ws` to tunnel SSH traffic over a WebSocket instead of a raw TCP connection to `--ssh-server`. The WebSocket is dialed against `--server`'s address (the same one `/manage/` and `/admin/` use), with its scheme swapped to `ws`/`wss`; `--ssh-server` is not used at all in this mode. All SSH traffic (both the outer/inner `ssh` subprocess connections `tealc ssh` builds, and `tealc`'s own internal SSH client used by `kill`/`reset`/`delete`/clipboard/`wireguard`) then tunnels through the server's `/ssh-ws/` endpoint instead of dialing the sshd port directly.

This requires the server to be started with `--ssh-websocket`, see [Reverse proxy support]({{< ref "03-server/03-access_control" >}}#reverse-proxy-support) in Access control.

> [!NOTE]
> `tealc bind` and `tealc embed-server` always route their own SSH access through an equivalent in-process WebSocket bridge, whether or not `--ws` is passed: see [client/agent binding]({{< ref "04-client/16-agent_binding" >}}) and [client/embed server]({{< ref "04-client/15-embed_server" >}}). `--ws` here is specifically the opt-in for `tealc ssh` (and the other commands sharing its SSH connection path) to reach a separately deployed server the same way.


### Flags

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

      --[no-]socks                           Forward the agent's SOCKS proxy to the local host ($TEALC_SOCKS).
      --[no-]http                            Forward the agent's HTTP proxy to the local host ($TEALC_HTTP).
      --[no-]http-mitm                       Forward the agent's HTTP MITM proxy to the local host ($TEALC_HTTP_MITM).
      --[no-]wg                              Forward the agent's WireGuard interface to the local host ($TEALC_WG).
      --socks-port=1080                      Local port to bind the SOCKS proxy ($TEALC_SOCKS_PORT).
      --http-port=3128                       Local port to bind the HTTP proxy ($TEALC_HTTP_PORT).
      --http-mitm-port=3129                  Local port to bind the HTTP MITM proxy ($TEALC_HTTP_MITM_PORT).
      --wg-port=51820                        Local port to bind the WireGuard proxy ($TEALC_WG_PORT).
      --[no-]ssh                             Connect directly to the agent's SSH service ($TEALC_SSH).
      --[no-]print                           Print the generated SSH command instead of executing it ($TEALC_PRINT).
      --proxy                                Use direct STDIN/STDOUT mode for ProxyCommand compatibility ($TEALC_PROXY).
      --log                                  Record the SSH session to a log file ($TEALC_LOG).
      --session                              Select and attach to a persistent shell session ($TEALC_SESSION).
  -o, --ssh-opts=SSH-OPTS,...                Additional SSH options (equivalent to '-o')
  -F, --ssh-config-file=STRING               Path to an SSH configuration file to use ($TEALC_SSH_CONFIG_FILE).
```

> [!NOTE]
> **SSH config file behavior differs across commands:**
> - `tealc ssh`: Uses config file only to resolve agent names
> - `tealc scp`: Passes config file directly to the underlying scp command (see [client/scp]({{< ref "04-client/05-scp" >}}))

> [!NOTE]
> `tealc ssh -V`/`--version` is a special case: it prints the version of the system's `ssh` client instead of `tealc`'s own version.

> [!NOTE]
> If the agent password is wrong, the client automatically re-prompts for it and retries, up to 4 attempts in total, before giving up. This applies to `tealc ssh`, `scp`, `rsync`, `rclone`, `jump`, and `bind` (which reuses the same SSH connection path), as well as `tealc clip get`/`set`, `kill`, and `reset`.

> [!NOTE]
> `--save-password` writes the prompted password to the client's configuration file in cleartext, under a top-level `agent-password:` map keyed by agent name:
> ```yaml
> agent-password:
>   user@hostname1: [password]
> ```
> On later runs, if the target agent is found in this map, the password prompt is skipped automatically. This also applies when `-M`/`--control-master` is used: if there is no already-open control connection yet, the client falls back to a saved or explicitly supplied (`-P`) password instead of prompting.

---
title: ssh
description:
weight: 3
---

## Connect to an agent

This command is a wrapper around SSH that automatically connects to an agent through the server.


### Example of the underlying SSH command
```bash
env SSH_ASKPASS_REQUIRE=force \
SSH_ASKPASS=/usr/local/bin/tealc \
TEALC_TYPE=agent \
TEALC_SERVER=http://localhost \
TEALC_SSH_SERVER=localhost:2222 \
TEALC_ACCESS_TOKEN=[ACCESS_TOKEN] \
TEALC_AGENT=user@hostname1 \
TEALC_PROMPT_STATIC_PASSWORD=true \
ssh -oStrictHostKeyChecking=no -oUserKnownHostsFile=/dev/null \
-oPubkeyAuthentication=no -oPreferredAuthentications=password 
-oLogLevel=ERROR -oExitOnForwardFailure=no \
-oNumberOfPasswordPrompts=1 \
-oProxyCommand='env SSH_ASKPASS_REQUIRE=force \
SSH_ASKPASS=/usr/local/bin/tealc \
TEALC_TYPE=otp \
TEALC_SERVER=http://localhost \
TEALC_SSH_SERVER=localhost:2222 \
TEALC_ACCESS_TOKEN=[ACCESS_TOKEN] \
TEALC_AGENT=user@hostname1 \
TEALC_PROMPT_STATIC_PASSWORD=true \
ssh -oClearAllForwardings=no -oStrictHostKeyChecking=no \
-oUserKnownHostsFile=/dev/null -oPubkeyAuthentication=no \
-oPreferredAuthentications=password -oLogLevel=ERROR \
-oExitOnForwardFailure=no -oNumberOfPasswordPrompts=1 \
-p2222 -W127.0.0.1:49521 -L3128:127.0.0.1:54079 \
-L1080:127.0.0.1:49524 user@hostname1@localhost' \
user@hostname1@39fce32832f3375a409d99a13a4f0c77
```

```bash
tealc ssh [AGENT_NAME]
```

You can also pass flags to the underlying SSH command:

```bash
tealc ssh [AGENT_NAME] -L "127.0.0.1:8080:127.0.0.1:8080"
```

> [!NOTE]
> By default, the `tealc ssh` command automatically mounts the proxies (SOCKS, HTTP, MITM HTTP & WireGuard), if they are exposed by the agent (see [client/proxies]({{< ref "04-client/04-proxies" >}}))




### Flags

```bash
  -h, --help                                 Show context-sensitive help.
  -s, --server=""                            HTTP server address to connect to ($TEALC_SERVER).
  -S, --ssh-server=""                        SSH server address to connect to ($TEALC_SSH_SERVER).
      --access-token=""                      Access token for the /manage/ API endpoint ($TEALC_ACCESS_TOKEN).
      --admin-token=""                       Admin token for the /admin/ API endpoint ($TEALC_ADMIN_TOKEN).
  -q, --quiet                                Suppress all log output ($TEALC_QUIET).
  -v, --verbose                              Increase verbosity level. Repeat for more detailed logs ($TEALC_VERBOSE).
  -k, --insecure                             Allow insecure connections (skip TLS certificate verification) ($TEALC_INSECURE).
  -V, --version                              Display version information and exit ($TEALC_VERSION).
      --generate-config                      Generate a configuration file based on the current options ($TEALC_GENERATE_CONFIG).
  -c, --config-file=""                       Path to configuration file ($TEALC_CONFIG_FILE).
  -P, --password=""                          Agent private password ($TEALC_PASSWORD).
  -Q, --prompt                               Prompt for the agent's private password ($TEALC_PROMPT).
      --[no-]save                            Save the prompted password in the configuration file ($TEALC_SAVE).
  -M, --control-master                       enable SSH control master to reuse SSH connection ($TEALC_CONTROL_MASTER).

      --[no-]socks                           Forward the agent's SOCKS proxy to the local host ($TEALC_SOCKS).
      --[no-]http                            Forward the agent's HTTP proxy to the local host ($TEALC_HTTP).
      --[no-]http-mitm                       Forward the agent's HTTP MITM proxy to the local host ($TEALC_HTTP_MITM).
      --[no-]wg                              Forward the agent's WireGuard interface to the local host ($TEALC_WG).
      --socks-port=1080                      Local port to bind the SOCKS proxy ($TEALC_SOCKS_PORT).
      --http-port=3128                       Local port to bind the HTTP proxy ($TEALC_HTTP_PORT).
      --http-mitm-port=3129                  Local port to bind the HTTP MITM proxy ($TEALC_HTTP_MITM_PORT).
      --wg-port=51820                        Local port to bind the WireGuard proxy ($TEALC_WG_PORT).
      --[no-]ssh                             Connect directly to the agent’s SSH service ($TEALC_SSH).
      --[no-]print                           Print the generated SSH command instead of executing it ($TEALC_PRINT).
      --proxy                                Use direct STDIN/STDOUT mode for ProxyCommand compatibility ($TEALC_PROXY).
      --log                                  Record the SSH session to a log file ($TEALC_LOG).
  -F, --ssh-config-file=STRING               Path to an SSH configuration file to use ($TEALC_SSH_CONFIG_FILE).
```

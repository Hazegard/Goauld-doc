---
title: Tunneling
description: Goauld agent tunneling
weight: 1
---

The Goauld agent supports multiple transport mechanisms to communicate with the server. If a transport fails, the agent automatically falls back to the next available method.

## Supported transports

The agent can connect using these transports (in default order):
1. Direct SSH connection
2. SSH over TLS
3. SSH over WebSocket
4. SSH over HTTP
5. SSH over DNS
6. SSH over QUIC (optional, requires `--rssh-order` configuration)

## Default behavior

For each transport, the agent attempts connection with a 60-second timeout (configurable with `--ssh-timeout`). After exhausting all transports, the agent retries indefinitely by default. Each retry cycle starts from the beginning.

## Configuration

Transport order and retry behavior can be customized:
- `--rssh-order`: Customize transport order (example: `--rssh-order=ssh,tls,ws,http,dns`)
- `--max-retries`: Limit retry attempts (example: `--max-retries 5` stops after 5 failed cycles; default is unlimited)

> [!NOTE]
> `--rssh-order` also accepts these special values:
> - `browser`: Use a browser-based tunnel (see [Browser proxy](#browser-proxy))
> - `bind`: Bind to a local socket (see [Agent binding](#agent-binding))
> 
> Relay agent routing is configured separately via `--relay-addr`, not through `--rssh-order` (see [agent/relay]({{< ref "02-agent/03-relay" >}})).


## Direct SSH connection


### Flags

- `--ssh-server`: (`[IP/Hostname]:[PORT]`)
- `--rssh-port`: the remote port the SSH tunnel binds to on the server side (default: `0`, meaning a random port is chosen).


## SSH over TLS

The SSH connection is encapsulated over a TLS connection.

> [!NOTE]
> TLS is used for proxy compatibility, not encryption: some proxies may allow TLS while blocking SSH.


### Flags
- `--tls-server`: (`[IP/Hostname]:[PORT]`) address of the TLS listener on the server.

The target SSH service itself is still the one configured via the `--ssh-server` flag; `--tls-server` only configures the TLS transport used to reach it.



## SSH over WebSocket

The SSH connection is encapsulated over a WebSocket connection.

This makes it easier to bypass restrictive proxies while maintaining acceptable performance.

### Flags
- `--server`: the WebSocket endpoint is `[server]/wssh/`

## SSH over HTTP

The SSH connection is encapsulated over HTTP requests/responses.

> [!WARNING]
> This transport is considerably slower than the previous ones, but allows traffic to pass through proxies that block WebSockets for instance.

### Flags

- `--server`: the HTTP endpoint is `[server]/sshttp/`



## SSH over DNS

This transport encapsulates SSH traffic inside DNS queries and responses, allowing communication in highly restricted environments where only DNS traffic is permitted.


> [!WARNING]
> This transport is considerably slower than the previous ones, but allows traffic to pass through proxies that block WebSockets.

### Flags

- `--dns-server`: DNS resolvers used to reach the Goauld DNS server. Optionally includes the Goauld server itself if directly reachable.
  - The special value `system` automatically adds detected system DNS servers to the list.
- `--dns-domain`: the domain on which DNS queries are performed

> [!NOTE]
> Shorter domain names allow more payload data per query, improving throughput.

- `--custom-dns-command`: in some cases, the DNS queries are not feasible directly, but system commands such as `Resolve-DnsName` are still allowed to perform DNS queries. When this flag is used, the provided command is executed for each DNS query. The command is responsible for performing the DNS query, and parsing the DNS response and returning the response as raw bytes.

{{< tabpane text=true >}}
{{% tab header="PowerShell" %}}
```powershell
((Resolve-DnsName -Type TXT -Server 127.0.0.1 '%s')[0].Strings -join '' -replace '\s+', '' -split '..' | ForEach-Object { [Convert]::ToByte($_,16) } )
```
{{% /tab %}}
{{% tab header="Bash" %}}
```bash
dig +short +unknownformat -t TXT '%s' @127.0.0.1 | head -n1 | cut -d ' ' -f3- | tr -d ' '  | xxd -r -p
```
{{% /tab %}}
{{< /tabpane >}}

## SSH over QUIC

The SSH connection is encapsulated over QUIC. This transport is opt-in and not part of the default `--rssh-order` (see the note above); add `quic` to `--rssh-order` to enable it.

### Flags

- `--quic-domain`: the QUIC domain used to tunnel traffic.

## Browser proxy

The agent can use a web browser to tunnel all the traffic.

1. The agent exposes a simple web page with custom JavaScript. The JavaScript opens 4 WebSocket connections
   1. Two connecting to the server (Control & data)
   2. Two connecting to the agent (Control & data)
      - The agent exposes a custom endpoint to allow the browser to initiate the connection to the agent
2. The web page pipes the WebSocket connections

<video width="90%" controls autoplay muted>
    <source src="browser-proxy.webm" type="video/webm">
    Your browser does not support the video tag.
</video>

### Flags

- `--browser-proxy-port`: the port used to expose the custom web pages and the WebSocket endpoints used by the browser to connect to the agent.

## Egress proxies

If required, the agent will try to reach the server using the proxy configuration identified on the system (see [https://github.com/aus/proxyplease?tab=readme-ov-file#proxy-selection](https://github.com/aus/proxyplease?tab=readme-ov-file#proxy-selection)).

### Flags

- `--proxy`: use a custom proxy instead of the system proxy
- `--proxy-username`: Username to authenticate on the proxy
- `--proxy-password`: Password to authenticate on the proxy
- `--proxy-domain`: Domain to authenticate on the proxy
- `--no-proxy`: Ignore the system proxy

The proxy will be selected by the following priority:

#### Windows

1. `--proxy [PROXY_URL]` (or any other means allowing configuration of the `proxy` flag, see  [general/compilation]({{< ref "01-general/02-compilation" >}}))
2. Environment Variable: `HTTPS_PROXY`, `HTTP_PROXY`, `FTP_PROXY`, or `ALL_PROXY`. `NO_PROXY` is respected.
3. Internet Options: Automatically detect settings (`WPAD`)
4. Internet Options: Use automatic configuration script (`PAC`)
5. Internet Options: Manual proxy server
6. WINHTTP: (`netsh winhttp`)

#### Linux

1. `--proxy [PROXY_URL]` (or any other means allowing configuration of the `proxy` flag, see  [general/compilation]({{< ref "01-general/02-compilation" >}}))
2.  Environment Variable: `HTTPS_PROXY`, `HTTP_PROXY`, `FTP_PROXY`, or `ALL_PROXY`. `NO_PROXY` is respected.

#### macOS

1. `--proxy [PROXY_URL]` (or any other means allowing configuration of the `proxy` flag, see  [general/compilation]({{< ref "01-general/02-compilation" >}}))
2. Environment Variable: `HTTPS_PROXY`, `HTTP_PROXY`, `FTP_PROXY`, or `ALL_PROXY`. `NO_PROXY` is respected.
3. Network Settings: `scutil`

## Agent binding


The client connects directly to the port exposed by the agent (see [client/agent binding]({{< ref "04-client/16-agent_binding" >}})).

### Flags

- `--rssh-order=bind`
- `--bind-port`

> [!NOTE]
> `--bind-port` is an alias of `--browser-proxy-port` (see [Browser proxy](#browser-proxy) above): both flags configure the same underlying port.

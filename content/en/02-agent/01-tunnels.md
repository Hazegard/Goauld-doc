---
title: Tunneling
description: Goauld agent tunneling
weight: 1
---

The Goauld agent supports multiple transport mechanisms to establish communication with the server.
If a transport fails, the agent automatically falls back to the next available method.


The agent will try to connect to the server using several transports:
1. Direct SSH connection
2. SSH over QUIC
3. SSH over TLS
4. SSH over WebSocket
5. SSH over HTTP
6. SSH over DNS

For each transport protocol, the agent will try to establish a connection to the server, with a 60 seconds timeout (configurable using `--ssh-timeout` flag). If the connection is established, the agent finalizes the connection.

Otherwise, the agent will try to connect using the next transport protocol (configurable using `--rssh-order`, e.g. `--rssh-order=ssh,tls,ws,http,dns`).

If no connection has been established after a full loop, the agent will try again.


## Direct SSH connection


### Flags

- `--ssh-server`: (`[IP/Hostname]:[PORT]`)


## SSH over TLS

The SSH connection is encapsulated over a TLS connection.

> [!NOTE]
> We do not rely on the TLS encryption mechanism here. Instead, we use TLS so that some proxies might allow TLS traffic while blocking SSH


### Flags
- `--tls-server`: (`[IP/Hostname]:[PORT]`)
The SSH server is configured using the `--ssh-server` flag.



## SSH over Websocket

The SSH connection is encapsulated over a Websocket connection.

This makes it easier to bypass restrictive proxies, while still maintaining an acceptable speed.

### Flags
- `--server`: the websocket endpoint is `[server]/wssh/`

## SSH over HTTP

The SSH connection is encapsulated over HTTP requests/responses.

> [!WARNING]
> This transport is considerably slower than the previous, but allows traffic to pass through proxies that block WebSockets for instance.

### Flags

- `--server`: the HTTP endpoint is `[server]/sshttp/`



## SSH over DNS

This transport encapsulates SSH traffic inside DNS queries and responses, allowing communication in highly restricted environments where only DNS traffic is permitted.


> [!WARNING]
> This transport is considerably slower than the previous ones, but allows traffic to pass through proxies that block WebSockets.

### Flags

- `--dns-server`: the servers used as recursive DNS resolvers to reach the DNS server. If the goauld server is directly reachable, it is possible to add it.
  - The special value `system` allows to automatically add the detected system DNS servers to the list.
- `--dns-domain`: the domain on which DNS queries are performed
> [!NOTE]
> A short domain name allows to add more data in the queries, which could result in improving the speed

- `--custom-dns-command`: in some context, the DNS queries are not feasible directly, but system commands such as `Resolve-DnsName` are still allowed to perform DNS queries. When this flag is used, the provided command is executed for each DNS query. The command is responsible for performing the DNS query, and parsing the DNS response and returning the response as raw bytes.

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


### Egress proxies

If required, the agent will try to reach the server using the proxy configuration identified on the system (see [https://github.com/aus/proxyplease?tab=readme-ov-file#proxy-selection](https://github.com/aus/proxyplease?tab=readme-ov-file#proxy-selection)).

### Flags

- `--proxy`: use a custom proxy instead of the system proxy
- `--proxy-username`: Username to authenticate on the proxy
- `--proxy-password`: Username to authenticate on the proxy
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

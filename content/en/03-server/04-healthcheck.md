---
title: Healthcheck
description: Goauld server healthcheck
weight: 4
---


To ensure tunnels are functioning correctly, some healthcheck scripts/tools are provided.


> [!WARNING]
> The provided healthchecks only verify that the SSHD server is reachable through the tunnel by checking the SSHD banner.


## SSH over TLS

```bash
echo "00000000000000000000000000000000\n" | timeout 1 openssl  s_client -quiet -connect $TLS_DOMAIN$:$TLS_PORT$ 2>/dev/null | grep -q "SSH-2.0-"
```


## SSH over WebSocket

### Building

```bash
go build -o ws-healthcheck ./healthcheck/websocket
```

### Running

```bash
{ echo "" ; sleep 1; } | ws-healthcheck  "wss://$WS_DOMAIN/wssh/00000000000000000000000000000000" 2>/dev/null
```


## SSH over DNS

### Building

```bash
go build -o dns-healthcheck ./healthcheck/dns/dnstt-client
```
### Running

```bash
{ echo "00000000000000000000000000000000S" ; sleep 1; } | dns-healthcheck -udp "$DNS_SERVER:$DNS_SERVER_PORT" "$DNS_DOMAIN" 2>/dev/null | grep -q "SSH-2.0-"
```

> [!NOTE]
> The "S" is required at the end of the echo

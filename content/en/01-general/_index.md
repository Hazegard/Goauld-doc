---
title: General information
description: Goauld general information
weight: 1
---


The compilation can be done directly, but it requires some tags:
- `client`: `-tags client`
- `mini_agent`: `-tags mini`

The compilation can also be achieved using goreleaser

## Optionnally required dependencies

- `garble` for obfuscation ([https://github.com/burrowers/garble](https://github.com/burrowers/garble))
- `upx` for compression [https://github.com/upx/upx](https://github.com/upx/upx)
- `goreleaser` for [https://github.com/goreleaser/goreleaser](https://github.com/goreleaser/goreleaser)


## Agent

- Direct compilation
```bash
go run -o tealc ./agent
```
- Using the script wrapper
```bash
go run ./scripts/build/  --id agent --goos windows --goarch amd64 --no-seed --gen-age-key=false --gen-access-token=false
```

- Using the cli
```bash
tealc compile --id agent --goarch amd64 --goos windows
```

Note: Compiling with the cli is recommended as it allows to feed the agent with compile time variable (see [Hot to feed variables])

## Server
```bash
go run -o goauld_server ./server
```
- Using the script wrapper
```bash
go run ./scripts/build/ --gen-age-key=false --gen-access-token=false --id server --goos linux --goarch amd64 -vvv
```

## Client
```bash
go run -tags client -o tealc ./client
```
- Using the script wrapper
```bash
go run ./scripts/build/ --gen-age-key=false --gen-access-token=false --id client --goos linux --goarch amd64 -vvv
```
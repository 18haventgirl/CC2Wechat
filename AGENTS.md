# CC2Wechat — Agent Instructions

A Go project bridging Claude Code to WeChat (personal account via iLink Bot).

## Project Identity

- **Name**: CC2Wechat
- **Language**: Go 1.25+
- **Purpose**: Connect Claude Code (AI coding agent) with WeChat personal account
- **Build**: `go build ./cmd/cc-connect/`
- **Entry point**: `cmd/cc-connect/main.go`

## Architecture

```
WeChat (iLink Bot) → platform/weixin/ → core/engine.go → agent/claudecode/ → Claude CLI
                                         ↑
                                    config/config.go
```

- `agent/claudecode/` — Claude Code CLI wrapper (start session, send prompt, stream events)
- `platform/weixin/` — WeChat iLink Bot (polling, receive/send messages, CDN upload)
- `core/` — Engine (session management, message routing, event processing)
- `config/` — TOML config parsing
- `daemon/` — System service management (systemd/launchd/schtasks)
- `cmd/cc-connect/` — CLI entry point + subcommands

## Key Commands

```bash
cc-connect setup              # Interactive setup wizard
cc-connect config example     # Print config template
cc-connect daemon install     # Install as system service
```

## Coding Conventions

- Follow existing code patterns in core/
- Interfaces are in `core/interfaces.go`
- Agent/platform registration via `core.RegisterAgent()` / `core.RegisterPlatform()` in init()
- Build tags gate platform/agent imports in `cmd/cc-connect/plugin_*.go`

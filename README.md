# CC2Wechat — Claude Code meets WeChat

<p align="center">
  <strong>Use Claude Code for AI-powered programming, right from WeChat</strong>
</p>

<p align="center">
  <a href="./README.md">English</a> | <a href="./README.zh-CN.md">中文</a>
</p>

CC2Wechat is a lightweight bridge connecting **Claude Code** (AI coding agent) with **WeChat** (personal account via iLink Bot). Send messages from WeChat, Claude Code executes programming tasks locally, and results stream back in real time.

A streamlined fork of [cc-connect](https://github.com/chenhg5/cc-connect).

## Features

- **Claude Code + WeChat only** — focused, lightweight, zero bloat
- **One-command setup** — `cc-connect setup` auto-detects environment, generates config, registers system service
- **Cross-platform** — Windows, macOS, Linux
- **Background service** — run as a system daemon with auto-start on login
- **Multi-session** — independent session contexts per project

## Quick Start

### 1. Install

```bash
git clone https://github.com/18haventgirl/CC2Wechat.git
cd CC2Wechat
go build -o cc-connect ./cmd/cc-connect/
```

### 2. Configure

```bash
# Interactive wizard (recommended)
./cc-connect setup

# Or non-interactive
./cc-connect setup --token YOUR_ILINK_BOT_TOKEN --no-daemon
```

The setup wizard walks you through:
1. Detecting Claude Code installation
2. Configuring WeChat iLink Bot token
3. Generating a minimal `config.toml`
4. Installing as a system service (optional)

### 3. Run

```bash
# Foreground
./cc-connect

# Background service
./cc-connect daemon start
./cc-connect daemon stop
./cc-connect daemon status
```

## Configuration

Config file at `~/.cc-connect/config.toml` (Windows: `%APPDATA%\.cc-connect\config.toml`):

```toml
language = "en"
data_dir = "~/.cc-connect/data"

[[projects]]
name = "default"

[projects.agent]
type = "claudecode"
[projects.agent.options]
# binary_path = "/usr/local/bin/claude"
# model = "claude-sonnet-4-6"

[[projects.platforms]]
type = "weixin"
[projects.platforms.options]
token = "${WEIXIN_BOT_TOKEN}"
# allow_from = "*"

[log]
level = "info"
```

### WeChat iLink Bot Options

| Option | Required | Description |
|--------|----------|-------------|
| `token` | Yes | iLink Bot Bearer Token |
| `allow_from` | No | Comma-separated allowed user IDs, `"*"` for all |
| `account_id` | No | Multi-account isolation label, default `"default"` |

### Claude Code Options

| Option | Required | Description |
|--------|----------|-------------|
| `binary_path` | No | Path to Claude CLI binary (auto-detected from PATH) |
| `model` | No | Model selection, e.g. `claude-sonnet-4-6` |

## Daemon Management

```bash
cc-connect daemon install    # Install (Windows: Task Scheduler, macOS: LaunchAgent, Linux: systemd)
cc-connect daemon uninstall  # Remove
cc-connect daemon start      # Start
cc-connect daemon stop       # Stop
cc-connect daemon restart    # Restart
cc-connect daemon status     # Check status
cc-connect daemon logs -f    # Follow logs
```

## Development

```bash
make build        # Build
make test         # Run tests
make release-all  # Cross-compile for all platforms
```

## License

MIT License

## Credits

Forked from [cc-connect](https://github.com/chenhg5/cc-connect) — thanks to the original author for the excellent work.

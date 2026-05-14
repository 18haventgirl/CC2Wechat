# cc-connect Minimal Edition Design Spec

**Date**: 2026-05-14  
**Status**: Approved  
**Goal**: Strip cc-connect down to Claude Code agent + WeChat (weixin) platform only, with an automated `setup` command.

---

## 1. Deletion Scope

### Delete (files and directories)

- `agent/acp/`, `agent/codex/`, `agent/cursor/`, `agent/devin/`, `agent/gemini/`, `agent/iflow/`, `agent/kimi/`, `agent/opencode/`, `agent/pi/`, `agent/qoder/`
- `platform/dingtalk/`, `platform/discord/`, `platform/feishu/`, `platform/line/`, `platform/max/`, `platform/qq/`, `platform/qqbot/`, `platform/slack/`, `platform/telegram/`, `platform/wecom/`, `platform/weibo/`
- `web/` (entire directory)
- `cmd/cc-connect/plugin_agent_acp.go`, `plugin_agent_codex.go`, `plugin_agent_cursor.go`, `plugin_agent_devin.go`, `plugin_agent_gemini.go`, `plugin_agent_iflow.go`, `plugin_agent_kimi.go`, `plugin_agent_opencode.go`, `plugin_agent_pi.go`, `plugin_agent_qoder.go`
- `cmd/cc-connect/plugin_platform_dingtalk.go`, `plugin_platform_discord.go`, `plugin_platform_feishu.go`, `plugin_platform_line.go`, `plugin_platform_max.go`, `plugin_platform_qq.go`, `plugin_platform_qqbot.go`, `plugin_platform_slack.go`, `plugin_platform_telegram.go`, `plugin_platform_wecom.go`, `plugin_platform_weibo.go`
- `cmd/cc-connect/plugin_web.go`
- `core/management.go` (management API server)
- `core/web_assets.go`, `core/web_manager.go`
- `core/bridge.go`, `core/webhook.go` (bridge/webhook servers — not needed for direct weixin)
- `docs/` files for removed platforms (dingtalk, discord, feishu, qq, qqbot, slack, telegram, wecom, weibo, max-webhook)

### Keep

- `agent/claudecode/` — the sole agent
- `platform/weixin/` — the sole platform
- `core/` — engine, interfaces, registry, session, message, cron, heartbeat (adapt where needed)
- `config/` — config parsing (trim example, keep config struct flexibility)
- `daemon/` — systemd/launchd/schtasks service management
- `cmd/cc-connect/main.go` — entry point (simplify)

### Adapt in core

- `Makefile`: Update ALL_AGENTS to `claudecode`, ALL_PLATFORMS to `weixin`
- `cmd/cc-connect/main.go`: Remove bridge/webhook/management server startup, adapt to single-project assumption where graceful
- `config.example.toml`: Replace with minimal template (~30 lines)
- `go.mod`: Remove dependencies that become unused after deletions

---

## 2. Setup Command

New subcommand: `cc-connect setup`.

### Interactive Mode (default)

4-step wizard:

1. **Detect Claude Code**: Check `PATH` for `claude` binary. If found, show version. If not, prompt user for installation path or offer to skip.
2. **Configure WeChat**: Prompt for iLink Bot `token` (required). Optionally prompt for `allow_from` (user ID allowlist) and `account_id`.
3. **Generate Config**: Write minimal `config.toml` to `~/.cc-connect/config.toml` (Windows: `%APPDATA%\.cc-connect\config.toml`). If existing config found, ask: overwrite / merge / abort.
4. **Register Daemon**: Offer to install as system service:
   - Windows: Task Scheduler (logon trigger)
   - macOS: LaunchAgent plist
   - Linux: systemd user service
   - Offer to start immediately after install.

### Non-Interactive Mode

Flags: `--token`, `--allow-from`, `--account-id`, `--claude-path`, `--no-daemon`, `--config-path`, `--force`.

### Implementation

New file: `cmd/cc-connect/setup.go`. Uses existing `daemon` package for service registration and `config` package for TOML generation.

---

## 3. Config Template (Minimal)

Target: ~30-line `config.example.toml`.

```toml
# cc-connect config
language = "zh-CN"
data_dir = "~/.cc-connect/data"

# [[providers]]
# name = "my-key"
# api_key = "${ANTHROPIC_API_KEY}"
# agent_types = ["claudecode"]

[[projects]]
name = "default"

[projects.agent]
type = "claudecode"

[[projects.platforms]]
type = "weixin"
[projects.platforms.options]
token = "${WEIXIN_BOT_TOKEN}"
# allow_from = "*"
# account_id = "default"

[log]
level = "info"
```

---

## 4. Cross-Platform Support

Setup and daemon must work on:

- **Windows 11**: Task Scheduler via `daemon/windows.go`, config at `%APPDATA%\.cc-connect\`
- **macOS**: LaunchAgent via `daemon/launchd.go`, config at `~/.cc-connect/`
- **Linux**: systemd user service via `daemon/systemd.go`, config at `~/.cc-connect/`

---

## 5. Non-Goals

- No multi-project support (single `default` project is assumed)
- No web UI or management API
- No bridge/webhook/relay servers
- No multi-agent or multi-platform routing
- No backward compatibility with full cc-connect config files (setup generates minimal config; old configs with unsupported types will error clearly)

---

## 6. Verification

- `go build ./cmd/cc-connect/` compiles successfully
- `cc-connect setup` runs the wizard and generates valid config
- `cc-connect --config <path>` starts successfully with generated config
- Claude Code agent receives messages from WeChat platform end-to-end
- `cc-connect daemon install/start/stop/status` works on each platform

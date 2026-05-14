# cc-connect Minimal Edition Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Strip cc-connect down to Claude Code agent + WeChat (weixin) platform only, add automated `cc-connect setup` command.

**Architecture:** Delete all unused agent packages (10 of 11), platform packages (11 of 12), web UI, bridge/webhook/management servers. Update config template, Makefile, and go.mod. Add `cmd/cc-connect/setup.go` implementing an interactive 4-step setup wizard with non-interactive flag mode.

**Tech Stack:** Go, BurntSushi/toml, daemon package (systemd/launchd/schtasks)

---

### Task 1: Delete unused agent packages

**Files:**
- Delete: `agent/acp/` (entire directory)
- Delete: `agent/codex/` (entire directory)
- Delete: `agent/cursor/` (entire directory)
- Delete: `agent/devin/` (entire directory)
- Delete: `agent/gemini/` (entire directory)
- Delete: `agent/iflow/` (entire directory)
- Delete: `agent/kimi/` (entire directory)
- Delete: `agent/opencode/` (entire directory)
- Delete: `agent/pi/` (entire directory)
- Delete: `agent/qoder/` (entire directory)

- [ ] **Step 1: Delete agent directories**

```bash
cd D:/WorkPlace/Tools/cc-connect
rm -rf agent/acp agent/codex agent/cursor agent/devin agent/gemini agent/iflow agent/kimi agent/opencode agent/pi agent/qoder
```

- [ ] **Step 2: Verify only claudecode remains**

```bash
ls agent/
```
Expected output: `claudecode/`

- [ ] **Step 3: Commit**

```bash
git add -A agent/
git commit -m "feat: remove unused agent packages, keep only claudecode

Co-Authored-By: Claude Opus 4.7 <noreply@anthropic.com>"
```

---

### Task 2: Delete unused platform packages

**Files:**
- Delete: `platform/dingtalk/`, `platform/discord/`, `platform/feishu/`, `platform/line/`, `platform/max/`, `platform/qq/`, `platform/qqbot/`, `platform/slack/`, `platform/telegram/`, `platform/wecom/`, `platform/weibo/`

- [ ] **Step 1: Delete platform directories**

```bash
cd D:/WorkPlace/Tools/cc-connect
rm -rf platform/dingtalk platform/discord platform/feishu platform/line platform/max platform/qq platform/qqbot platform/slack platform/telegram platform/wecom platform/weibo
```

- [ ] **Step 2: Verify only weixin remains**

```bash
ls platform/
```
Expected output: `weixin/`

- [ ] **Step 3: Commit**

```bash
git add -A platform/
git commit -m "feat: remove unused platform packages, keep only weixin

Co-Authored-By: Claude Opus 4.7 <noreply@anthropic.com>"
```

---

### Task 3: Delete web directory

**Files:**
- Delete: `web/` (entire directory)

- [ ] **Step 1: Delete web directory**

```bash
cd D:/WorkPlace/Tools/cc-connect
rm -rf web
```

- [ ] **Step 2: Delete web-related core files**

```bash
rm -f core/web_assets.go core/web_manager.go
```

- [ ] **Step 3: Commit**

```bash
git add -A web/ core/web_assets.go core/web_manager.go
git commit -m "feat: remove web UI and web asset embedding

Co-Authored-By: Claude Opus 4.7 <noreply@anthropic.com>"
```

---

### Task 4: Delete bridge, webhook, and management server core files

**Files:**
- Delete: `core/management.go`, `core/management_test.go`
- Delete: `core/bridge.go`, `core/bridge_test.go`, `core/bridge_capabilities.go`, `core/bridge_capabilities_test.go`, `core/bridge_capabilities_snapshot_test.go`
- Delete: `core/webhook.go`

- [ ] **Step 1: Delete core server files**

```bash
cd D:/WorkPlace/Tools/cc-connect
rm -f core/management.go core/management_test.go
rm -f core/bridge.go core/bridge_test.go core/bridge_capabilities.go core/bridge_capabilities_test.go core/bridge_capabilities_snapshot_test.go
rm -f core/webhook.go
```

- [ ] **Step 2: Verify the deletions**

```bash
ls core/management.go core/bridge.go core/webhook.go 2>&1
```
Expected: "No such file or directory" for all three.

- [ ] **Step 3: Commit**

```bash
git add -A core/
git commit -m "feat: remove bridge, webhook, and management server core files

Co-Authored-By: Claude Opus 4.7 <noreply@anthropic.com>"
```

---

### Task 5: Delete plugin files for removed agents and platforms

**Files:**
- Delete: `cmd/cc-connect/plugin_agent_acp.go`, `plugin_agent_codex.go`, `plugin_agent_cursor.go`, `plugin_agent_devin.go`, `plugin_agent_gemini.go`, `plugin_agent_iflow.go`, `plugin_agent_kimi.go`, `plugin_agent_opencode.go`, `plugin_agent_pi.go`, `plugin_agent_qoder.go`
- Delete: `cmd/cc-connect/plugin_platform_dingtalk.go`, `plugin_platform_discord.go`, `plugin_platform_feishu.go`, `plugin_platform_line.go`, `plugin_platform_max.go`, `plugin_platform_qq.go`, `plugin_platform_qqbot.go`, `plugin_platform_slack.go`, `plugin_platform_telegram.go`, `plugin_platform_wecom.go`, `plugin_platform_weibo.go`
- Delete: `cmd/cc-connect/plugin_web.go`

- [ ] **Step 1: Delete unused plugin files**

```bash
cd D:/WorkPlace/Tools/cc-connect/cmd/cc-connect
rm -f plugin_agent_acp.go plugin_agent_codex.go plugin_agent_cursor.go plugin_agent_devin.go plugin_agent_gemini.go plugin_agent_iflow.go plugin_agent_kimi.go plugin_agent_opencode.go plugin_agent_pi.go plugin_agent_qoder.go
rm -f plugin_platform_dingtalk.go plugin_platform_discord.go plugin_platform_feishu.go plugin_platform_line.go plugin_platform_max.go plugin_platform_qq.go plugin_platform_qqbot.go plugin_platform_slack.go plugin_platform_telegram.go plugin_platform_wecom.go plugin_platform_weibo.go
rm -f plugin_web.go
```

- [ ] **Step 2: Verify only claudecode + weixin plugin files remain**

```bash
ls plugin_*.go
```
Expected: `plugin_agent_claudecode.go  plugin_platform_weixin.go`

- [ ] **Step 3: Commit**

```bash
git add -A cmd/cc-connect/plugin_*.go
git commit -m "feat: remove plugin files for deleted agents, platforms, and web

Co-Authored-By: Claude Opus 4.7 <noreply@anthropic.com>"
```

---

### Task 6: Delete unused cmd subcommand files

**Files:**
- Delete: `cmd/cc-connect/web.go` — web admin subcommand
- Delete: `cmd/cc-connect/feishu.go`, `cmd/cc-connect/feishu_test.go` — feishu subcommand
- Delete: `cmd/cc-connect/relay.go` — relay subcommand
- Delete: `cmd/cc-connect/provider.go` — provider management subcommand (depends on multi-provider config logic)
- Delete: `cmd/cc-connect/doctor_runas.go`, `cmd/cc-connect/doctor_runas_windows.go`, `cmd/cc-connect/doctor_runas_test.go` — run-as-user doctor (references multi-user management)

Note: Keep `weixin.go` — WeChat platform CLI tools. Keep `daemon.go`, `daemon_test.go` — daemon management. Keep `config_cmd.go` — config example/format command.

- [ ] **Step 1: Delete unused subcommand files**

```bash
cd D:/WorkPlace/Tools/cc-connect/cmd/cc-connect
rm -f web.go feishu.go feishu_test.go relay.go provider.go doctor_runas.go doctor_runas_windows.go doctor_runas_test.go
```

- [ ] **Step 2: Commit**

```bash
git add -A cmd/cc-connect/
git commit -m "feat: remove unused subcommand files (web, feishu, relay, provider, doctor_runas)

Co-Authored-By: Claude Opus 4.7 <noreply@anthropic.com>"
```

---

### Task 7: Delete docs for removed platforms

**Files:**
- Delete: `docs/dingtalk.md`, `docs/discord.md`, `docs/feishu.md`, `docs/qq.md`, `docs/qqbot.md`, `docs/slack.md`, `docs/telegram.md`, `docs/wecom.md`, `docs/weibo.md`, `docs/max-webhook.md`, `docs/slack-app-manifest.json`, `docs/slack-feature-inventory.md`
- Delete: `docs/bridge-protocol.md`, `docs/bridge-protocol.zh-CN.md`
- Delete: `docs/management-api.md`, `docs/management-api.zh-CN.md`

- [ ] **Step 1: Delete unused doc files**

```bash
cd D:/WorkPlace/Tools/cc-connect
rm -f docs/dingtalk.md docs/discord.md docs/feishu.md docs/qq.md docs/qqbot.md docs/slack.md docs/telegram.md docs/wecom.md docs/weibo.md docs/max-webhook.md docs/slack-app-manifest.json docs/slack-feature-inventory.md
rm -f docs/bridge-protocol.md docs/bridge-protocol.zh-CN.md
rm -f docs/management-api.md docs/management-api.zh-CN.md
```

- [ ] **Step 2: Commit**

```bash
git add -A docs/
git commit -m "feat: remove docs for deleted platforms, bridge, and management API

Co-Authored-By: Claude Opus 4.7 <noreply@anthropic.com>"
```

---

### Task 8: Clean up main.go references to deleted components

**Files:**
- Modify: `cmd/cc-connect/main.go`

This is the most complex adaptation task. The following sections in `main.go` must be modified:

**8a. Remove subcommand cases from switch**

Remove these cases from the subcommand switch in `main()` (around lines 72-117):
```go
case "web":
    runWeb(os.Args[2:])
    return
case "feishu":
    runFeishu(os.Args[2:])
    return
case "relay":
    runRelay(os.Args[2:])
    return
case "provider":
    runProviderCommand(os.Args[2:])
    return
```

**8b. Remove SetWebSetupFunc / SetWebStatusFunc wiring** (around lines 744-760)

Remove the entire block:
```go
engine.SetWebSetupFunc(func() (int, string, bool, error) {
    // ... entire function body ...
})
engine.SetWebStatusFunc(func() string {
    // ... entire function body ...
})
```

**8c. Remove bridge server startup** (around lines 819-847)

Remove the entire `// Start bridge server if enabled` block including `var bridgeSrv *core.BridgeServer` through `bridgeSrv.Start()`.

**8d. Remove webhook server startup** (around lines 849-865)

Remove the entire `// Start webhook server if enabled` block.

**8e. Remove management server startup** (around lines 867-885)

Remove the entire `// Start management API server if enabled` block, including the `var mgmtSrv *core.ManagementServer` through all the `mgmtSrv.Set*` calls.

**8f. Remove bridge/webhook/management shutdown code** (around lines 1088-1095)

Remove:
```go
if mgmtSrv != nil {
    mgmtSrv.Stop()
}
if bridgeSrv != nil {
    bridgeSrv.Stop()
}
if webhookSrv != nil {
    webhookSrv.Stop()
}
```

**8g. Update banner text** (around lines 1306-1310)

Change from:
```go
  Bridge your messaging platforms to local AI coding agents.
  Supports: Claude Code, Codex, Cursor, Gemini CLI, Qoder CLI, OpenCode
  Platforms: Feishu, Telegram, Slack, DingTalk, Discord, LINE, WeChat Work, Weixin, QQ, QQ Bot
```
To:
```go
  Connect Claude Code to WeChat.
```

- [ ] **Step 1: Remove subcommand cases**

In `cmd/cc-connect/main.go`, remove `case "web":`, `case "feishu":`, `case "relay":`, and `case "provider":` blocks from the subcommand switch.

- [ ] **Step 2: Remove engine web setup callbacks**

Remove the block starting with `// Wire /web command callbacks` containing `engine.SetWebSetupFunc(...)` and `engine.SetWebStatusFunc(...)`.

- [ ] **Step 3: Remove bridge server startup block**

Remove the block starting with `// Start bridge server if enabled` through `bridgeSrv.Start()`.

- [ ] **Step 4: Remove webhook server startup block**

Remove the block starting with `// Start webhook server if enabled` through `webhookSrv.Start()`.

- [ ] **Step 5: Remove management server startup block**

Remove the block starting with `// Start management API server if enabled` through all `mgmtSrv.Set*` calls and `mgmtSrv.Start()`.

- [ ] **Step 6: Remove server shutdown code**

Remove the `mgmtSrv.Stop()`, `bridgeSrv.Stop()`, `webhookSrv.Stop()` blocks from the shutdown section.

- [ ] **Step 7: Update banner text**

Replace the multi-line support/platform listing in the startup banner with a single concise line.

- [ ] **Step 8: Verify no remaining references**

```bash
cd D:/WorkPlace/Tools/cc-connect
grep -n "mgmtSrv\|bridgeSrv\|webhookSrv\|runWeb\|runFeishu\|runRelay\|runProviderCommand\|SetWebSetupFunc\|SetWebStatusFunc" cmd/cc-connect/main.go
```
Expected: no output.

- [ ] **Step 9: Commit**

```bash
git add cmd/cc-connect/main.go
git commit -m "feat: remove bridge/webhook/management server wiring from main.go

Co-Authored-By: Claude Opus 4.7 <noreply@anthropic.com>"
```

---

### Task 9: Update embed.go with minimal config template

**Files:**
- Modify: `embed.go`

- [ ] **Step 1: Update embed.go**

The file `embed.go` at project root embeds `config.example.toml`. The content is embedded at compile time — no code change needed, just the TOML file change in Task 10. Verify embed.go:

```go
package ccconnect

import _ "embed"

//go:embed config.example.toml
var ConfigExampleTOML string
```

No changes needed — this stays as-is.

---

### Task 10: Replace config.example.toml with minimal template

**Files:**
- Modify: `config.example.toml`

- [ ] **Step 1: Write minimal config template**

Replace the entire content of `config.example.toml` with:

```toml
# cc-connect — Claude Code + WeChat
# Config location: ~/.cc-connect/config.toml
# Generate with: cc-connect setup

language = "zh-CN"
data_dir = "~/.cc-connect/data"

# Optional: configure a custom AI provider for Claude Code
# [[providers]]
# name = "my-key"
# api_key = "${ANTHROPIC_API_KEY}"
# agent_types = ["claudecode"]

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
# account_id = "default"

[log]
level = "info"
```

- [ ] **Step 2: Commit**

```bash
git add config.example.toml
git commit -m "feat: replace config.example.toml with minimal claudecode+weixin template

Co-Authored-By: Claude Opus 4.7 <noreply@anthropic.com>"
```

---

### Task 11: Clean up config/config.go

**Files:**
- Modify: `config/config.go`

Remove `EnableWebAdmin` function and `WebSetupResult` type, and the `orDefault` helper if only used by `EnableWebAdmin`.

- [ ] **Step 1: Remove WebSetupResult type and EnableWebAdmin function**

Delete lines containing `WebSetupResult` struct definition (around lines 3301-3308) and the entire `EnableWebAdmin` function (around lines 3310-3382).

- [ ] **Step 2: Remove orDefault if unused**

Check if `orDefault` is referenced elsewhere:

```bash
grep -n "orDefault" config/config.go
```

If only used in `EnableWebAdmin` (lines 3384-3387), delete it too.

- [ ] **Step 3: Commit**

```bash
git add config/config.go
git commit -m "feat: remove EnableWebAdmin and WebSetupResult from config

Co-Authored-By: Claude Opus 4.7 <noreply@anthropic.com>"
```

---

### Task 12: Clean up core/engine.go web-related fields and methods

**Files:**
- Modify: `core/engine.go`

Remove the `webSetupFunc` and `webStatusFunc` fields and their setters, plus the `/web` command handler that calls them. Keep `providerSaveFunc` — it's used for persisting provider changes via chat commands.

- [ ] **Step 1: Remove web callback fields**

Remove lines 268-271 (the fields):
```go
// /web command callbacks
webSetupFunc  func() (port int, token string, needRestart bool, err error)
webStatusFunc func() (url string)
```

- [ ] **Step 2: Remove SetWebSetupFunc and SetWebStatusFunc methods**

Remove lines 614-615:
```go
func (e *Engine) SetWebSetupFunc(fn func() (int, string, bool, error)) { e.webSetupFunc = fn }
func (e *Engine) SetWebStatusFunc(fn func() string)                    { e.webStatusFunc = fn }
```

- [ ] **Step 3: Remove /web command handler**

Find the `/web` command handler block (around lines 13636-13669) and remove the entire block that references `e.webSetupFunc` and `e.webStatusFunc`.

- [ ] **Step 4: Commit**

```bash
git add core/engine.go
git commit -m "feat: remove web setup/status callbacks from engine

Co-Authored-By: Claude Opus 4.7 <noreply@anthropic.com>"
```

---

### Task 13: Update Makefile

**Files:**
- Modify: `Makefile`

- [ ] **Step 1: Update Makefile variables and targets**

Make the following edits:

1. Change `ALL_AGENTS` (line 36):
```makefile
ALL_AGENTS := claudecode
```

2. Change `ALL_PLATFORMS` (line 37):
```makefile
ALL_PLATFORMS := weixin
```

3. Remove `ALL_EXTRAS` (line 38):
```makefile
ALL_EXTRAS :=
```
Or delete the line entirely.

4. Remove the `web:` target (lines 70-72):
```makefile
web:
	@if [ ! -d web/node_modules ]; then cd web && npm install; fi
	cd web && npm run build
```

5. Change `build:` target (line 74) to not depend on `web:`:
```makefile
build:
	go build $(_TAGS_FLAG) -ldflags "$(LDFLAGS)" -o $(APP) $(CMD)
```

6. Remove `build-noweb:` target (lines 77-78).

7. Remove `NO_WEB` handling (lines 61-63).

8. Update `test-release-local` (lines 134-138) to remove feishu-specific tests:
```makefile
test-release-local:
	go test ./tests/release_local/...
	go test ./config
	go test ./core -run 'TestEngineSendToSessionWithAttachments|TestProcessInteractiveEvents_SuppressesDuplicateSideChannelText|TestCmdList_AllSessionsVisibleAfterRepeatedNew|TestCmdList_SessionVisibleDuringAgentProcessing|TestEngine_Alias|TestEngine_BannedWords|TestEngine_DisabledCommands'
```

- [ ] **Step 2: Commit**

```bash
git add Makefile
git commit -m "feat: simplify Makefile for single-agent single-platform build

Co-Authored-By: Claude Opus 4.7 <noreply@anthropic.com>"
```

---

### Task 14: Clean up go.mod

**Files:**
- Modify: `go.mod`, `go.sum`

- [ ] **Step 1: Remove unused dependencies**

```bash
cd D:/WorkPlace/Tools/cc-connect
go mod tidy
```

This will remove all dependencies that were only used by the deleted packages (e.g., platform-specific SDKs).

- [ ] **Step 2: Verify build compiles**

```bash
go build ./...
```

Expected: successful compilation with zero errors.

- [ ] **Step 3: Commit**

```bash
git add go.mod go.sum
git commit -m "chore: tidy go.mod after removing unused packages

Co-Authored-By: Claude Opus 4.7 <noreply@anthropic.com>"
```

---

### Task 15: Create setup command

**Files:**
- Create: `cmd/cc-connect/setup.go`
- Modify: `cmd/cc-connect/main.go` (add `case "setup":` to subcommand switch)

- [ ] **Step 1: Create setup.go**

Create `cmd/cc-connect/setup.go` with the full setup implementation:

```go
package main

import (
	"bufio"
	"flag"
	"fmt"
	"os"
	"os/exec"
	"path/filepath"
	"runtime"
	"strings"

	"github.com/chenhg5/cc-connect/daemon"
)

func runSetup(args []string) {
	fs := flag.NewFlagSet("setup", flag.ExitOnError)
	token := fs.String("token", "", "WeChat iLink Bot token")
	allowFrom := fs.String("allow-from", "", "allowed WeChat user IDs (comma-separated, * for all)")
	accountID := fs.String("account-id", "default", "account label for multi-account isolation")
	claudePath := fs.String("claude-path", "", "path to claude binary")
	noDaemon := fs.Bool("no-daemon", false, "skip daemon installation")
	configPath := fs.String("config-path", "", "custom config file path")
	force := fs.Bool("force", false, "overwrite existing config without prompting")
	_ = fs.Parse(args)

	interactive := *token == ""

	fmt.Println(strings.Repeat("=", 52))
	fmt.Println("  cc-connect setup — Claude Code + WeChat")
	fmt.Println(strings.Repeat("=", 52))
	fmt.Println()

	// Step 1: Detect Claude Code
	fmt.Println("[1/4] Detecting Claude Code...")
	claudeBin := *claudePath
	if claudeBin == "" {
		claudeBin = detectClaudeCode()
	}
	if claudeBin == "" {
		if interactive {
			fmt.Print("Claude Code not found in PATH. Enter path to claude binary (or press Enter to skip): ")
			scanner := bufio.NewScanner(os.Stdin)
			if scanner.Scan() {
				claudeBin = strings.TrimSpace(scanner.Text())
			}
		}
	}
	if claudeBin != "" {
		ver := getClaudeVersion(claudeBin)
		fmt.Printf("  Found: %s (%s)\n", claudeBin, ver)
	} else {
		fmt.Println("  Skipped. Claude Code must be installed separately.")
	}
	fmt.Println()

	// Step 2: WeChat token
	fmt.Println("[2/4] Configuring WeChat connection...")
	botToken := *token
	if botToken == "" && interactive {
		fmt.Print("Enter WeChat iLink Bot token: ")
		scanner := bufio.NewScanner(os.Stdin)
		if scanner.Scan() {
			botToken = strings.TrimSpace(scanner.Text())
		}
	}
	if botToken == "" {
		fmt.Fprintln(os.Stderr, "Error: WeChat bot token is required. Use --token flag or enter interactively.")
		os.Exit(1)
	}
	fmt.Println("  Token configured.")
	fmt.Println()

	// Step 3: Generate config
	fmt.Println("[3/4] Generating configuration...")
	cfgDir := configDir()
	cfgFile := *configPath
	if cfgFile == "" {
		cfgFile = filepath.Join(cfgDir, "config.toml")
	}

	if _, err := os.Stat(cfgFile); err == nil && !*force {
		if interactive {
			fmt.Printf("Config file already exists at %s\n", cfgFile)
			fmt.Print("Overwrite? [y/N]: ")
			scanner := bufio.NewScanner(os.Stdin)
			if !scanner.Scan() || strings.ToLower(strings.TrimSpace(scanner.Text())) != "y" {
				fmt.Println("  Skipped config generation.")
				fmt.Println()
				return
			}
		}
	}

	if err := os.MkdirAll(cfgDir, 0755); err != nil {
		fmt.Fprintf(os.Stderr, "Error creating config directory: %v\n", err)
		os.Exit(1)
	}

	allowFromLine := ""
	if *allowFrom != "" {
		allowFromLine = fmt.Sprintf("\nallow_from = \"%s\"", *allowFrom)
	}

	claudeOpts := ""
	if claudeBin != "" {
		claudeOpts = fmt.Sprintf("\n[projects.agent.options]\nbinary_path = \"%s\"", claudeBin)
	}

	cfg := fmt.Sprintf(`# cc-connect — Claude Code + WeChat
# Generated by cc-connect setup

language = "zh-CN"
data_dir = "%s"

[[projects]]
name = "default"

[projects.agent]
type = "claudecode"%s

[[projects.platforms]]
type = "weixin"
[projects.platforms.options]
token = "%s"
account_id = "%s"%s

[log]
level = "info"
`, filepath.ToSlash(filepath.Join(cfgDir, "data")), claudeOpts, botToken, *accountID, allowFromLine)

	if err := os.WriteFile(cfgFile, []byte(cfg), 0644); err != nil {
		fmt.Fprintf(os.Stderr, "Error writing config: %v\n", err)
		os.Exit(1)
	}
	fmt.Printf("  Config written to %s\n", cfgFile)
	fmt.Println()

	// Step 4: Daemon installation
	fmt.Println("[4/4] System service...")
	if *noDaemon {
		fmt.Println("  Skipped (--no-daemon).")
		fmt.Println()
		printFinish(cfgFile)
		return
	}

	if interactive {
		fmt.Print("Install as system service (auto-start on login)? [Y/n]: ")
		scanner := bufio.NewScanner(os.Stdin)
		if scanner.Scan() {
			answer := strings.ToLower(strings.TrimSpace(scanner.Text()))
			if answer == "n" || answer == "no" {
				fmt.Println("  Skipped.")
				fmt.Println()
				printFinish(cfgFile)
				return
			}
		}
	}

	mgr, err := daemon.NewManager()
	if err != nil {
		fmt.Println("  Service management not supported on this platform.")
		fmt.Println()
		printFinish(cfgFile)
		return
	}

	dcfg := daemon.Config{
		WorkDir: cfgDir,
	}
	if err := daemon.Resolve(&dcfg); err != nil {
		fmt.Fprintf(os.Stderr, "  Error resolving daemon config: %v\n", err)
		os.Exit(1)
	}

	if err := mgr.Install(dcfg); err != nil {
		fmt.Fprintf(os.Stderr, "  Error installing service: %v\n", err)
	} else {
		if err := daemon.SaveMeta(&daemon.Meta{
			LogFile:     dcfg.LogFile,
			LogMaxSize:  dcfg.LogMaxSize,
			WorkDir:     dcfg.WorkDir,
			BinaryPath:  dcfg.BinaryPath,
			InstalledAt: daemon.NowISO(),
		}); err != nil {
			fmt.Fprintf(os.Stderr, "  Warning: failed to save metadata: %v\n", err)
		}
		fmt.Printf("  Service installed (%s).\n", mgr.Platform())
	}

	fmt.Println()
	printFinish(cfgFile)
}

func detectClaudeCode() string {
	// Check PATH first
	if p, err := exec.LookPath("claude"); err == nil {
		return p
	}
	// Common install paths
	home, _ := os.UserHomeDir()
	candidates := []string{}
	if runtime.GOOS == "windows" {
		candidates = []string{
			filepath.Join(home, "AppData", "Roaming", "npm", "claude.cmd"),
			filepath.Join(home, "AppData", "Local", "claude", "claude.exe"),
		}
	} else {
		candidates = []string{
			"/usr/local/bin/claude",
			filepath.Join(home, ".local", "bin", "claude"),
			filepath.Join(home, "bin", "claude"),
		}
	}
	for _, p := range candidates {
		if _, err := os.Stat(p); err == nil {
			return p
		}
	}
	return ""
}

func getClaudeVersion(path string) string {
	cmd := exec.Command(path, "--version")
	out, err := cmd.Output()
	if err != nil {
		return "unknown"
	}
	return strings.TrimSpace(string(out))
}

func configDir() string {
	if runtime.GOOS == "windows" {
		appData := os.Getenv("APPDATA")
		if appData == "" {
			home, _ := os.UserHomeDir()
			appData = filepath.Join(home, "AppData", "Roaming")
		}
		return filepath.Join(appData, ".cc-connect")
	}
	home, _ := os.UserHomeDir()
	return filepath.Join(home, ".cc-connect")
}

func printFinish(cfgFile string) {
	fmt.Println(strings.Repeat("=", 52))
	fmt.Println("  Setup complete!")
	fmt.Println()
	fmt.Printf("  Config: %s\n", cfgFile)
	fmt.Printf("  Start:  cc-connect --config %s\n", cfgFile)
	fmt.Println()
	fmt.Println("  Useful commands:")
	fmt.Println("    cc-connect daemon start        Start the service")
	fmt.Println("    cc-connect daemon stop         Stop the service")
	fmt.Println("    cc-connect daemon status       Check service status")
	fmt.Println("    cc-connect daemon logs         View logs")
	fmt.Println(strings.Repeat("=", 52))
}
```

- [ ] **Step 2: Wire setup subcommand into main.go**

In `cmd/cc-connect/main.go`, add to the subcommand switch in `main()`:

```go
case "setup":
    runSetup(os.Args[2:])
    return
```

Place it after `case "config-example":` and before `case "config":`.

- [ ] **Step 3: Verify compilation**

```bash
go build ./cmd/cc-connect/
```
Expected: successful compilation.

- [ ] **Step 5: Commit**

```bash
git add cmd/cc-connect/setup.go cmd/cc-connect/main.go
git commit -m "feat: add cc-connect setup command with interactive wizard

Co-Authored-By: Claude Opus 4.7 <noreply@anthropic.com>"
```

---

### Task 16: Final build verification

- [ ] **Step 1: Full build**

```bash
cd D:/WorkPlace/Tools/cc-connect
go build ./...
```
Expected: all packages compile without errors.

- [ ] **Step 2: Vet**

```bash
go vet ./...
```
Expected: no issues.

- [ ] **Step 3: Verify setup command runs**

```bash
go run ./cmd/cc-connect setup --help
```
Expected: setup flag usage displayed.

- [ ] **Step 4: Verify setup non-interactive mode**

```bash
go run ./cmd/cc-connect setup --token test123 --no-daemon --force
```
Expected: config generated at `~/.cc-connect/config.toml`.

- [ ] **Step 5: Verify config-example command works**

```bash
go run ./cmd/cc-connect config-example
```
Expected: minimal config template printed to stdout.

- [ ] **Step 6: Commit any remaining changes**

```bash
git status
git add -A
git commit -m "chore: final verification after minimal edition refactor

Co-Authored-By: Claude Opus 4.7 <noreply@anthropic.com>"
```

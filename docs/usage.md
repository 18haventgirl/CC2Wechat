# Usage Guide

CC2Wechat — Claude Code + WeChat usage guide.

## Basics

Send messages to your WeChat Bot. CC2Wechat forwards them to Claude Code and streams responses back in real time.

### Sessions

- Each WeChat user gets an independent session context
- Sessions auto-reset after idle timeout (configurable via `idle_timeout_mins`)
- Send `/new` or `/reset` to manually reset

### Permission Modes

```toml
[projects.agent.options]
mode = "default"   # default | accept-edits | yolo
```

| Mode | Description |
|------|-------------|
| `default` | Operations require confirmation |
| `accept-edits` | Auto-accept file edits |
| `yolo` | Full auto (use with caution) |

### Model Switching

```
/model claude-sonnet-4-6
/model claude-opus-4-7
```

### Working Directory

```
/dir /path/to/project
```

### Custom Provider

```toml
[[providers]]
name = "my-proxy"
api_key = "${ANTHROPIC_API_KEY}"
base_url = "https://your-proxy.com/v1"
agent_types = ["claudecode"]
```

## WeChat Features

- **Images**: Send images to the bot for Claude Code to analyze
- **Files**: Claude Code-generated files can be sent back via WeChat
- **Multi-account**: Use `account_id` to separate multiple bots

## Troubleshooting

### Claude Code not found

```bash
which claude
./cc-connect setup --claude-path /path/to/claude
```

### No response from WeChat

1. Check service status: `cc-connect daemon status`
2. Check logs: `cc-connect daemon logs -f`
3. Verify the token is valid
4. Confirm network access to `https://ilinkai.weixin.qq.com`

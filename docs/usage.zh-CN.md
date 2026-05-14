# 使用指南

CC2Wechat — Claude Code + 微信 使用指南。

## 基础使用

向微信 Bot 发送消息即可。CC2Wechat 会将消息转发给 Claude Code，并将回复实时返回微信。

### 会话管理

- 每个微信用户拥有独立的会话上下文
- 会话在空闲超时后自动重置（可配置 `idle_timeout_mins`）
- 发送 `/new` 或 `/reset` 可手动重置会话

### 权限模式

在配置中设置 Claude Code 的权限模式：

```toml
[projects.agent.options]
mode = "default"   # default | accept-edits | yolo
```

| 模式 | 说明 |
|------|------|
| `default` | 操作需确认 |
| `accept-edits` | 自动接受文件编辑 |
| `yolo` | 全部自动执行（谨慎使用） |

### 模型切换

通过微信发送命令切换模型：

```
/model claude-sonnet-4-6
/model claude-opus-4-7
```

### 工作目录

Claude Code 默认在配置的 `work_dir` 目录下工作。可通过 `/dir` 或 `/cd` 命令切换：

```
/dir /path/to/project
```

### Provider 配置

如需使用自定义 API 代理，在 `config.toml` 中添加：

```toml
[[providers]]
name = "my-proxy"
api_key = "${ANTHROPIC_API_KEY}"
base_url = "https://your-proxy.com/v1"
agent_types = ["claudecode"]
```

## 微信功能

### 图片与文件

- 发送图片给 Bot，Claude Code 可识别图片内容
- Claude Code 生成的文件可通过微信回传

### 多个微信账号

使用 `account_id` 区分不同微信 Bot：

```toml
[[projects.platforms]]
type = "weixin"
[projects.platforms.options]
token = "TOKEN_1"
account_id = "bot-1"

[[projects.platforms]]
type = "weixin"
[projects.platforms.options]
token = "TOKEN_2"
account_id = "bot-2"
```

## 微信 iLink Bot 获取

微信 iLink Bot 是微信官方的个人号机器人平台。获取 Token 的步骤：

1. 访问微信 iLink 平台注册
2. 创建 Bot 应用
3. 获取 Bearer Token
4. 将 Token 填入配置文件

## 故障排查

### Claude Code 未找到

```bash
# 检查 claude 是否在 PATH 中
which claude

# 或手动指定路径
./cc-connect setup --claude-path /path/to/claude
```

### 微信消息无响应

1. 检查 `cc-connect daemon status` 确认服务运行中
2. 查看日志：`cc-connect daemon logs -f`
3. 确认 Token 有效
4. 确认网络可访问 `https://ilinkai.weixin.qq.com`

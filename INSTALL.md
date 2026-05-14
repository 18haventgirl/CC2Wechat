# CC2Wechat 安装指南

## 环境要求

- **Go 1.25+**
- **Claude Code** — [安装指南](https://docs.anthropic.com/en/docs/claude-code/overview)
- **微信 iLink Bot Token** — 从微信 iLink 平台获取

## 安装

### 从源码编译

```bash
git clone https://github.com/18haventgirl/CC2Wechat.git
cd CC2Wechat
go build -o cc-connect ./cmd/cc-connect/
```

### 一键 Setup

```bash
./cc-connect setup
```

交互式引导完成：
1. 检测 Claude Code 安装
2. 配置微信 iLink Bot Token
3. 生成配置文件
4. 注册系统服务（可选）

### 非交互模式

```bash
./cc-connect setup \
  --token YOUR_ILINK_BOT_TOKEN \
  --claude-path /path/to/claude \
  --allow-from "*"
```

## 配置文件

Setup 自动生成 `~/.cc-connect/config.toml`（Windows: `%APPDATA%\.cc-connect\config.toml`）。

如需手动调整，参考以下完整选项：

```toml
language = "zh-CN"
data_dir = "~/.cc-connect/data"

[[projects]]
name = "default"

[projects.agent]
type = "claudecode"
[projects.agent.options]
binary_path = "/usr/local/bin/claude"   # Claude CLI 路径（可选）
model = "claude-sonnet-4-6"             # 模型选择（可选）

[[projects.platforms]]
type = "weixin"
[projects.platforms.options]
token = "YOUR_TOKEN"                    # 必填：iLink Bot Token
allow_from = "*"                        # 允许的用户 ID，* 表示全部
account_id = "default"                  # 多账号隔离标签

[log]
level = "info"                          # debug / info / warn / error
```

## 系统服务

```bash
# 安装（开机自启）
cc-connect daemon install

# 启动/停止/重启
cc-connect daemon start
cc-connect daemon stop
cc-connect daemon restart

# 查看状态
cc-connect daemon status

# 查看日志
cc-connect daemon logs -f
```

| 平台 | 服务类型 |
|------|----------|
| Windows | Task Scheduler (schtasks) |
| macOS | LaunchAgent |
| Linux | systemd (user) |

## 验证

```bash
# 前台运行测试
./cc-connect

# 看到启动 banner 即表示成功
# 向微信 Bot 发送消息，Claude Code 会开始响应
```

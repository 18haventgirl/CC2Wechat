# CC2Wechat — Claude Code 连接微信

<p align="center">
  <strong>在微信里使用 Claude Code 进行 AI 编程</strong>
</p>

<p align="center">
  <a href="./README.md">English</a> | <a href="./README.zh-CN.md">中文</a>
</p>

CC2Wechat 是一个轻量桥接工具，将 **Claude Code**（AI 编程 Agent）与**个人微信**（iLink Bot）连接起来。通过微信发送消息，Claude Code 在本地执行编程任务，结果实时返回微信。

基于 [cc-connect](https://github.com/chenhg5/cc-connect) 精简而来。

## 特性

- **仅 Claude Code + 微信** — 专注、轻量、零冗余
- **一键 Setup** — `cc-connect setup` 自动检测环境、生成配置、注册系统服务
- **跨平台** — Windows / macOS / Linux 全支持
- **后台运行** — 注册为系统服务，开机自启
- **多会话管理** — 支持多项目工作目录、独立会话上下文

## 快速开始

### 1. 安装

```bash
# 克隆项目
git clone https://github.com/18haventgirl/CC2Wechat.git
cd CC2Wechat

# 编译
go build -o cc-connect ./cmd/cc-connect/
```

### 2. 配置

```bash
# 交互式向导（推荐）
./cc-connect setup

# 或非交互模式
./cc-connect setup --token YOUR_ILINK_BOT_TOKEN --no-daemon
```

Setup 会引导你完成：
1. 检测 Claude Code 安装位置
2. 配置微信 iLink Bot Token
3. 生成最小 `config.toml`
4. 注册为系统服务（可选）

### 3. 运行

```bash
# 前台运行
./cc-connect

# 后台服务
./cc-connect daemon start
./cc-connect daemon stop
./cc-connect daemon status
```

## 配置说明

配置文件位于 `~/.cc-connect/config.toml`（Windows: `%APPDATA%\.cc-connect\config.toml`）：

```toml
language = "zh-CN"
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

### 微信 iLink Bot 配置项

| 配置项 | 必填 | 说明 |
|--------|------|------|
| `token` | 是 | iLink Bot Bearer Token |
| `allow_from` | 否 | 允许的用户 ID 列表（逗号分隔），`"*"` 表示全部允许 |
| `account_id` | 否 | 多账号隔离标签，默认 `"default"` |

### Claude Code 配置项

| 配置项 | 必填 | 说明 |
|--------|------|------|
| `binary_path` | 否 | Claude CLI 路径，默认从 PATH 检测 |
| `model` | 否 | 模型选择，如 `claude-sonnet-4-6` |

## 系统服务管理

```bash
cc-connect daemon install    # 安装服务（Windows: 任务计划程序, macOS: LaunchAgent, Linux: systemd）
cc-connect daemon uninstall  # 卸载服务
cc-connect daemon start      # 启动
cc-connect daemon stop       # 停止
cc-connect daemon restart    # 重启
cc-connect daemon status     # 状态
cc-connect daemon logs -f    # 实时日志
```

## 开发

```bash
# 编译
make build

# 测试
make test

# 交叉编译
make release-all
```

## 许可证

MIT License

## 致谢

本项目基于 [cc-connect](https://github.com/chenhg5/cc-connect) 精简而来，感谢原作者的出色工作。

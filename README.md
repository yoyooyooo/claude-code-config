# CCC (Claude Code Config) - Claude 配置管理工具

> 🚀 **简洁、安全、智能**的 Claude Code 配置切换工具

CCC 是一个轻量级的命令行工具，专为 Claude Code 用户设计，让你可以在不同的 API 配置间快速切换，支持多个服务商和环境配置。

## ✨ 核心特性

- 🎯 **智能检测** - 自动识别当前配置状态，无需手动同步
- 🔒 **安全可靠** - 自动备份配置文件，原子性操作保证数据安全
- 📁 **多配置管理** - 支持添加、删除、切换多个配置档案
- 🎨 **用户友好** - 彩色输出，清晰的状态提示
- 🔐 **隐私保护** - 敏感信息在显示时自动脱敏
- ⚡ **轻量高效** - 纯 Shell 脚本，无额外依赖

## 📋 系统要求

- **操作系统**: macOS / Linux
- **依赖工具**: `jq` (JSON 处理工具)
- **前置条件**: Claude Code 已安装并配置

## 🛠 快速安装

### 方法一：一键安装脚本

```bash
# 下载并安装
curl -fsSL https://raw.githubusercontent.com/yoyooyooo/claude-code-config/refs/heads/main/ccc | bash
```

### 方法二：手动安装

```bash
# 下载脚本
curl -o ccc https://raw.githubusercontent.com/yoyooyooo/claude-code-config/refs/heads/main/ccc
chmod +x ccc

# 安装到系统路径
sudo mv ccc /usr/local/bin/ccc

# 或使用脚本自带的安装功能
./ccc install
```

### 安装依赖

```bash
# macOS
brew install jq

# Ubuntu/Debian
sudo apt-get install jq

# CentOS/RHEL/Fedora
sudo yum install jq
```

## 🎯 快速开始

### 1️⃣ 初始化

```bash
ccc init
```

### 2️⃣ 添加配置档案

**方式一：交互式添加**

```bash
ccc add work
# 按提示输入 Base URL、API Key 等信息
```

**方式二：从当前配置导入**

```bash
# 先手动配置好 ~/.claude/settings.json
ccc import default
```

### 3️⃣ 查看配置状态

```bash
ccc list
```

输出示例：

```
=== Claude Code 配置管理 ===

当前活跃配置: work (智能检测)
  API Key Helper: [已配置]
  Base URL: https://api.example.com
  API Key: sk-***[已配置]

可用配置档案:
  default
  * work (当前)
```

### 4️⃣ 切换配置

```bash
ccc use default
ccc use work
```

## 📖 命令详解

### 基础命令

| 命令       | 说明                   | 示例       |
| ---------- | ---------------------- | ---------- |
| `ccc init` | 初始化配置管理         | `ccc init` |
| `ccc list` | 查看所有配置和当前状态 | `ccc list` |
| `ccc help` | 显示帮助信息           | `ccc help` |

### 配置管理

| 命令                | 说明               | 示例                 |
| ------------------- | ------------------ | -------------------- |
| `ccc add <name>`    | 交互式添加新配置   | `ccc add kimi`       |
| `ccc import <name>` | 从当前设置导入配置 | `ccc import current` |
| `ccc use <name>`    | 切换到指定配置     | `ccc use work`       |
| `ccc rm <name>`     | 删除配置档案       | `ccc rm old-config`  |

### 安装相关

| 命令          | 说明           | 示例          |
| ------------- | -------------- | ------------- |
| `ccc install` | 安装到系统路径 | `ccc install` |

## 🏗 配置文件结构

### 主配置文件

CCC 的配置存储在 `~/.claude/ccc-config.json`：

```json
{
  "profiles": {
    "default": {
      "apiKeyHelper": "echo 'sk-xxx'",
      "env": {
        "ANTHROPIC_BASE_URL": "https://api.anthropic.com",
        "ANTHROPIC_API_KEY": "sk-xxx"
      }
    },
    "work": {
      "env": {
        "ANTHROPIC_BASE_URL": "https://api.work.com",
        "ANTHROPIC_API_KEY": "sk-yyy"
      }
    }
  },
  "current": "default"
}
```

### 管理的字段

CCC 只修改 `~/.claude/settings.json` 中的以下字段：

- `apiKeyHelper` - API 密钥助手命令
- `env.ANTHROPIC_API_KEY` - API 密钥
- `env.ANTHROPIC_BASE_URL` - API 基础 URL

**其他字段**（如 `permissions`、`includeCoAuthoredBy` 等）**完全不受影响**。

## 🔥 高级特性

### 智能配置检测

CCC 会自动检测当前 `settings.json` 的配置状态：

- ✅ **配置匹配某个档案** → 显示活跃配置名称
- ⚠️ **配置存在但未保存** → 提示保存为档案
- ❌ **无有效配置** → 提示添加配置

### 自动备份机制

每次切换配置时，CCC 会自动备份当前配置：

```
~/.claude/settings.json.backup.20240115_143022
```

### 安全特性

- 🔒 **原子操作** - 使用临时文件确保操作完整性
- 📋 **JSON 验证** - 修改后验证格式正确性
- 🛡️ **最小修改** - 只修改必要字段，保持其他配置不变
- 🔐 **信息脱敏** - 显示时自动隐藏敏感信息

## 🎨 使用场景

### 多环境开发

```bash
# 开发环境
ccc add dev

# 生产环境
ccc add prod

# 测试环境
ccc add test

# 快速切换
ccc use dev    # 开发时使用
ccc use prod   # 部署时使用
```

### 多服务商 API

```bash
# OpenAI 官方
ccc add openai

# 国内代理服务
ccc add proxy

# 本地 API
ccc add local

# 根据需要切换
ccc use proxy  # 日常使用代理
ccc use openai # 重要任务使用官方
```

## 🐛 故障排除

### 常见问题

**❓ 提示 `jq: command not found`**

```bash
# 安装 jq 工具
brew install jq        # macOS
sudo apt install jq    # Ubuntu
sudo yum install jq     # CentOS
```

**❓ 提示权限不足**

```bash
# 使用管理员权限安装
sudo cp ccc /usr/local/bin/ccc
sudo chmod +x /usr/local/bin/ccc
```

**❓ Claude 配置目录不存在**

```
请先安装并运行 Claude Code，确保 ~/.claude/settings.json 文件存在
```

**❓ 配置检测不准确**

```bash
# 手动同步配置状态
ccc list  # 智能检测会自动修正状态
```

### 调试信息

如需调试，可检查以下文件：

- **Claude 主配置**: `~/.claude/settings.json`
- **CCC 配置**: `~/.claude/ccc-config.json`
- **备份文件**: `~/.claude/settings.json.backup.*`

## 🤝 贡献指南

欢迎贡献代码！请遵循以下步骤：

1. Fork 本仓库
2. 创建功能分支：`git checkout -b feature/amazing-feature`
3. 提交变更：`git commit -m 'Add some amazing feature'`
4. 推送分支：`git push origin feature/amazing-feature`
5. 提交 Pull Request

### 开发环境

```bash
# 克隆仓库
git clone https://github.com/yoyooyooo/ccc.git
cd ccc

# 直接运行测试
./ccc help
```

---
name: ssh-remote-control
description: SSH远程控制电脑 - 让AI Agent通过SSH连接和操作远程Mac/Linux电脑，无需在被控电脑上安装任何agent工具 / Enable AI agents to remotely control computers via SSH without installing any agent software on the controlled device.
metadata: {"openclaw": {"homepage": "https://github.com/lixiang92229/skill-ssh-remote-control"}}
---

# SSH Remote Control Skill
# SSH 远程控制技能

让 AI Agent 通过 SSH 远程连接和控制电脑，无需在被控电脑上安装任何 agent 工具。

Enable AI agents to remotely connect and control computers via SSH, without installing any agent software on the controlled device.

---

## 概述 | Overview

**为什么需要这个 Skill？ | Why this Skill?**

传统的 AI Agent 控制电脑的方式是在本地安装 agent 软件。本 Skill 反其道而行——让 AI Agent 在服务器上运行，通过 SSH 远程控制任何有 SSH 接口的设备。

Traditional AI agents require installing software locally. This skill takes a different approach - running the AI agent on a server and controlling any device with SSH access remotely.

**核心优势 | Core Advantages:**

- **远程控制 | Remote Control**: Agent 在云端，触手伸向多台设备
- **零安装 | Zero Install**: 被控电脑只需开启 SSH，无需安装任何 agent
- **跨平台 | Cross-Platform**: 支持 macOS、Linux
- **CLI 优先 | CLI First**: 对 AI 友好的操作方式，Token 消耗低
- **安全可控 | Secure & Controllable**: 本地可随时关闭穿透工具

---

## 环境变量 | Environment Variables

| 变量名 | 必填 | 说明 |
|--------|------|------|
| `SSH_TARGET_HOST` | 是 | 远程电脑的公网地址或域名 / Remote computer's public address or domain |
| `SSH_TARGET_PORT` | 是 | SSH 端口（默认22）/ SSH port (default 22) |
| `SSH_TARGET_USER` | 是 | 远程电脑用户名 / Remote computer username |
| `SSH_KEY_PATH` | 是 | 本地私钥路径 / Local private key path |

---

## 快速开始 | Quick Start

### 1. 配置环境变量 | Configure Environment Variables

```bash
export SSH_TARGET_HOST="your-tunnel-address.example.com"
export SSH_TARGET_PORT="12345"
export SSH_TARGET_USER="your-username"
export SSH_KEY_PATH="/path/to/private/key"
```

### 2. 测试连接 | Test Connection

```bash
ssh -i $SSH_KEY_PATH -p $SSH_TARGET_PORT $SSH_TARGET_USER@$SSH_TARGET_HOST 'echo "OK"'
```

### 3. 让 AI 操作 | Let AI Operate

告诉 AI: "帮我打开远程电脑上的 Safari 浏览器"

AI 会执行:
```bash
ssh -i $SSH_KEY_PATH -p $SSH_TARGET_PORT $SSH_TARGET_USER@$SSH_TARGET_HOST 'open -a Safari'
```

---

## 功能示例 | Feature Examples

### 文件操作 | File Operations

```bash
# 查看目录 | List directory
ssh -i $SSH_KEY_PATH -p $SSH_TARGET_PORT $SSH_TARGET_USER@$SSH_TARGET_HOST 'ls -la ~/Desktop/'

# 创建文件 | Create file
ssh -i $SSH_KEY_PATH -p $SSH_TARGET_PORT $SSH_TARGET_USER@$SSH_TARGET_HOST 'cat > ~/Desktop/ai.txt << '"'"'EOF'"'"'
内容 | Content
EOF'

# 上传文件 | Upload file
scp -i $SSH_KEY_PATH -P $SSH_TARGET_PORT localfile $SSH_TARGET_USER@$SSH_TARGET_HOST:/path/
```

### 软件控制 (macOS) | Software Control

```bash
# 打开应用 | Open application
ssh -i $SSH_KEY_PATH -p $SSH_TARGET_PORT $SSH_TARGET_USER@$SSH_TARGET_HOST 'open -a "Safari"'

# 关闭应用 | Quit application
ssh -i $SSH_KEY_PATH -p $SSH_TARGET_PORT $SSH_TARGET_USER@$SSH_TARGET_HOST 'osascript -e '"'"'quit application "Safari"'"'"''

# 查看运行程序 | List running processes
ssh -i $SSH_KEY_PATH -p $SSH_TARGET_PORT $SSH_TARGET_USER@$SSH_TARGET_HOST 'osascript -e '"'"'tell application "System Events" to get name of every process'"'"''
```

### 系统监控 | System Monitoring

```bash
# 系统版本 | System version
ssh -i $SSH_KEY_PATH -p $SSH_TARGET_PORT $SSH_TARGET_USER@$SSH_TARGET_HOST 'sw_vers'

# 资源使用 | Resource usage
ssh -i $SSH_KEY_PATH -p $SSH_TARGET_PORT $SSH_TARGET_USER@$SSH_TARGET_HOST 'top -l 1 | head -10'

# 磁盘空间 | Disk space
ssh -i $SSH_KEY_PATH -p $SSH_TARGET_PORT $SSH_TARGET_USER@$SSH_TARGET_HOST 'df -h'
```

---

## 被控电脑配置 | Controlled Computer Setup

### macOS 配置步骤 | macOS Setup Steps

1. **开启远程登录 | Enable Remote Login**
   - 系统偏好设置 → 共享 → 勾选"远程登录"
   - System Preferences → Sharing → Enable "Remote Login"

2. **生成SSH密钥对 | Generate SSH Key Pair**
   ```bash
   ssh-keygen -t ed25519 -C "ai@server"
   ```

3. **添加公钥 | Add Public Key**
   ```bash
   echo "公钥内容 | public-key" >> ~/.ssh/authorized_keys
   chmod 700 ~/.ssh
   chmod 600 ~/.ssh/authorized_keys
   ```

4. **配置内网穿透 | Configure Tunnel**

   大多数家庭/办公网络没有公网IP，需要内网穿透工具将内网端口映射到公网。

   Most home/office networks don't have public IPs, requiring tunnel tools to expose the local port.

   **常用工具 | Common Tools:**

   | 工具 | 特点 | 官网 |
   |------|------|------|
   | 花生壳 | 国内老牌，稳定 | oray.com |
   | ngrok | 国际流行，配置简单 | ngrok.com |
   | frp | 开源免费，可自建服务器 | github.com/fatedier/frp |
   | NATAPP | 国内服务 | natapp.cn |

---

## 安全说明 | Security Notes

本技能的安全性完全由你掌控 | Security is fully under your control:

- **本地可控 | Local Control**: 穿透工具在你本地运行，可随时关闭 / Tunnel runs locally, can be turned off anytime
- **按需连接 | On-demand**: 需要时开启，不需要时关闭 / Enable when needed, disable when not
- **密钥认证 | Key Auth**: 即使端口暴露，没有正确密钥无法登录 / Without correct key, cannot login even if port is exposed
- **关闭即安全 | Disabled = Secure**: 关闭穿透后，任何人无法访问 / After disabling, no one can access

**建议 | Recommendations:**
- 仅在需要AI远程控制时才开启穿透 / Enable tunnel only when AI remote control is needed
- 使用完毕立即关闭 / Disable immediately after use
- 建议配合SSH密钥认证，禁用密码登录 / Use SSH key auth, disable password login
- 定期更换SSH密钥 / Regularly rotate SSH keys

---

## 版本历史 | Changelog

- 1.0.3 (2026-03-31): 修复GitHub链接 / Fix GitHub link
- 1.0.2 (2026-03-31): 添加项目链接 / Add project links
- 1.0.1 (2026-03-31): 强调安全性，通用内网穿透工具 / Emphasize security, generic tunnel tools
- 1.0.0 (2026-03-30): 初始版本 / Initial release

---

## 项目地址 | Project Links

- **ClawHub**: https://clawhub.com/skill/ssh-remote-control
- **GitHub**: https://github.com/lixiang92229/skill-ssh-remote-control

---

## 许可 | License

MIT License

---

## 关于 | About

本技能由 Miss M 开发，用于探索 AI Agent 远程控制电脑的无限可能。

Developed by Miss M to explore the endless possibilities of AI agents remotely controlling computers.

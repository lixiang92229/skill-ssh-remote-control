# SSH Remote Control Skill for OpenClaw

[English](#english) | [中文](#中文)

---

## English

### Overview

This is an **OpenClaw Skill** for enabling AI agents to remotely connect and control computers via SSH — **without installing any agent software on the controlled device**.

Traditional AI agents require installing software locally. This skill takes a different approach — running the AI agent on a server and controlling any device with SSH access remotely.

### Core Advantages

- **Remote Control**: AI agent runs on server,触手伸向多台设备
- **Zero Install**: Controlled device only needs SSH enabled, no agent software needed
- **Cross-Platform**: Supports macOS, Linux
- **CLI First**: AI-friendly commands, low Token consumption
- **Secure & Controllable**: Local tunnel can be disabled anytime

### Architecture

```
┌─────────────┐     SSH      ┌─────────────────┐
│   AI Agent │ ──────────> │  Remote Computer  │
│  (Server) │   Encrypted  │  (Mac/Linux)      │
│             │ <──────────  │                   │
│  Execute    │   Return    │  No agent needed │
└─────────────┘             └─────────────────┘
```

### Requirements

- SSH server enabled on remote device
- Tunnel tool (花生壳/ngrok/frp/etc.) if device is behind NAT
- SSH key-based authentication

### Quick Start

1. **Configure environment variables**:
```bash
export SSH_TARGET_HOST="your-tunnel-address.example.com"
export SSH_TARGET_PORT="12345"
export SSH_TARGET_USER="your-username"
export SSH_KEY_PATH="/path/to/private/key"
```

2. **Test connection**:
```bash
ssh -i $SSH_KEY_PATH -p $SSH_TARGET_PORT $SSH_TARGET_USER@$SSH_TARGET_HOST 'echo "OK"'
```

3. **Let AI operate**:
Tell AI: "Open Safari on the remote computer"

AI will execute:
```bash
ssh -i $SSH_KEY_PATH -p $SSH_TARGET_PORT $SSH_TARGET_USER@$SSH_TARGET_HOST 'open -a Safari'
```

### Feature Examples

**File Operations**:
```bash
# List directory
ssh -i $SSH_KEY_PATH -p $SSH_TARGET_PORT $SSH_TARGET_USER@$SSH_TARGET_HOST 'ls -la ~/Desktop/'

# Create file
ssh -i $SSH_KEY_PATH -p $SSH_TARGET_PORT $SSH_TARGET_USER@$SSH_TARGET_HOST 'cat > ~/Desktop/ai.txt << EOF
Content
EOF'

# Upload file
scp -i $SSH_KEY_PATH -P $SSH_TARGET_PORT localfile $SSH_TARGET_USER@$SSH_TARGET_HOST:/path/
```

**Software Control (macOS)**:
```bash
# Open application
ssh -i $SSH_KEY_PATH -p $SSH_TARGET_PORT $SSH_TARGET_USER@$SSH_TARGET_HOST 'open -a "Safari"'

# Quit application
ssh -i $SSH_KEY_PATH -p $SSH_TARGET_PORT $SSH_TARGET_USER@$SSH_TARGET_HOST 'osascript -e "quit application \"Safari\""'

# List running processes
ssh -i $SSH_KEY_PATH -p $SSH_TARGET_PORT $SSH_TARGET_USER@$SSH_TARGET_HOST 'osascript -e "tell application \"System Events\" to get name of every process"'
```

**System Monitoring**:
```bash
# System version
ssh -i $SSH_KEY_PATH -p $SSH_TARGET_PORT $SSH_TARGET_USER@$SSH_TARGET_HOST 'sw_vers'

# Resource usage
ssh -i $SSH_KEY_PATH -p $SSH_TARGET_PORT $SSH_TARGET_USER@$SSH_TARGET_HOST 'top -l 1 | head -10'

# Disk space
ssh -i $SSH_KEY_PATH -p $SSH_TARGET_PORT $SSH_TARGET_USER@$SSH_TARGET_HOST 'df -h'
```

### Remote Device Setup (macOS)

1. **Enable Remote Login**: System Preferences → Sharing → Enable "Remote Login"

2. **Generate SSH Key Pair**:
```bash
ssh-keygen -t ed25519 -C "ai@server"
```

3. **Add Public Key**:
```bash
echo "public-key" >> ~/.ssh/authorized_keys
chmod 700 ~/.ssh
chmod 600 ~/.ssh/authorized_keys
```

4. **Configure Tunnel** (if behind NAT):

| Tool | Features | Website |
|------|----------|---------|
| 花生壳 | Stable, SSH support | oray.com |
| ngrok | Popular, easy setup | ngrok.com |
| frp | Open source, self-hosted | github.com/fatedier/frp |
| NATAPP | China-based service | natapp.cn |

### Security Notes

Security is fully under your control:

- **Local Control**: Tunnel runs locally, can be turned off anytime
- **On-demand**: Enable when needed, disable when not
- **Key Auth**: Without correct key, cannot login even if port is exposed
- **Disabled = Secure**: After disabling tunnel, no one can access

**Recommendations**:
- Enable tunnel only when AI remote control is needed
- Disable immediately after use
- Use SSH key auth, disable password login
- Regularly rotate SSH keys

### Security Warnings

⚠️ **Remote access inherently carries risk**:
- Only enable SSH and tunnel when needed
- Use strong SSH keys (ed25519 minimum)
- Monitor access logs regularly
- Keep your tunnel credentials secure
- Consider using VPN for additional security layer

### Version History

- v1.0.4 (2026-03-31): Rewrite README in bilingual format
- v1.0.3 (2026-03-31): Fix GitHub link
- v1.0.2 (2026-03-31): Add project links
- v1.0.1 (2026-03-31): Emphasize security, generic tunnel tools
- v1.0.0 (2026-03-30): Initial release

### Project Links

- **ClawHub**: https://clawhub.com/skill/ssh-remote-control
- **GitHub**: https://github.com/lixiang92229/skill-ssh-remote-control

### License

MIT License

---

## 中文

### 概述

这是面向 **OpenClaw** 的 **SSH 远程控制 Skill** — 让 AI Agent 通过 SSH 远程连接和控制电脑，**无需在被控电脑上安装任何 agent 软件**。

传统 AI Agent 需要在本地安装软件才能控制电脑。本技能反其道而行——让 AI Agent 在服务器上运行，通过 SSH 远程控制任何有 SSH 接口的设备。

### 核心优势

- **远程控制**：AI Agent 在云端，触手伸向多台设备
- **零安装**：被控电脑只需开启 SSH，无需安装任何 agent
- **跨平台**：支持 macOS、Linux
- **CLI 优先**：对 AI 友好的操作方式，Token 消耗低
- **安全可控**：本地可随时关闭穿透工具

### 技术架构

```
┌─────────────┐     SSH      ┌─────────────────┐
│   AI Agent │ ──────────> │  远程电脑        │
│   (服务器)  │   加密隧道   │   (Mac/Linux)    │
│             │ <──────────  │                 │
│   执行命令   │   返回结果   │   无需安装agent │
└─────────────┘             └─────────────────┘
```

### 环境要求

- 远程设备开启 SSH 服务
- 内网穿透工具（如果设备在内网）
- SSH 密钥认证

### 快速开始

1. **配置环境变量**：
```bash
export SSH_TARGET_HOST="your-tunnel-address.example.com"
export SSH_TARGET_PORT="12345"
export SSH_TARGET_USER="your-username"
export SSH_KEY_PATH="/path/to/private/key"
```

2. **测试连接**：
```bash
ssh -i $SSH_KEY_PATH -p $SSH_TARGET_PORT $SSH_TARGET_USER@$SSH_TARGET_HOST 'echo "OK"'
```

3. **让 AI 操作**：
告诉 AI："帮我打开远程电脑上的 Safari"

AI 会执行：
```bash
ssh -i $SSH_KEY_PATH -p $SSH_TARGET_PORT $SSH_TARGET_USER@$SSH_TARGET_HOST 'open -a Safari'
```

### 功能示例

**文件操作**：
```bash
# 查看目录
ssh -i $SSH_KEY_PATH -p $SSH_TARGET_PORT $SSH_TARGET_USER@$SSH_TARGET_HOST 'ls -la ~/Desktop/'

# 创建文件
ssh -i $SSH_KEY_PATH -p $SSH_TARGET_PORT $SSH_TARGET_USER@$SSH_TARGET_HOST 'cat > ~/Desktop/ai.txt << EOF
内容
EOF'

# 上传文件
scp -i $SSH_KEY_PATH -P $SSH_TARGET_PORT localfile $SSH_TARGET_USER@$SSH_TARGET_HOST:/path/
```

**软件控制 (macOS)**：
```bash
# 打开应用
ssh -i $SSH_KEY_PATH -p $SSH_TARGET_PORT $SSH_TARGET_USER@$SSH_TARGET_HOST 'open -a "Safari"'

# 关闭应用
ssh -i $SSH_KEY_PATH -p $SSH_TARGET_PORT $SSH_TARGET_USER@$SSH_TARGET_HOST 'osascript -e "quit application \"Safari\""'

# 查看运行中的程序
ssh -i $SSH_KEY_PATH -p $SSH_TARGET_PORT $SSH_TARGET_USER@$SSH_TARGET_HOST 'osascript -e "tell application \"System Events\" to get name of every process"'
```

**系统监控**：
```bash
# 系统版本
ssh -i $SSH_KEY_PATH -p $SSH_TARGET_PORT $SSH_TARGET_USER@$SSH_TARGET_HOST 'sw_vers'

# 资源使用
ssh -i $SSH_KEY_PATH -p $SSH_TARGET_PORT $SSH_TARGET_USER@$SSH_TARGET_HOST 'top -l 1 | head -10'

# 磁盘空间
ssh -i $SSH_KEY_PATH -p $SSH_TARGET_PORT $SSH_TARGET_USER@$SSH_TARGET_HOST 'df -h'
```

### 被控电脑配置 (macOS)

1. **开启远程登录**：系统偏好设置 → 共享 → 勾选"远程登录"

2. **生成 SSH 密钥对**：
```bash
ssh-keygen -t ed25519 -C "ai@server"
```

3. **添加公钥**：
```bash
echo "公钥内容" >> ~/.ssh/authorized_keys
chmod 700 ~/.ssh
chmod 600 ~/.ssh/authorized_keys
```

4. **配置内网穿透**（如果设备在内网）：

大多数家庭/办公网络没有公网 IP，需要内网穿透工具将内网端口映射到公网。

| 工具 | 特点 | 官网 |
|------|------|------|
| 花生壳 | 国内老牌，稳定 | oray.com |
| ngrok | 国际流行，配置简单 | ngrok.com |
| frp | 开源免费，可自建服务器 | github.com/fatedier/frp |
| NATAPP | 国内服务 | natapp.cn |

### 安全说明

安全性完全由你掌控：

- **本地可控**：穿透工具在你本地运行，可随时关闭
- **按需连接**：需要时开启，不需要时关闭
- **密钥认证**：即使端口暴露，没有正确密钥无法登录
- **关闭即安全**：关闭穿透后，任何人无法访问

**安全建议**：
- 仅在需要 AI 远程控制时才开启穿透
- 使用完毕立即关闭
- 建议配合 SSH 密钥认证，禁用密码登录
- 定期更换 SSH 密钥

### 安全风险提示

⚠️ **远程访问本身存在风险**：
- 仅在需要时开启 SSH 和穿透
- 使用强 SSH 密钥（ed25519 及以上）
- 定期查看登录日志
- 妥善保管穿透凭证
- 考虑使用 VPN 作为额外安全层

### 版本历史

- v1.0.4 (2026-03-31)：重写为整段中英双语格式
- v1.0.3 (2026-03-31)：修复 GitHub 链接
- v1.0.2 (2026-03-31)：添加项目链接
- v1.0.1 (2026-03-31)：强调安全性，通用内网穿透工具
- v1.0.0 (2026-03-30)：初始版本

### 项目地址

- **ClawHub**：https://clawhub.com/skill/ssh-remote-control
- **GitHub**：https://github.com/lixiang92229/skill-ssh-remote-control

### 开源协议

MIT License

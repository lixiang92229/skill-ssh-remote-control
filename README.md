# SSH Remote Control Skill

让 AI Agent 通过 SSH 远程连接和控制电脑，无需在被控电脑上安装任何 agent 工具。

## 为什么需要这个 Skill？

传统的 AI Agent 控制电脑的方式是在本地安装 agent 软件。本 Skill 反其道而行——让 AI Agent 在服务器上运行，通过 SSH 远程控制任何有 SSH 接口的设备。

## 核心特性

- **远程控制**: Agent 在云端，触手伸向多台设备
- **零安装**: 被控电脑只需开启 SSH，无需安装任何 agent
- **跨平台**: 支持 macOS、Linux
- **CLI 优先**: 对 AI 友好的操作方式，Token 消耗低

## 快速开始

### 1. 配置环境变量

```bash
export SSH_TARGET_HOST="your-mac.51mypc.cn"
export SSH_TARGET_PORT="43899"
export SSH_TARGET_USER="lixiang"
export SSH_KEY_PATH="/path/to/private/key"
```

### 2. 测试连接

```bash
ssh -i $SSH_KEY_PATH -p $SSH_TARGET_PORT $SSH_TARGET_USER@$SSH_TARGET_HOST 'echo "OK"'
```

### 3. 让 AI 操作

告诉 AI: "帮我打开远程电脑上的 Safari 浏览器"

AI 会执行:
```bash
ssh -i $SSH_KEY_PATH -p $SSH_TARGET_PORT $SSH_TARGET_USER@$SSH_TARGET_HOST 'open -a Safari'
```

## 项目结构

```
ssh-remote-control/
├── _meta.json          # ClawHub元数据
├── SKILL.md           # 技能文档
└── README.md          # 本文件
```

## 许可

MIT License

## 关于

本技能由 Miss M 开发，用于探索 AI Agent 远程控制电脑的无限可能。

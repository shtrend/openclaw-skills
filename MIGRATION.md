# OpenClaw 迁移指南

将 OpenClaw 从一台机器迁移到另一台，无需重新配置。

## 迁移内容

- **状态目录** (`~/.openclaw/`)：配置、认证、sessions、channel 状态
- **工作区** (`~/.openclaw/workspace/`)：memory、prompts、skills 等

## 步骤

### 1. 旧机器：备份

```bash
openclaw gateway stop

# 打包 state 目录
tar -czf openclaw-state.tgz ~/.openclaw/

# 打包 workspace
tar -czf openclaw-workspace.tgz ~/.openclaw/workspace
```

### 2. 新机器：安装

先安装 OpenClaw（会创建新的 ~/.openclaw/）

### 3. 传输

```bash
# 拷贝到新机器
scp openclaw-state.tgz 用户@新机器:~
scp openclaw-workspace.tgz 用户@新机器:~

# 解压
tar -xzf openclaw-state.tgz -C ~
tar -xzf openclaw-workspace.tgz -C ~/.openclaw/
```

### 4. 修复

```bash
openclaw doctor
openclaw gateway restart
openclaw status
```

## 注意事项

| 问题 | 解决 |
|------|------|
| profile 不匹配 | 用相同 profile 启动 |
| 只拷贝 openclaw.json | 必须拷贝整个目录 |
| 权限问题 | 检查文件所有者 |
| 远程模式 | 迁移 gateway 所在主机 |

## 验证

- [ ] `openclaw status` 显示运行中
- [ ] 频道仍连接（WhatsApp 等无需重新配对）
- [ ] 工作区文件存在

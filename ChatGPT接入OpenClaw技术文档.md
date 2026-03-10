# ChatGPT接入OpenClaw技术文档[有问题，后续等待排查]

## 1. 简介

本文档详细介绍如何将ChatGPT订阅自带的Codex额度接入OpenClaw，实现零成本使用GPT-5.4等模型。OpenAI官方明确支持在OpenClaw这类外部工具中使用Codex OAuth授权，目前零风险。

### 核心优势
- 无需额外购买OpenAI API
- 利用ChatGPT订阅自带的Codex额度
- Codex额度与聊天额度分离，互不影响
- OpenAI官方支持，合规使用

## 2. 前置条件

### 2.1 环境要求
- 已安装OpenClaw（版本需≥2026.3.7）
- 拥有ChatGPT账号（免费用户或Plus/Business/Pro订阅）
- 支持的操作系统：Linux/macOS/Windows

### 2.2 版本兼容性
- **最低版本**：2026.3.7
- **推荐版本**：最新稳定版
- **重要提示**：2026.3.2及以下版本无法识别GPT-5.4模型

## 3. 接入流程

### 3.1 升级OpenClaw

在终端执行以下命令升级OpenClaw：

```bash
openclaw update
```

**说明**：
- 升级过程需要几分钟，请耐心等待
- 升级完成后会自动重启Gateway服务
- 如遇升级失败，可执行 `npm install -g openclaw` 重新安装

### 3.2 备份配置

运行向导前，建议先备份现有配置：

```bash
cp -a ~/.openclaw ~/.openclaw.bak
```

**Windows用户**：
```powershell
Copy-Item -Recurse -Force $env:USERPROFILE\.openclaw $env:USERPROFILE\.openclaw.bak
```

### 3.3 运行Onboarding向导

执行以下命令启动配置向导：

```bash
openclaw onboard --auth-choice openai-codex
```

向导配置选项：

1. **I understand this is personal-by-default...**
   - 选择：`Yes`

2. **Onboarding mode**
   - 选择：`QuickStart`

3. **Config handling**
   - 选择：`Use existing values`
   - **警告**：切勿选择`Reset`，否则将丢失所有设置、记忆和定时任务

### 3.4 OAuth授权

向导执行到OpenAI Codex OAuth步骤时：

1. 向导会提供一个授权URL
2. 将URL复制到浏览器中打开
3. 使用ChatGPT账号登录
4. 点击授权按钮
5. 浏览器会跳转到回调地址：`http://localhost:1455/auth/callback?code=...`

**本地部署场景**：
- 浏览器会自动弹出授权窗口，直接登录授权即可

**远程服务器部署场景**：
- 服务器上无法打开回调页面是正常现象
- 将浏览器地址栏中的完整URL复制下来
- 粘贴回终端的`Paste the redirect URL`输入框

### 3.5 跳过后续配置

向导后续配置步骤：

1. **频道选择**
   - 选择：`Skip for now`
   - 原因：之前配置的频道仍然保留

2. **Skills配置**
   - 选择：`Skip`

3. **Search provider**
   - 选择：`Skip for now`
   - 原因：之前配置的搜索引擎仍然保留

4. **Hooks配置**
   - 选择：`Skip`

5. **Gateway service**
   - 选择：`Restart`

6. **How do you want to hatch your bot**
   - 选择：`Do this later`
   - 原因：龙虾已配置完成，无需重复操作

### 3.6 设置模型

执行以下命令设置GPT-5.4模型：

```bash
openclaw models set openai-codex/gpt-5.4
```

## 4. 核心配置步骤

### 4.1 验证配置

通过以下任一方式验证配置是否成功：

**方式一：Web控制台**
- 访问OpenClaw Web控制台
- 发送测试消息

**方式二：TUI界面**
```bash
openclaw tui
```

**方式三：连接的频道**
- 在已配置的频道发送测试消息

### 4.2 查看额度

查看Codex使用额度：

1. 打开ChatGPT网页端
2. 左侧菜单栏选择`Codex`
3. 右上角点击设置
4. 左侧菜单选择`Usage`

**额度类型**：
- 5小时额度
- 每周额度
- Code Review额度

**重要说明**：
- OpenClaw消耗的是Codex专用额度
- 与ChatGPT聊天额度完全分离
- 两个额度池互不影响

## 5. 关键代码示例

### 5.1 完整接入脚本

```bash
#!/bin/bash

# ChatGPT接入OpenClaw自动化脚本

# 1. 备份配置
echo "正在备份配置..."
cp -a ~/.openclaw ~/.openclaw.bak

# 2. 升级OpenClaw
echo "正在升级OpenClaw..."
openclaw update

# 3. 运行向导（需要手动交互）
echo "启动配置向导..."
openclaw onboard --auth-choice openai-codex

# 4. 设置模型
echo "设置GPT-5.4模型..."
openclaw models set openai-codex/gpt-5.4

echo "配置完成！"
```

### 5.2 Windows PowerShell脚本

```powershell
# ChatGPT接入OpenClaw - Windows版本

# 1. 备份配置
Write-Host "正在备份配置..." -ForegroundColor Green
Copy-Item -Recurse -Force $env:USERPROFILE\.openclaw $env:USERPROFILE\.openclaw.bak

# 2. 升级OpenClaw
Write-Host "正在升级OpenClaw..." -ForegroundColor Green
openclaw update

# 3. 运行向导
Write-Host "启动配置向导..." -ForegroundColor Green
openclaw onboard --auth-choice openai-codex

# 4. 设置模型
Write-Host "设置GPT-5.4模型..." -ForegroundColor Green
openclaw models set openai-codex/gpt-5.4

Write-Host "配置完成！" -ForegroundColor Green
```

### 5.3 模型切换命令

```bash
# 查看可用模型
openclaw models list

# 设置GPT-5.4
openclaw models set openai-codex/gpt-5.4

# 设置其他模型
openclaw models set openai-codex/gpt-4
openclaw models set openai-codex/gpt-4-turbo

# 查看当前模型
openclaw models get
```

## 6. 常见问题解决方案

### 6.1 升级失败

**问题现象**：执行`openclaw update`失败

**解决方案**：
```bash
npm install -g openclaw
```

### 6.2 模型识别错误

**问题现象**：设置`openai-codex/gpt-5.4`时报错

**原因分析**：OpenClaw版本过低，不支持GPT-5.4

**解决方案**：
```bash
# 检查当前版本
openclaw --version

# 升级到最新版本
openclaw update

# 验证版本是否≥2026.3.7
```

### 6.3 OAuth授权回调失败

**问题现象**：浏览器跳转到localhost回调页面失败

**解决方案**：
- **本地部署**：确保OpenClaw服务正在运行，端口1455未被占用
- **远程部署**：这是正常现象，直接复制浏览器地址栏的完整URL到终端即可

### 6.4 配置丢失

**问题现象**：选择`Reset`选项后，所有配置丢失

**解决方案**：
```bash
# 恢复备份
cp -a ~/.openclaw.bak ~/.openclaw

# 重启Gateway
openclaw gateway restart
```

### 6.5 额度不足

**问题现象**：OpenClaw无法正常响应，提示额度不足

**解决方案**：
1. 检查Codex额度是否用尽
2. 等待额度重置（每周重置）
3. 考虑升级ChatGPT订阅计划

### 6.6 模型切换不生效

**问题现象**：执行模型切换命令后，仍使用旧模型

**解决方案**：
```bash
# 重启Gateway服务
openclaw gateway restart

# 验证当前模型
openclaw models get
```

## 7. 注意事项

### 7.1 安全性
- OpenAI官方明确支持Codex OAuth用于OpenClaw
- 目前零风险，可放心使用
- 不要分享授权URL或回调URL

### 7.2 配置管理
- 务必在重大操作前备份配置
- 避免选择`Reset`选项
- 定期检查模型版本兼容性

### 7.3 额度管理
- Codex额度与聊天额度分离
- 关注额度使用情况，避免影响使用
- 免费用户也有Codex权限，但额度可能较低

### 7.4 版本更新
- OpenClaw会定期更新以支持新模型
- 建议定期执行`openclaw update`
- 关注官方公告了解新功能

### 7.5 网络环境
- 确保网络连接稳定
- OAuth授权需要访问OpenAI服务
- 防火墙需允许相关端口通信

### 7.6 平台差异
- Linux/macOS使用`cp`命令备份
- Windows使用`Copy-Item`命令
- 路径格式根据操作系统调整

## 8. 技术支持

### 8.1 官方资源
- OpenClaw官方文档
- OpenAI Codex文档
- OpenClaw社区论坛

### 8.2 故障排查流程
1. 检查OpenClaw版本
2. 验证网络连接
3. 查看日志文件
4. 尝试重启服务
5. 恢复备份配置

### 8.3 日志查看

```bash
# 查看Gateway日志
openclaw logs gateway

# 查看所有日志
openclaw logs --all

# 实时查看日志
openclaw logs --follow
```

## 9. 附录

### 9.1 支持的模型列表
- `openai-codex/gpt-5.4`（推荐）
- `openai-codex/gpt-4`
- `openai-codex/gpt-4-turbo`
- 其他OpenAI Codex支持的模型

### 9.2 配置文件位置
- **Linux/macOS**：`~/.openclaw/`
- **Windows**：`%USERPROFILE%\.openclaw\`

### 9.3 常用命令速查

```bash
# 升级OpenClaw
openclaw update

# 运行向导
openclaw onboard --auth-choice openai-codex

# 设置模型
openclaw models set <model-name>

# 查看模型
openclaw models list

# 查看当前模型
openclaw models get

# 启动TUI
openclaw tui

# 重启Gateway
openclaw gateway restart

# 查看日志
openclaw logs gateway

# 查看版本
openclaw --version
```

### 9.4 版本历史
- **2026.3.7**：支持GPT-5.4模型
- **2026.3.2**：不支持GPT-5.4，需升级

---

**文档版本**：1.0
**最后更新**：2026-03-10
**适用版本**：OpenClaw ≥ 2026.3.7

---
title: VS Code 中直接使用 Codex 教程及连接失败解决方案
date: 2026-06-25 22:42:23
updated: 2026-06-26 21:18:41
tags:
- vscode
- codex
- openai
- python
categories:
- 技术记录
---

> 本文原发布于 CSDN：<https://blog.csdn.net/Xi1yang/article/details/162315971>
>
> 版权声明：本文为博主原创文章，遵循 CC 4.0 BY-SA 版权协议，转载请附上原文出处链接和本声明。

### 前言

最近在学习和尝试 OpenAI Codex，发现它可以直接在 VS Code 中使用，帮助我们更高效地完成代码编写、解释和调试。

本文记录一下在 VS Code 中配置 Codex 的过程，同时整理我遇到的连接失败问题及解决办法。

### 一、安装 Codex CLI

#### 1. 打开 PowerShell 终端安装 Codex CLI

首先需要确保本机已经安装 Node.js 和 npm。

然后在 PowerShell 终端中执行：

```bash
npm install -g @openai/codex
```

安装完成后，可以运行：

```bash
codex
```

如果能够正常进入 Codex，说明 CLI 安装成功。

#### 2. 安装成功后检查环境变量

安装成功之后，在系统环境变量中可以看到新的 npm 全局路径。

如果后续在 VS Code 中无法识别 `codex` 命令，可以优先检查 npm 全局安装路径是否已经添加到系统环境变量 `Path` 中。

### 二、在 VS Code 中配置 Codex

#### 1. 打开 VS Code 设置

打开 VS Code 设置，搜索：

```text
Codex: Cli Path
```

#### 2. 找到 settings.json

找到 `settings.json`，点击进入编辑。

#### 3. 在 settings.json 中加入 Codex CLI 路径

在 `settings.json` 中加入 Codex CLI 的路径，例如：

```json
{
  "codex.cliPath": "C:\\Users\\你的用户名\\AppData\\Roaming\\npm\\codex.cmd"
}
```

如果你的 npm 全局路径不是默认路径，需要根据实际安装位置修改。

#### 4. VS Code 识别 Codex

配置完成后，VS Code 就能识别 Codex 命令。

建议重启 VS Code 后再尝试运行 Codex 插件。

### 三、运行

在 VS Code 中打开 Codex 扩展，根据提示运行即可。

如果遇到连接失败，可以检查 `.codex` 目录下的配置。

Windows 下通常位于：

```text
C:\Users\你的用户名\.codex
```

可以在该目录下创建或修改 `config.toml`：

```toml
model = "gpt-4.1"
approval_policy = "on-request"
sandbox_mode = "workspace-write"
network_access = true

[model_providers.openai]
base_url = "https://api.openai.com/v1"
env_key = "OPENAI_API_KEY"
```

然后在同一目录下创建或修改 `auth.json`：

```json
{
  "OPENAI_API_KEY": "你的 API 密钥"
}
```

保存后重新打开 VS Code，再运行 Codex。

### 四、常见问题

#### 1. 下载 npm 包时报错

如果安装时出现类似报错：

```text
npm : 无法加载文件 D:\nodejs\npm.ps1，因为在此系统上禁止运行脚本。
```

这是 PowerShell 执行策略导致的。

可以用管理员身份打开 PowerShell，然后执行：

```powershell
Set-ExecutionPolicy RemoteSigned
```

执行后选择 `Y` 确认。

也可以直接使用 `npm.cmd` 代替 `npm` 执行命令，例如：

```powershell
npm.cmd install -g @openai/codex
```

#### 2. 下载之后检查 npm 路径

如果安装完成后命令仍然无法识别，需要检查 npm 的全局安装路径：

```bash
npm config get prefix
npm config get cache
```

如果路径不符合预期，可以重新设置：

```bash
npm config set prefix "D:\nodejs\node_global"
npm config set cache "D:\nodejs\node_cache"
```

然后把全局安装路径添加到系统环境变量 `Path` 中，例如：

```text
D:\nodejs\node_global
```

配置完成后重新打开终端，再执行：

```bash
codex
```

#### 3. 更新 Node.js

如果安装过程中提示 Node.js 版本过低，需要更新 Node.js。

可以到 Node.js 官网下载新版本安装包：

```text
https://nodejs.org/
```

安装完成后检查版本：

```bash
node -v
npm -v
```

确认版本正常后，再重新安装 Codex CLI：

```bash
npm install -g @openai/codex
```

### 总结

整体流程如下：

1. 安装 Node.js 和 npm。
2. 使用 npm 全局安装 Codex CLI。
3. 在 VS Code 中配置 Codex CLI 路径。
4. 在 `.codex` 目录中配置 `config.toml` 和 `auth.json`。
5. 重启 VS Code 后运行 Codex。

如果遇到连接失败，可以重点检查 API Key、模型配置、网络连接、npm 全局路径和 PowerShell 执行策略。

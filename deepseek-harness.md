# DeepSeek Harness（dsh）深度页

这是 [README.md](README.md) 里旗舰条目的完整上手页。目标：照着做就能跑起来，跑起来之后还能装插件。

> 信息基于 2026-08 的官方公开资料与社区实测整理。项目是开发者预览版，命令与界面可能变动，请以 `npx @deepseek-ai/dsh --help` 与官方文档 [deepseek-harness.github.io](https://deepseek-harness.github.io) 为准。

## 它到底是什么

DeepSeek Harness（命令行缩写 `dsh`）是 DeepSeek 官方开源的 Agent 框架。**核心理念一句话："Everything is a Plugin"**——模型接入、工具调用、会话存储、审批策略、界面组件，整个系统的每一个环节都是可替换、可组合的插件，跑在 [Cordis](https://github.com/koishijs/cordis) 框架上。

和 Claude Code / Codex 的区别：同类工具通常把核心逻辑写死，你只能在预留接口上做有限定制；dsh 反过来，从模型接入到工具注册表全部插件化，换搜索引擎、接自家模型，改配置即可，不用动框架代码。

## 安装

先确认 **Node.js ≥ 22.19**（LTS 即可）。`node -v` 看版本，不够就去 nodejs.org 下最新 LTS。

**方式 A：一行命令跑起来（最省事，推荐首次体验）**

```bash
npx @deepseek-ai/dsh web
```

终端会打印本地地址，默认 **http://127.0.0.1:3080**，浏览器打开即进 Web UI。注意：终端窗口不能关，它是真正干活的 Host，网页只是操作界面。

**方式 B：全局安装（日常使用推荐）**

```bash
npm install -g @deepseek-ai/dsh
dsh --version      # 能打印版本号即成功
dsh web            # 启动
```

**方式 C：从源码运行（开发者）**

```bash
git clone https://github.com/deepseek-ai/deepseek-harness.git
cd deepseek-harness
pnpm install
pnpm run build
pnpm dsh web
```

**方式 D：让 AI 帮你装**——把下面这段丢给任何能操作终端的 AI 工具（Claude Code / Codex / Cursor），它会把环境检查、安装、启动全流程跑完：

```
在这台电脑上安装并启动 DeepSeek Harness(DSH)：
1. 检查 Node.js，版本需 22.19 及以上，没有则安装 LTS 版；
2. 执行 npm install -g @deepseek-ai/dsh 全局安装，并用 dsh --version 验证；
3. 启动 dsh web，把终端打印的本地访问地址告诉我。
```

## 配置模型

dsh 是 "BYO 模型" 模式——框架本身不带模型，需要你自己配一个端点。

1. 打开 Web UI → **设置 → 模型**，粘贴你的 DeepSeek API Key（去 DeepSeek 开放平台创建，Key 只在创建那一刻显示明文）。
2. 在模型选择器里选一个模型（如 DeepSeek V4 Flash）。**配置即时生效，无需重启。**
3. 想接别家（Anthropic / OpenAI / 本地 llama.cpp）：设置里"添加提供方"或"添加自定义提供方"，填 baseURL + 协议（如 `openai-completions`）+ API Key（用 `apiKeyEnv` 环境变量更安全）。

## 工作区与权限

**Workspace（工作区）** 是 Agent 读写文件的边界目录——你希望它在哪个文件夹里干活，就把哪个文件夹添加为 Workspace。没选工作区之前，会话输入框是禁用的（强制你先划边界）。

三档权限模型：

| 权限 | 内部名 | 允许做什么 |
| --- | --- | --- |
| 只读 | Read Only | 只能看，不能改任何文件 |
| 工作区写 | Workspace Write | 只能在工作区内写文件（日常默认） |
| 全开 | Full Access (`danger-full-access`) | 全盘读写，无边界，切换前会二次确认 |

> 关键认知：权限限制的是"写"不是"看"。Workspace Write 只限制写入范围，读取文件、联网不受同等限制。涉及敏感操作（联网上传、系统级改动）会弹审批窗，不会静默放行。

底层有真实 OS 级沙箱支撑：Linux 用 bwrap/Landlock，macOS 用 Seatbelt，Windows 用 ACL 受限令牌。

## 四种模式（Preset）

新建会话时选，开始运行后锁定（中途切换会破坏会话可复现性，所以不允许切）：

- **标准模式 Standard**——全功能：文件编辑、Shell、检索、Skills、计划、子代理、工作流。拿不准就选它。
- **PTC 模式 Code Mode**——模型直接写一段程序，把多步工具操作编排好一次执行。适合步骤多但逻辑固定的批量活。
- **极简模式 Minimal**——只留一个 shell + 一个文件编辑，官方拿它跑基准测试，日常用不上。
- **创造模式 Creative**——能看到当前跑着哪些插件，还能造新插件和新的 Agent 预设。想自己扩展生态选这个。

## 装社区插件（重点）

插件就是 GitHub 上打了 `dsh-plugin` 标签的项目。安装按 profile 管理：

```bash
# 从 GitHub 装（推荐）
dsh plugin --profile <profile> add github:owner/repo#main

# 从本地目录装
dsh plugin --profile demo add ./hello-plugin

# 移除
dsh plugin --profile <profile> remove <plugin-id>
```

也可以不敲命令——在 dsh 里直接把插件开源地址发给 Agent，让它帮你装（省事的话先把权限调成全开，AI 执行命令时不用一个个确认）。

想找现成插件，GitHub 搜 **`awesome-deepseek-harness`**（社区维护，已收录 300+ 可装插件，分类整理）。想看一个真实的小插件长啥样，参考 [gengyueworks/dsh-zhihu](https://github.com/gengyueworks/dsh-zhihu)——我用 dsh 真实 API 写的知乎读取插件。

## 排错速查

| 现象 | 原因 | 解决 |
| --- | --- | --- |
| `command not found: node` | Node 未装或未加入 PATH | 重装 Node，保留"添加到 PATH"；已装则重开终端 |
| `node -v` 低于 22.19 | 版本过旧 | 下最新 LTS 覆盖安装 |
| `command not found: dsh` | 全局安装未成功 | 重跑安装命令；或用 `npx @deepseek-ai/dsh web` 兜底 |
| 端口被占用 | 3080 被占 | `dsh web --port 8080` |
| 网页打不开 | 启动失败 / 端口冲突 | 看跑 `dsh web` 的终端报错输出；Ctrl+C 停掉重启 |
| Windows PowerShell 红一片 | 拦截了 `npx.ps1` | 改用 `npx.cmd @deepseek-ai/dsh web` |

**排错第一原则**：先看跑 `dsh web` 的那个终端窗口——绝大多数问题的答案都在它的报错输出里。

## 回到总清单

👈 [README.md](README.md)

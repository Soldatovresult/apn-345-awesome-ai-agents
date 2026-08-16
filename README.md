# AI Agent 精选 · awesome-ai-agents

想上手 / 看懂 AI Agent，这一个清单就够了（中英双语）。

> 2026 年，"AI Agent 正在吃掉软件"已经不是预言，是正在发生的事。
> 但信息太碎：今天一个框架、明天一个协议、后天 DeepSeek 又开源个 Harness。
> 这份清单只收**真的能跑起来、真的能上手**的东西，按"连接 → 能力 → 运行时"三层组织。
> 不收标题党、不收还没发布就吹上天的。

## 取舍标准

- **能跑，不画饼**：入选的要么已经开源可用，要么有官方文档能照着做。
- **三层分清**：连接（MCP）、能力（Skills）、运行时（Harness/框架）——别混为一谈。
- **中英都给**：技术名保留英文，中文用户能直接查、能直接装。
- **中国视角**：优先补上中文世界被忽略的好东西，不止搬运英文榜单。

## 核心速览：一句话建立心智模型

AI Agent 的生态现在分三层，记住这三层，看什么都清楚：

| 层 | 它解决什么 | 代表 | 一句话 |
| --- | --- | --- | --- |
| 🔌 连接层 MCP | Agent 怎么"接上"外部工具 / 数据 | Model Context Protocol | 给 Agent 装手脚（数据库、邮箱、API） |
| 🧩 能力层 Skills | Agent 怎么"学会"做某事 | Anthropic Agent Skills | 给 Agent 写操作手册（知识 / 流程） |
| 🛠 运行时 Harness | Agent 怎么"跑起来"并被组装 | DeepSeek Harness、LangGraph… | 给 Agent 一块乐高底板 |

**三年一条线（为什么是现在）**：

- **2024-11** Anthropic 开源 **MCP**——先把"Agent 怎么连工具"标准化。
- **2025-10** Anthropic 在 Claude Code 推出 **Agent Skills**；12 月作为开放标准发布（agentskills.io），48 小时内被 Microsoft、OpenAI 采纳；同月 MCP 捐给 Linux 基金会。
- **2026** DeepSeek 开源 **Harness（dsh）**，把"Everything is a Plugin"做成普通人一行命令能跑的网页应用，把插件化 Agent 推到全民视野。

> 一句话：MCP 给了 Agent 手脚，Skills 给了 Agent 脑子里的操作手册，Harness 把这一切拼成你能用的产品。

## 🔌 连接层：MCP（Model Context Protocol）

- **Model Context Protocol** —— Anthropic 提出的开放标准，让 Agent 用统一方式连上数据库、邮箱、GitHub 等外部系统；2025-12 捐给 Linux 基金会（Agentic AI Foundation）。官方文档 [modelcontextprotocol.io](https://modelcontextprotocol.io)，仓库 [github.com/modelcontextprotocol](https://github.com/modelcontextprotocol)。安装常常一行：`claude mcp add --transport http <name> <url>`。据 2026 年初统计，月下载量已近亿级。
- **官方参考服务器合集** —— [github.com/modelcontextprotocol/servers](https://github.com/modelcontextprotocol/servers)，官方维护的各类 MCP server（文件系统、GitHub、Fetch 等），新手从这里挑着装。
- **awesome-mcp** —— 社区维护的 MCP 客户端 / 服务器 / 教程精选，找现成 server 先来这（GitHub 搜 `awesome-mcp`）。

## 🧩 能力层：Agent Skills

- **Anthropic Agent Skills** —— 2025-10 随 Claude Code 推出、12 月开源的标准。一个 Skill 就是一个带 `SKILL.md` 的文件夹（名字 + 描述 + 指令），跨厂商通用：Claude Code、OpenAI Codex、Gemini CLI、Cursor、VS Code、OpenCode、Goose 都能跑。规范站 [agentskills.io](https://agentskills.io)，已有 40+ 客户端采用。装现成的：`npx skills add anthropics/skills`（或 `vercel-labs/agent-skills`）；Claude Code 里 `/plugin marketplace add anthropics/skills` 后 `/plugin install document-skills@anthropic-agent-skills`。
- **skills.sh** —— 事实上的 Skill 市场，聚合各家的可安装 Skill。
- **[gengyueworks/skills](https://github.com/gengyueworks/skills)** —— 我自己的 AI Skills 合集（写作 / 金融研究 / 工具 / 方法论），中文场景下的实操 Skill。

## 🛠 运行时与框架：把 Agent 跑起来

> ⭐ **DeepSeek Harness（dsh）** —— 本清单的旗舰条目。
> DeepSeek 官方开源的 Agent 框架，核心理念就一句：**"Everything is a Plugin"**——模型接入、工具、会话、审批策略、甚至 UI，全是可热替换的插件（跑在 Cordis 框架上）。
> - 仓库：[github.com/deepseek-ai/deepseek-harness](https://github.com/deepseek-ai/deepseek-harness)（MIT，65k+★）
> - 一行跑起来：`npx @deepseek-ai/dsh web` → 打开 http://127.0.0.1:3080（需 Node.js ≥ 22.19）
> - 装社区插件（GitHub 上打了 `dsh-plugin` 标签的项目）：`dsh plugin --profile <profile> add github:owner/repo#main`
> - 社区插件精选清单：GitHub 搜 `awesome-deepseek-harness`（社区维护，已收录 300+ 可装插件）
> - 想看一个真实的小插件长啥样？👉 [gengyueworks/dsh-zhihu](https://github.com/gengyueworks/dsh-zhihu)（我用 dsh 真实 API 写的知乎读取插件，可作为写插件的参考样本）
>
> 完整安装 / 配置 / 四种模式 / 三档权限，见深度页 👉 [deepseek-harness.md](deepseek-harness.md)

其余主流运行时 / 框架（按场景挑）：

- **LangChain / LangGraph** —— 最流行的 Agent 编排库；LangGraph 用图把多步 Agent 流程管起来，适合复杂分支。
- **Microsoft AutoGen** —— 多 Agent 对话框架，几个 Agent 互相讨论把任务做出来。
- **CrewAI** —— 给每个 Agent 分配"角色 / 目标 / 工具"，像组一支团队。
- **OpenAI Agents SDK** —— 轻量 Agent 框架（前身 Swarm），几行代码起一个带工具调用的 Agent。
- **Google ADK（Agent Development Kit）** —— Google 的 Agent 开发套件，原生接 Gemini 与 Google 生态。
- **Microsoft Semantic Kernel** —— 面向企业的 Agent / 编排 SDK，.NET 与 Python 双支持。
- **Dify** —— 开源的 LLMOps / 可视化 Agent 搭建平台，拖拽就能做带工具的 Agent，非程序员也能用。
- **n8n** —— 工作流自动化工具，AI 节点 + 上千集成，把 Agent 嵌进日常流程。
- **MetaGPT** —— 多 Agent 模拟一家"软件公司"，一句话需求产出需求文档 / 设计 / 代码。
- **LlamaIndex** —— 数据框架（RAG + Agent），擅长让 Agent 基于你的私有知识干活。
- **Cordis** —— dsh 底层的插件运行时（源自 Koishi 生态），"一切皆插件"的引擎本身。

## 🎓 学习资源

- **[gengyueworks/agent-course](https://github.com/gengyueworks/agent-course)** —— 我写的《和智能体一起工作》框架预览，专门给普通人讲清楚怎么跟 Agent 协作（非程序员向）。
- **[gengyueworks/ai-timeline](https://github.com/gengyueworks/ai-timeline)** —— 中英双语 AI 编年史，116 个信源、从 1936 到 2026，建立 AI 的"时间感"。
- **[gengyueworks/ai-dictionary](https://github.com/gengyueworks/ai-dictionary)** —— AI 祛魅词典，1327 条索引 + 50 条精选样板，把黑话翻译回人话。
- **Anthropic《Building Effective Agents》** —— Anthropic 官方出的 Agent 设计指南，讲清楚什么时候该用 Agent、怎么搭才不翻车。
- **Anthropic《Skills explained》** —— 官方博客，把 Skills / MCP / 子 Agent 的区别讲透（配研究 Agent 的完整例子）。
- **DeepLearning.AI《AI Agents》短课（Andrew Ng）** —— 入门 Agent 的免费短课，跟着做几个小项目就懂了。
- **awesome-deepseek-harness** —— 想扎进 dsh 生态，这个社区清单是最好的入口（分类整理好的 300+ 插件，GitHub 搜 `awesome-deepseek-harness`）。

## 🀄 中文优质资源

- 上面三个 **gengyueworks** 仓库本身就是中文友好入口（双语）。
- 想读中文的"从 Agent 到 Skills 范式转变"长文，搜「Agent Skills 范式转变 53AI」有一篇讲透 MCP / Skills / A2A 关系的深度文。
- DeepSeek Harness 的中文保姆级教程社区已在产出（安装 / 配置 / 权限 / 插件一条龙），搜「DeepSeek Harness 教程」即可，官方文档在 [deepseek-harness.github.io](https://deepseek-harness.github.io)。

## 关联

- 同源项目：[极简清单 awesome-minimalism](https://github.com/gengyueworks/awesome-minimalism) · [习惯收藏库 awesome-habits](https://github.com/gengyueworks/awesome-habits) —— 同一套"精选策展"打法。
- 我的 AI Skills 合集：[gengyueworks/skills](https://github.com/gengyueworks/skills)
- 我的 Agent 课程：[gengyueworks/agent-course](https://github.com/gengyueworks/agent-course)
- 我的 AI 编年史：[gengyueworks/ai-timeline](https://github.com/gengyueworks/ai-timeline)
- 我的 AI 词典：[gengyueworks/ai-dictionary](https://github.com/gengyueworks/ai-dictionary)
- 我的 dsh 知乎插件（写作样本）：[gengyueworks/dsh-zhihu](https://github.com/gengyueworks/dsh-zhihu)

---

许可证：**CC0 1.0**（公有领域奉献，可自由使用、修改、转载，无需署名）。
贡献方式见 [CONTRIBUTING.md](CONTRIBUTING.md)。

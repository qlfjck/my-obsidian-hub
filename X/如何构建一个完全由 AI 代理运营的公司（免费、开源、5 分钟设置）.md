**# Paperclip：把你的AI Agent组织成一家真正的AI公司**

> [!IMPORTANT] Paperclip免费开源工具：把Claude Code、OpenClaw等多个AI Agent组织成带组织架构、预算、目标和统一仪表盘的AI公司

**📅 发布时间：** 2026年3月16日 **👤 作者：** Nick Spisak（@NickSpisak_） **🔗 来源平台：** X (Twitter)

---

## 💡 核心观点

如果你正在同时使用Claude Code、OpenClaw、Codex、OpenCode、Cursor或其他AI编码Agent，Paperclip就是为你量身打造的免费开源工具。它能把你已有的多个AI Agent组织成一家真正的“AI公司”——包含组织架构图、职位头衔、预算、公司目标和一个统一仪表盘，所有Agent都有明确角色、老板和预算，任务跨重启持久化，你甚至可以用手机查看一切。  
Paperclip完全自托管、无账号、无订阅、MIT协议开源，由@dotta开发，已在GitHub上线，第一周获160万浏览量。它解决的核心痛点是：多个Agent分散管理、输出复制粘贴、电脑重启后一切丢失、预算失控等问题，让非技术人员也能轻松构建和管理AI团队。

---

## 📖 详细内容梳理

### 引言：Paperclip诞生的背景

如果你正在使用Claude Code、OpenClaw、Codex、OpenCode、Cursor或其他任何AI编码Agent，那么Paperclip就是为你而生的。  
它是一个刚刚在GitHub上发布的免费开源工具，第一周就获得了160万次浏览。  
它的核心作用是：把你已经在使用的所有AI Agent组织成一家真正的公司——有组织架构图、职位头衔、预算、公司目标，以及一个统一的仪表盘来管理它们。  
你其实已经在使用这些“员工”了，Paperclip只是给了它们一个真正的工作环境。

> [!QUOTE] You're already using the workers. Paperclip gives them a company to work in.

如果你是非技术人员，也想学习如何构建这样的系统，作者推荐加入他们的“Build With AI”社区。

### 问题：当前使用多个AI Agent的痛点

目前大多数人的使用方式是：  
- 一个终端跑Claude Code  
- 另一个终端跑Codex  
- Cursor又开在别处  
- 不断在它们之间复制粘贴输出  
- 经常搞不清哪个Agent在做什么  
- 电脑重启后一切都丢失了  

Paperclip彻底解决这些问题：  
- 每个Agent都有明确角色、老板和预算  
- 任务在电脑重启后依然持久存在  
- 你可以随时在手机上查看所有进展  

Paperclip完全开源（MIT协议），无需账号、无订阅，完全自托管在你自己的机器上，由@dotta开发。

### 8大核心特性

**1. 组织架构图（Org Charts）**  
你可以像真实公司一样创建角色（CEO、CTO、工程师、设计师、营销人员等）。每个Agent都有职位、岗位描述和直接上级。汇报关系直接决定工作流如何在Agent之间传递。

**2. 目标对齐（Goal Alignment）**  
你设定一个公司级大目标（如“打造一个AI笔记应用，实现100万美金MRR”），目标自动拆解为项目，项目再拆解为任务。每个Agent在工作时都能看到自己任务背后的完整意义，而不仅仅是“做什么”。

**3. 心跳机制（Heartbeats）**  
Agent不会24/7一直烧钱运行，而是按照设定好的“心跳”时间表醒来，检查是否有工作要做，做完后立刻休眠。你可以精确控制每个Agent的检查频率，从而控制成本。

**4. 预算控制（Budget Controls）**  
每个Agent都有每月独立预算上限，达到上限后自动停止。Paperclip实时追踪每个Agent消耗的AI token费用，你再也不会因为某个Agent陷入死循环而收到300美元的意外账单。

**5. 工单系统（Ticket System）**  
所有工作都通过工单进行。每一次对话都有完整线程，每一次工具调用都有 traceable 记录，每一个决策都有日志。如果出问题，你可以精确追溯时间、原因和责任人。

**6. 兼容你已在使用的所有工具**  
Paperclip不绑定任何AI提供商。它原生支持Claude Code、OpenClaw、Codex、OpenCode、Cursor、纯Bash脚本，或任何能通过HTTP接收信号的Agent。你甚至可以混用：CEO用Claude，工程师用Codex，Paperclip只负责把它们组织成团队。

**7. 多公司支持（Multi-Company Support）**  
一个Paperclip实例可以同时运行多家完全独立的AI公司。每家公司的数据、预算和审计记录完全隔离。你可以同时运营一个AI营销代理公司和一个AI SaaS公司，所有内容都在同一个仪表盘里。

**8. 治理机制（Governance）**  
你是董事会：你审批招聘、审核战略变更、随时暂停或关闭任何Agent。所有配置变更都有版本记录，出问题可随时回滚。你始终保持完全控制权。

### 5分钟快速上手指南

**一键安装方法**（推荐）  
在终端直接运行以下命令即可完成安装、数据库创建和仪表盘启动：  
（原文提供具体命令，此处省略具体代码，但实际操作为一键式）

**手动安装方法**  
- 要求：Node.js 20+ 和 pnpm 9.15+  
- 按照GitHub仓库的详细步骤操作

安装完成后打开 http://localhost:3100 仪表盘，你将：  
1. 创建第一家公司并设定使命  
2. 招聘第一个Agent（通常从CEO开始）  
3. CEO会主动建议招聘更多Agent（如编码工程师）  
4. 你审批招聘并设置预算  
5. Agent开始围绕公司目标自主工作  
6. 你通过仪表盘（电脑或手机）统一管理一切

### 即将上线：ClipMart市场

团队正在开发ClipMart——一个一键导入预构建AI公司的市场。  
你将能直接浏览“营销代理公司”“SaaS构建器”“内容工作室”等完整模板，一键导入Paperclip，立即获得一个配备齐全的AI公司。  
ClipMart尚未上线，但已在路线图上。

### 适合人群

Paperclip不是给“只用一个AI工具完成单一任务”的人准备的。  
它专为以下人群设计：  
- 已经同时运行多个AI Agent，却苦于无法协调  
- 曾经打开过10+个Claude Code终端，却完全搞不清每个终端在做什么  
- 曾经因为无人监控而让AI Agent烧掉200美元token  

一句话总结：**一个仪表盘、一张组织架构图、一套预算系统，把所有Agent全部管起来**。

GitHub仓库已上线，完全免费，5分钟即可搭建完成。

（后续有Part 2：安装后如何进一步使用）

**社区与后续**  
作者正在构建一个社区，专门拆解如何把自定义Agent变成真正的“员工”：自定义技能、定时工作流、睡着也能运行的系统。  
感兴趣可加入等待列表（链接已附）。

---

## 🔗 原文链接

[点击查看原文](https://x.com/NickSpisak_/status/2033518072724705437)

**🏷️ 标签：** #Paperclip #AI Agent公司 #多Agent协调 #开源AI工具 #OrgChart #预算控制 #ClaudeCode #OpenClaw #零员工公司 #AI工作流
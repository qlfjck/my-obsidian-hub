# Paperclip AI：30K星开源AI代理编排系统，一键组建零人工公司实战指南

> [!IMPORTANT] 核心速览 Paperclip AI用仪表盘+目标层级+预算控制，让AI代理团队自主运营公司，你只当董事会。

**📅 发布时间：** 2026年3月19日 **👤 作者：** AlphaSignal AI (@AlphaSignalAI) **🔗 来源平台：** X (Twitter)

---

## 💡 核心观点

Paperclip AI 是一款开源的 Node.js 服务端 + React UI 工具，仅上线几天就收获超过 30K GitHub 星标。它通过目标层级、工单系统、组织架构图和严格预算控制，将多个 AI 代理组织成一个完整“虚拟公司”，用户仅作为董事会角色审批雇佣、审核策略、暂停/终止代理，而代理则自主执行任务并汇报。

与 CrewAI、AutoGen、LangGraph 等框架不同，Paperclip 位于更高层：它不是构建单个工作流的框架，而是提供公司级治理层（组织图、预算、审计追踪、版本回滚），真正实现“零人工公司”的基础设施。

核心机制是“heartbeat 心跳调度 + 完整目标祖先传递”，确保每个代理始终知道自己在为公司使命工作；预算作为第一类组织约束，防止失控消耗 API 费用。

---

## 📖 详细内容梳理

### Paperclip AI 是什么？核心功能与定位

Paperclip AI 是一款开源工具，包含 Node.js 后端服务器和 React 前端 UI。它能编排一整个 AI 代理团队来运营一家虚拟公司。

用户只需带来自己的 AI 代理、分配目标，然后从单一仪表盘跟踪所有工作和成本。

> [!QUOTE] You operate as the board of directors. The agents do the work.

它不是单次任务执行工具，而是完整的公司操作系统：代理在工单系统中工作，所有任务都追溯到公司使命；代理拥有完整目标层级（goal ancestry），永远知道“正在做什么”和“为什么做”。

代理采用 **heartbeat 心跳调度** 机制：定时醒来、检查分配的任务、执行、汇报。委托关系在组织架构图中上下流动——CEO 代理需要工程工作时，会通过 Paperclip 的工单系统委托给工程代理。

治理层完全由用户掌控：你审批所有雇佣、审核策略、可随时暂停或终止任何代理、配置变更全部版本化、坏变更可一键回滚。

### 如何从零搭建 Paperclip AI

搭建要求：
- Node.js 20+
- pnpm 9.15+

**步骤 1：克隆仓库并安装依赖**  
使用 GitHub Desktop 或终端命令克隆仓库。  
进入项目根目录，运行安装命令（具体命令在原文中给出）。确保终端无任何错误。

**步骤 2：构建项目**  
在同一终端窗口执行 `pnpm run build`，看到 “Done” 且无错误即成功。

**步骤 3：启动项目**  
执行 `pnpm run dev`，终端显示 API 服务器已启动。

### 创建第一个 AI 员工（Creating AI workers）

在浏览器打开 `http://127.0.0.1:3100/` 或 `http://localhost:3100/`，进入欢迎界面，按照屏幕提示设置你的虚拟公司。

**Agent 标签页**：新建代理，命名为 CEO，按照表单填写。  
路径名称（pathname）获取方法：  
- 打开 Finder 导航到文件夹  
- 右键（或 Control-Click）文件夹  
- 按住 Option (⌥) 键，“Copy” 变为 “Copy as Pathname”  
- 点击复制路径

**创建任务**：标题和描述越详细越好。  
示例给 CEO 代理的任务描述（原文给出具体示例）。

最后在 Launch 标签页点击 “Create and open issue” 按钮。  
代理启动成功后会自动跳转到主仪表盘。  
系统会自动创建嵌入式 PostgreSQL 数据库，无需额外配置。

### 管理目标与组织架构（Managing Goals and Tracking the Org）

CEO 代理启动后会自动开始招聘，可能提出招聘 “Founding Engineer” 并列出所需能力。你会收到审批提示，可接受或拒绝。

所有虚拟员工可在 Org 标签页查看，支持可视化组织层级图。

**Goals 标签页**：点击 “New goal” 创建公司级目标，并详细描述。  
示例目标：  
“Launch AlphaSignal v0.1: Bootstrap the company, hire core team, define product, and ship the first working version of AlphaSignal.”

可将大目标拆分为子目标，分配给特定代理或团队，进度自动向上层级汇总。

### 预算控制与成本实时追踪（Budget Control and Cost Tracking）

Costs 标签页是核心功能：  
- 为每个代理设置每月推理预算上限  
- 实时监控花费  
- 达到 80% 时发出警告  
- 达到 100% 时代理自动暂停，新任务被阻挡，直到你提高限额或进入下一个计费周期  
- 可随时手动覆盖

> [!IMPORTANT] 这解决了多代理系统最常见的问题：单个失控循环可能在你察觉前烧掉数百美元 API 费用。Paperclip 将预算视为第一类组织约束。

### Paperclip 与现有 Agent 框架的对比

Paperclip **不与** CrewAI、AutoGen、LangGraph 竞争：

- CrewAI：定义角色与任务的 crew  
- LangGraph：图式工作流控制  
- AutoGen：对话式代理架构  

这些框架都聚焦于“如何让代理在单个工作流中协作”。

Paperclip 位于更高层：它提供公司级基础设施——组织架构图、预算执行、工单系统、审计追踪、治理层。你完全可以在 Paperclip 内部使用 CrewAI 代理。

> [!QUOTE] The closest comparison is the difference between a CI/CD pipeline and a project management tool. One executes tasks. The other manages the organization around those tasks.

### 更广泛的意义

“零人工公司”是否会成为主流仍是开放问题，但 Paperclip 提供的组织图、预算强制、持久状态、审计追踪等底层工具，对任何需要协调多个 AI 代理的人都立即可用。

作者最后询问读者看法，并附上仓库和官网链接，欢迎关注 @AlphaSignalAI 获取更多类似内容。

---

## 🔗 原文链接

[点击查看原文](https://x.com/AlphaSignalAI/status/2034647427609751913?s=20)

---

**🏷️ 标签：** #PaperclipAI #AI代理编排 #零人工公司 #多代理系统 #开源AI工具 #预算控制 #组织架构 #Agent治理层
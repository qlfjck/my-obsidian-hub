# Open SWE：开源生产级编码Agent框架，封装Stripe、Ramp、Coinbase内部实践的通用架构

> [!IMPORTANT] 核心速览 Open SWE是LangChain开源框架，基于Deep Agents与LangGraph，完整复刻Stripe/Ramp/Coinbase生产级内部编码Agent的六大共同架构模式，让企业零门槛部署可定制的AI编码Agent。

**📅 发布时间：** 2026年3月17日 **👤 作者：** LangChain (@LangChain) **🔗 来源平台：** X (Twitter)

---

## 💡 核心观点

过去一年，Stripe（Minions）、Ramp（Inspect）、Coinbase（Cloudbot）等工程团队独立开发了内部编码Agent，这些系统不要求工程师切换新界面，而是深度集成现有工作流（Slack、Linear、GitHub）。

尽管独立开发，但它们高度趋同于六大架构模式：隔离云沙箱、精选工具集、Slack优先调用、启动时丰富上下文、子Agent编排、与开发者工作流集成。这表明生产环境部署AI Agent存在普适性要求。

今天LangChain发布**Open SWE**开源框架，基于Deep Agents与LangGraph，将这些模式封装成可定制组件，为正在探索内部编码Agent的组织提供起点。

---

## 📖 详细内容梳理

### 生产部署中的共同模式（Patterns from Production Deployments）

LangChain观察Stripe、Ramp、Coinbase的编码Agent，发现它们做出了高度相似的架构决策：

- **隔离执行环境**：任务在专用云沙箱中运行，沙箱内拥有完整权限，但严格边界隔离，防止错误波及生产系统，同时允许Agent无需每次操作都请求人工批准。
- **精选工具集**：Stripe工程团队表示，Agent拥有约500个工具，但这些工具是精心挑选和维护的，而非随意积累。工具的**精选质量**比数量更重要。
- **Slack优先调用**：三家公司均将Slack作为主要交互界面，让开发者在已有沟通工作流中调用Agent，避免切换新应用。
- **启动时丰富上下文**：Agent在开始工作前，会自动拉取Linear issue、Slack thread或GitHub PR的完整上下文，减少后续通过工具调用发现需求的开销。
- **子Agent编排**：复杂任务会被分解并委托给专门的子Agent，每个子Agent拥有隔离上下文和专注职责。
- **与开发者工作流集成**：Agent通过Slack、Linear、GitHub无缝嵌入现有流程，而非要求工程师采用全新界面。

这些选择已在多个生产环境中被验证有效，但每个组织仍需根据自身环境和需求进行适配。

### Open SWE的架构（Open SWE's Architecture）

Open SWE提供了这些模式的开源实现，与观察到的生产实践一一对应：

#### 1. Agent Harness：基于Deep Agents的组合式构建

Open SWE并非从零开发或fork现有Agent，而是**组合**在Deep Agents框架之上（类似Ramp在OpenCode之上构建Inspect）。

组合优势：
- **升级路径**：Deep Agents未来在上下文管理、规划效率、token优化等方面的改进，可直接继承，无需重构自定义部分。
- **无需fork的定制**：组织特定的工具、prompt、工作流仅作为配置存在，而非修改核心Agent逻辑。

Deep Agents内置能力：
- write_todos规划
- 基于文件的上下文管理
- task工具原生支持子Agent生成
- 中间件钩子实现确定性编排

#### 2. Sandbox：隔离云环境

每个任务运行在独立的云沙箱（远程Linux环境），拥有完整shell访问权限。仓库会被克隆进入，Agent获得完整权限，所有错误被严格限制在沙箱内。

Open SWE开箱支持多种沙箱提供商：
- Modal
- Daytona
- Runloop
- LangSmith

也可实现自定义沙箱后端。

关键行为：
- 每个对话线程拥有持久沙箱，后续消息可复用
- 沙箱不可达时自动重建
- 支持多任务并行，每个任务独立沙箱

#### 3. Tools：精选而非累积

Open SWE提供精炼工具集（具体工具列表在原文中列出），加上Deep Agents内置工具（read_file、write_file、edit_file、ls、glob、grep、write_todos、task）。

强调：更小、更精选的工具集更容易测试、维护和被Agent推理。组织可按需显式添加内部API、部署系统、专项测试框架等工具。

#### 4. Context Engineering：AGENTS.md + 来源上下文

上下文来自两个层面：
- **AGENTS.md文件**：若仓库根目录存在AGENTS.md，沙箱会读取并注入系统prompt，编码团队约定、测试要求、架构决策等仓库级知识。
- **任务特定上下文**：启动前自动组装完整Linear issue（标题、描述、评论）或Slack thread历史，无需额外工具调用。

这种双层设计平衡了仓库全局知识与任务即时信息。

#### 5. Orchestration：子Agent + 中间件

编排结合两种机制：
- **子Agent**：主Agent通过task工具生成子Agent，子Agent拥有独立上下文、中间件栈、todo列表和文件操作。
- **中间件**：确定性钩子在Agent循环中运行，包括：
  - check_message_queue_before_model：在下一次模型调用前注入后续消息（Linear评论或Slack消息），允许用户在中途提供输入。
  - open_pr_if_needed：安全网，如果Agent未完成PR步骤，中间件自动提交并打开PR。
  - ToolErrorMiddleware：优雅捕获并处理工具错误。

这种Agentic（模型驱动）与确定性（中间件驱动）的分离，平衡了可靠性和灵活性。

#### 6. Invocation：Slack、Linear、GitHub集成

- **Slack**：在任意thread中@bot，支持repo:owner/name语法指定仓库，Agent在thread内回复状态更新和PR链接。
- **Linear**：在issue中@openswe评论，Agent读取完整上下文、反应👀确认，并在评论中返回结果。
- **GitHub**：在Agent创建的PR评论中@openswe，Agent会处理review反馈并推送到同一分支。

每个调用生成确定性thread ID，后续消息自动路由到同一Agent实例。

#### 7. Validation：Prompt驱动 + 安全网

Agent被prompt要求在提交前运行linter、formatter和测试；open_pr_if_needed中间件作为后备，确保关键步骤可靠执行。可通过额外中间件扩展CI检查、可视验证或review门控。

### 为什么选择Deep Agents

Deep Agents提供了可组合、可维护的基础设施：
- **上下文管理**：长任务产生大量中间数据，通过基于文件的内存卸载，避免对话历史膨胀。
- **规划原语**：write_todos工具结构化分解任务、跟踪进度、适应新信息。
- **子Agent隔离**：子Agent拥有独立上下文，避免相互污染。
- **中间件钩子**：注入确定性逻辑。
- **升级路径**：Deep Agents作为独立库持续迭代，改进可直接流向Open SWE。

### 针对组织的定制化

Open SWE设计为可定制基础而非成品，所有主要组件均可插拔：
- 沙箱提供商
- 模型（默认Claude Opus 4，可按子任务配置）
- 工具
- 触发器
- 系统prompt
- 中间件

Customization Guide提供各扩展点的详细示例。

### 与内部实现的对比

Open SWE与Stripe、Ramp、Coinbase内部系统的核心模式高度一致，差异主要在于实现细节、内部集成和组织特定工具——这正是框架适应不同环境的预期结果。

### 如何开始使用

- GitHub仓库：github.com/langchain-ai/open-swe
- 安装指南：包含GitHub App创建、LangSmith设置、Linear/Slack/GitHub触发器、生产部署步骤
- 定制化指南：详细说明如何替换沙箱、模型、工具等
- MIT许可，可fork、定制、内部部署

---

## 🔗 原文链接

[点击查看原文](https://x.com/LangChain/status/2033959303766512006?s=20)

---

**🏷️ 标签：** #OpenSWE #LangChain #生产级编码Agent #DeepAgents #AIAgent框架 #内部编码Agent #Agent编排 #开源Agent基础设施 #Sandbox隔离 #子Agent编排
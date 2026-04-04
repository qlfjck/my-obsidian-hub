# Claude Architect 自学指南：无需证书也能掌握生产级AI应用开发

> [!IMPORTANT] Claude Architect 完整自学路径：拆解官方认证考试核心知识，零基础即可构建生产级应用

**📅 发布时间：** 2026年3月15日 **👤 作者：** hoeem（@hooeem） **🔗 来源平台：** X (Twitter)

---

## 💡 核心观点

Anthropic官方的“Claude Certified Architect”认证考试仅限Claude Partner才能参加，但证书本身并不重要。真正重要的是**掌握考试所考查的核心能力**：Claude Code、Claude Agent SDK、Claude API、Model Context Protocol（MCP）。  
学会这些，你就能独立构建生产级AI应用（Customer Support Agent、多Agent研究系统、CI/CD流水线、结构化数据提取等），这些技能直接可变现。  
作者将官方考试指南完整拆解为5大领域（共100%），每个领域列出反模式、正确做法、学习资源、Claude启动Prompt以及“该构建什么”实战项目，供任何人自学。无需证书，只需知识，即可成为真正的Claude Architect。

---

## 📖 详细内容梳理

### 引言：证书不重要，知识才重要

成为Claude Architect需要掌握Claude Code、Claude Agent SDK、Claude API和Model Context Protocol（MCP）。  
官方考试仅限Claude Partner参加，但这并不重要——掌握考试内容就能让你构建生产级应用。  
作者把整个考试指南拆解出来，聚焦真正有价值的知识点，避免大家浪费时间。  
这些技能直接可货币化：客户支持Agent、代码生成、多Agent研究系统、开发者生产力工具、CI/CD集成、结构化数据提取等。

> [!QUOTE] You don't need the certificate to build production-grade applications. You just need the knowledge.

### DOMAIN 1: AGENTIC ARCHITECTURE & ORCHESTRATION（27%）

考试重点考察你能否拒绝三大反模式：  
- 用自然语言判断循环终止  
- 用任意迭代上限作为主要停止机制  
- 检查Assistant文本作为完成指示  

**最大错误**：认为子Agent与Coordinator共享内存——实际上子Agent上下文完全隔离，所有信息必须在Prompt中显式传递。  

**最高分规则**：当涉及金钱或安全关键场景时，仅靠Prompt指令不够，必须通过Hooks和前置门控在程序层面强制工具调用顺序。  

**学习资源**：  
- Agent SDK Overview（了解Agent循环和子Agent模式）  
- Building Agents with the Claude Agent SDK（Anthropic官方最佳实践：Hooks、编排、Session）  
- Agent SDK Python仓库及示例（Hands-on：Hooks、自定义工具、fork_session）  

**启动Prompt**（直接复制到Claude即可进入Domain 1学习）：  
（原文提供具体Prompt，此处为作者推荐的引导Prompt，帮助系统性学习Agent架构）  

**实战项目**：构建一个带3-4个MCP工具的多工具Agent，实现正确的stop_reason处理、PostToolUse Hook规范化数据格式，以及工具调用拦截Hook阻止策略违规。单次练习即可覆盖Domain 1大部分考点。

### DOMAIN 2: TOOL DESIGN & MCP INTEGRATION（18%）

工具描述被严重低估——它是Claude进行工具选择的首要机制。描述模糊或重叠会导致选择不可靠。  

**经典考题**：get_customer和lookup_order描述几乎相同，导致持续误路由。正确解法不是few-shot示例、不是路由分类器、不是合并工具，而是**优化描述**。  

**必须背熟**：tool_choice选项  
- “auto”：模型可能返回文本  
- “any”：必须调用工具，由模型选择  
- 强制指定工具  

**最佳实践**：给Agent 18个工具会严重降低选择可靠性。每个子Agent应限制在4-5个与其角色相关的工具。  

**学习资源**：  
- MCP Integration for Claude Code（服务器作用域、环境变量展开、project vs user config）  
- MCP规范及社区服务器（理解协议、决定何时用社区服务器 vs 自定义）  
- Claude Agent SDK TypeScript仓库（工具定义模式、结构化错误响应）  

**启动Prompt**（直接复制到Claude进入Domain 2学习）：  
（原文提供具体Prompt）  

**实战项目**：构建两个功能相似的MCP工具，先故意写模糊描述导致误路由，再优化描述感受差异。

### DOMAIN 3: CLAUDE CODE CONFIGURATION & WORKFLOWS（20%）

这是区分“会用Claude Code”和“为团队配置Claude Code”的关键领域。  

**CLAUDE.md层级**：三层结构  
- 用户级（~/.claude/CLAUDE.md）  
- 项目级（.claude/CLAUDE.md）  
- 目录级（子目录文件）  

**考试最爱陷阱**：团队成员因指令在用户级配置（不版本控制、不共享）而丢失指令。  

**路径特定规则**：.claude/rules/目录使用YAML frontmatter + glob模式（如**/*.test.tsx），可在整个代码库应用约定。目录级CLAUDE.md无法实现此功能。  

**Plan Mode vs Direct Execution**：  
- Plan Mode：适用于整体重构、多文件迁移、架构决策  
- Direct Execution：适用于单文件Bug修复、单次验证、清晰范围任务  

**学习资源**：  
- Claude Code官方文档（CLAUDE.md层级、rules目录、slash commands、skills frontmatter）  
- Claude Code CLI Cheatsheet（命令、skills、hooks、CI/CD标志）  
- Creating the Perfect CLAUDE.md（真实团队配置模式 + MCP集成）  

**启动Prompt**（直接复制到Claude进入Domain 3学习）：  
（原文提供具体Prompt）  

**实战项目**：构建一个包含CLAUDE.md层级、.claude/rules/ glob模式、带context: fork的skill、以及.mcp.json中带环境变量展开的MCP服务器的项目。分别用Plan Mode做多文件重构、Direct Execution做单Bug修复。

### DOMAIN 4: PROMPT ENGINEERING & STRUCTURED OUTPUT（20%）

整个领域核心两个字：**be explicit**（要足够明确）。  

“Be conservative”无法提升精确度，“Only report high-confidence findings”无法减少假阳性。真正有效的是**明确定义哪些问题该报告、哪些该跳过，并给出每个严重级别的具体代码示例**。  

**Few-shot示例**是最高杠杆技术：2-4个针对性示例，展示模糊案例处理逻辑，并解释为什么选择某一行动而非其他。  

**tool_use + JSON schemas**可消除语法错误，但无法消除语义错误。Schema设计要点：  
- 可为空字段（当源数据可能缺失时，防止模型编造）  
- “unclear”枚举值  
- “other” + detail字符串  

**Message Batches API**：节省50%费用，最长24小时处理，无延迟SLA，适合夜间报告；同步API适合pre-merge阻塞检查。  

**学习资源**：  
- Anthropic Prompt Engineering文档（few-shot模式、明确标准、结构化输出）  
- Anthropic API Tool Use文档（tool_use、tool_choice、JSON schema强制）  
- 考试指南样题Q10-Q12（最佳学习材料，逐一分析每个干扰项为什么错误）  

**启动Prompt**（直接复制到Claude进入Domain 4学习）：  
（原文提供具体Prompt）  

**实战项目**：构建一个使用tool_use的提取流水线（含必填、可选、可为空字段），加入验证-重试循环，通过Batches API运行批量任务，并用custom_id处理失败。

### DOMAIN 5: CONTEXT MANAGEMENT & RELIABILITY（15%）

权重最低，但错误会连锁影响所有领域。  

**Progressive summarisation**会破坏事务性数据。正确做法：建立持久的“case facts”区块，提取金额、日期、订单号等关键信息，永不总结，每条Prompt都包含。  

**Lost in the middle**效应：模型容易忽略长输入中埋藏的信息。关键总结应放在Prompt开头。  

**合理升级触发条件**（仅3种）：  
- 客户明确要求转人工（立即执行）  
- 策略存在空白  
- 无法继续推进  

考试会诱导你选择的两种不可靠触发：情感分析和自我报告的置信度分数。  

**正确错误传播**：结构化上下文（失败类型、尝试过的查询、部分结果、备选方案）。反模式：静默抑制错误或单个失败就终止整个工作流。  

**学习资源**：  
- Building Agents with the Claude Agent SDK（上下文管理、错误传播、升级设计）  
- Agent SDK session文档（resumption、fork_session、/compact）  
- Everything Claude Code仓库（实战上下文管理模式、scratchpad文件、策略性压缩）  

**启动Prompt**（直接复制到Claude进入Domain 5学习）：  
（原文提供具体Prompt）  

**实战项目**：构建一个带两个子Agent的Coordinator，模拟超时场景，验证Coordinator收到结构化错误上下文并用部分结果继续推进。测试冲突来源情况。

### 推荐Anthropic官方学习资源

1. Building with the Claude API  
2. Introduction to Model Context Protocol  
3. Claude Code in Action  
4. Claude 101  

### 结语：成为“非认证”Claude Architect

无论你能否拿到官方证书，掌握以上知识就能真正构建生产级应用。  
作者鼓励大家立即行动：去Claude里粘贴对应Prompt开始学习，然后动手构建实战项目。  
证书只是一个勾，知识才是生产力。  
现在，就去成为Claude Architect吧！

---

## 🔗 原文链接

[点击查看原文](https://x.com/hooeem/status/2033198345045336559)

**🏷️ 标签：** #ClaudeArchitect #ClaudeCode #ClaudeAgentSDK #MCP #AgenticArchitecture #ToolDesign #PromptEngineering #ContextManagement #生产级AI应用 #自学指南
# Claude Code 自进化系统：让AI每一步都自我进化

> [!IMPORTANT] Claude Code 自进化指南：修正自动升级为永久规则，系统代际进化

**📅 发布时间：** 2026年3月29日 **👤 作者：** Meta Alchemist（@meta_alchemist） **🔗 来源平台：** X (Twitter)

---

## 💡 核心观点

AI的未来在于每一步的自我进化，Agent正在自我进化，CLI也应如此。  
作者提供一套完整、可直接复制的Claude Code 自进化系统：通过CLAUDE.md认知核心、路径作用域规则、专用子Agent、记忆系统和进化引擎，让Claude在每次会话中自动捕获你的修正、观察代码模式，当同一修正出现两次时自动升级为永久规则，并通过验证检查机械化执行。  
最终实现：Claude在写代码前先决策框架、委托复杂任务给专用Agent、仅在相关路径加载安全/性能规则、自动进化知识库，系统每代都更适应你的项目，成为真正“活的”AI工程伙伴。

---

## 📖 详细内容梳理

### 开篇：AI自我进化的时代

AI的未来在于每一步的自我进化。  
Agent现在正在自我进化，大量研究从各个角度分享了自我进化系统的益处。  
CLI的未来也将是自我进化的。  
为什么你的Claude Code不能现在就拥有自我进化系统呢？  
如果你准备给Claude插上新翅膀，让它超越初始标准版本、通过会话不断进化，这篇指南就是为你准备的。  
首先，保存这篇文章。因为你需要把它复制粘贴到每一个想让CLI“真正进化”的Claude项目中。

> [!QUOTE] 那其实会进化。  
> 有一个系统：你每次做出的修正都会被捕获、记录；  
> 当同一修正出现两次，它就会突变为Claude永远遵循的永久规则。  
> Claude在工作中发现的代码库模式会被观察并像田野笔记一样记录。  
> 进化系统会审查所有积累的信号，并提出哪些已学行为应该毕业成为永久DNA。  
> 这就是自然选择在你的AI工程系统中的体现。

### 你正在构建的系统

这个系统有四个层级：  
**Layer 1：认知核心**  
一个CLAUDE.md文件，它不只是命令列表，而是编程Claude的思考方式。  
它在写任何代码前运行决策框架，在完成任何事情前检查质量门。  

**Layer 2：专用Agent**  
两个子Agent角色（Architect规划者和Reviewer验证者），它们可以被生成用于专注工作，拥有不同的工具访问权限和模型层级。  

**Layer 3：路径作用域规则**  
安全规则仅在编辑认证代码时加载；API设计规则仅在handlers目录中激活；性能规则在任何地方监控N+1查询。  
Claude的上下文保持精简，因为它只加载当前文件相关的规则。  

**Layer 4：进化引擎**  
一个记忆系统捕获修正和观察；一个技能在你纠正Claude时自动触发；一个定期审查命令将已学模式升级为永久规则。  
这一层让系统真正自我改进而非静态。

### 文件夹结构

你将创建以下结构：  
- 项目根目录  
  - CLAUDE.md（认知核心）  
  - .claude/  
    - settings.json（权限与安全）  
    - rules/（路径作用域规则）  
      - security.md  
      - api-design.md  
      - performance.md  
    - agents/（专用Agent）  
      - architect.md  
      - reviewer.md  
    - commands/（显式工作流）  
      - review.md  
      - fix-issue.md  
      - evolve.md  
    - skills/  
      - evolution/  
        - SKILL.md  
    - memory/（记忆系统）  
      - README.md  
      - learned-rules.md  
      - evolution-log.md  

### 构建步骤

**Step 1：创建文件夹结构**  
直接在项目根目录创建上述文件夹和文件。

**Step 2：CLAUDE.md（认知核心）**  
这是整个系统最重要的文件。它直接加载到Claude的system prompt中，塑造整个会话的思考方式。  
在项目根目录创建CLAUDE.md，内容包括：  
- 决策框架（写代码前必须先grep现有模式、最小变更原则、爆炸半径评估等）  
- 完成标准（5项质量门）  
- 命令部分（你的实际build/test/lint命令）  
- 架构部分（描述真实文件夹结构）  
- 约定部分（根据你的技术栈调整）  

**Step 3：Settings（权限与安全）**  
创建 `.claude/settings.json`，定义允许/拒绝的bash命令。  
允许命令无需确认直接运行；拒绝命令完全阻挡；其余触发确认提示。  
自定义：根据你的工具链（npm/pnpm/make等）调整模式。

**Step 4：Rules（路径作用域智能）**  
创建三个规则文件，仅在匹配路径时加载，保持上下文精简。  
- `security.md`：安全规则  
- `api-design.md`：API设计规则  
- `performance.md`：性能规则  

**Step 5：Agents（专用子Agent）**  
- Architect Agent：专注规划  
- Reviewer Agent：专注验证  
每个Agent有独立system prompt、工具访问权限和模型偏好。

**Step 6：Commands（显式工作流）**  
创建三个slash命令：  
- `/project:review`：提交前审查（运行测试、linter、diff审查）  
- `/project:fix-issue`：修复GitHub issue  
- `/project:evolve`：进化审计（每周运行或重复错误时触发）

**Step 7：进化引擎（学习技能）**  
创建 `.claude/skills/evolution/SKILL.md`，这是系统的核心。  
它在你纠正Claude时自动触发，记录修正，当同一模式出现两次时自动升级为learned-rules.md，并附带验证检查。

**Step 8：记忆系统**  
- `memory/README.md`：记忆系统协议  
- `learned-rules.md`：已学规则（带验证检查）  
- `evolution-log.md`：进化日志  

### 进化循环实际运行方式（Session by Session）

**Session 1**：你纠正Claude使用三元运算符，进化技能记录修正并生成验证检查。  
**Session 3**：同一模式再次出现，自动升级为永久规则并附带grep验证。  
**Session 5**：启动任务前自动运行验证扫描，所有规则通过后继续。  
**Session 8**：Claude主动观察模式并验证无反例，自动升级为规则。  
**Session 12**：运行`/project:evolve`，分析日志并提出毕业/修剪建议。  
**Session 20**：系统已有8条带验证的规则、3条永久规则、2条已修剪，修正次数接近零。

### 自定义建议

**必须自定义**：CLAUDE.md中的Commands和Architecture部分、settings.json中的allow/deny模式。  
**建议自定义**：Conventions、规则路径模式、API设计细节。  
**保持原样**：决策框架、完成标准、记忆系统、Agent定义、通用命令等。

### 测试你的设置

创建所有文件后，进行8项测试：  
- 测试认知核心、决策框架、路径作用域规则、review命令、进化技能、evolve命令、验证引擎、会话评分等。  
所有测试均通过后，系统即可投入使用。

### 常见问题解答

- 是否影响所有Claude项目？否，仅当前项目。  
- 如何全局规则？在`~/.claude/CLAUDE.md`中设置。  
- CLAUDE.md大小限制？控制在150行以内。  
- 记忆文件是否会无限增长？会，但/evolve命令会处理和修剪。  
- 版本控制建议？gitignore部分日志文件，仅提交已学规则。  
- Claude不遵守规则怎么办？检查长度、具体性、是否带verify检查。  
- 支持Python/Go/Rust等其他栈？完全支持，只需调整命令和文件夹结构。

### 一键设置脚本

作者提供bash脚本`setup-claude.sh`，一键创建整个文件夹结构。

### 最终效果

Claude Code现在进化成了一个拥有更多认知和协同系统的版本。  
每个已学规则都带有可机器检查的验证测试。  
系统不依赖Claude“记住”，而是机械执行grep确认合规，仅在失败时提示。  
最好的免疫系统是隐形的——你只会在违规时注意到它。  
Session 1 和 Session 20 之间的差距才是真正价值所在。  
系统会随着使用不断进化，规则越来越贴合你的代码库。

---

## 🔗 原文链接

[点击查看原文](https://x.com/meta_alchemist/status/2038222105654022325)

**🏷️ 标签：** #ClaudeCode #自进化系统 #SelfEvolvingAI #CLAUDEmd #进化引擎 #路径作用域规则 #专用Agent #Vibecoding #AI工程系统 #harnessEngineering
# 2026年每一款CLI都将被Agent调用：人类与AI双重设计指南

> [!IMPORTANT] CLI设计须兼顾人类交互与AI Agent解析

**📅 发布时间：** 2026年3月31日 **👤 作者：** Google Cloud Tech（@GoogleCloudTech，由 @ghchinoy、@Saboo_Shubham_ 和 Zack Akil 撰写） **🔗 来源平台：** X (Twitter)

---

## 💡 核心观点

2026年每一款新构建的CLI都将被AI Agent调用，但绝大多数CLI并未为此做好准备：传统的交互式提示、彩色输出和终端UI在Agent自动解析时会完全失效，而单纯剥离这些特性又会严重损害人类用户体验。  
核心解决方案是“同时为人类与AI设计”——将CLI内部逻辑视为数据引擎，解耦原始结构化数据（JSON/NDJSON）与人类呈现层（TUI），通过结构化可发现性、Agent优先互操作性、配置上下文、错误引导、标志一致性、可视化设计及版本生命周期等7大模式，实现同一命令同时服务两种受众，无需维护两套代码。  
最终目标是让CLI既为人类提供愉悦交互，又为Agent提供可预测、可解析的机器接口，从而在Agent时代成为真正可扩展的基础设施。

---

## 📖 详细内容梳理

### 开篇：CLI面临的Agent时代挑战

每一年构建的CLI都将在某个时刻被Agent调用。  
大多数CLI并未为此做好准备。  
交互式提示、彩色输出以及已成为CLI设计标准假设的终端UI，一旦Agent试图自动解析输出结果，就会瞬间失效。  
但如果简单剥离这些特性，又会让CLI对人类用户的体验变得糟糕。  
你无需在两种受众之间二选一，完全可以同时为人类和AI设计CLI。

> [!QUOTE] Every CLI built this year will be called by an agent at some point. Most aren't ready for it.

### 核心哲学：解耦数据与呈现

设计哲学非常简单：**将数据与呈现解耦**。  
当Agent或脚本调用CLI时，需要的是原始、结构化的数据（如JSON）。  
当人类调用时，需要的是将这些数据渲染成可读、可交互的格式（如TUI）。  
把CLI的内部逻辑视为一个“数据引擎”，终端UI只是其中一种可能的“客户端”，就能同时无缝服务两者。  
同一个`watch`命令，对人类应输出实时更新的TUI，对Agent则应输出NDJSON事件流。

> [!QUOTE] The core philosophy is simple: decouple the data from the presentation.

### 1. 结构化可发现性（Structured discoverability）

CLI的入口点应清晰映射其所有能力。Agent和人类都从`--help`开始。  
按功能而非字母顺序对命令进行分组，例如“任务管理”“信息”“配置”，避免根帮助输出变成文字墙。  
明确标记入口点：在帮助描述中添加提示如“（从这里开始）”或“（典型第一步）”，帮助Agent决定先调用哪个命令。  
为每条命令填充三个字段：  
- **Short（快速扫描）**：一行摘要，5-10个词，以动作动词开头。  
- **Long（深度理解）**：命令做什么、为什么使用、与相似命令有何不同。  
- **Example（立即可用）**：3-5个可直接复制粘贴的具体示例。  

示例比描述更重要。开发者会先复制粘贴，Agent会解析示例来推断标志模式。

### 外部化的Agent指令（Externalized agent directives）

可发现性不应止步于`--help`。不要假设LLM天生知道如何使用你的工具——要明确教它。  
在仓库根目录提供`AGENTS.md`文件，定义交互规则、默认工作流和AI开发者的架构标准。  
对于复杂命令，包含`skills/`目录，存放专用的提示文件，供外部Agent直接摄取。

### 2. Agent优先的互操作性（Agent-first interoperability）

要被Agent使用，CLI必须可解析且可预测。

#### `--json` 无处不在
每条产生数据的命令都应支持`--json`或`--no-tui`标志，输出有效的JSON或NDJSON。  
如果Agent无法解析你的输出，那么你的CLI在Agentic世界中就不存在。

#### 自动检测受众
支持`NO_COLOR`和`[APP]_NO_TUI`环境变量。  
当这些变量被设置，或stdout被管道传输时，自动跳过所有交互元素：无提示、无加载动画、无颜色代码。

#### 非交互回退模式
使用TUI（如Bubble Tea）的命令应提供纯文本或JSON的plain模式。TUI是人类界面，JSON输出是Agent界面——同一命令，两种渲染器。

#### 保护上下文窗口
Agent有严格的token限制。默认输出中主动截断海量文本块并屏蔽敏感密钥，避免压垮Agent上下文窗口或将API密钥泄露到对话日志。  
只有Agent真正需要时，才通过`--full`或`--verbose`等明确标志提供原始未过滤载荷。

#### 预处理并排序数据
Agent不应被迫编写复杂数据操作逻辑来寻找关键信息。CLI应自动预排序输出，让最关键、可行动的项目（例如严重漏洞、无限制密钥、待办任务）始终出现在响应最顶部。

#### 委托状态管理
不要让Agent陷入CLI进程内的交互式状态循环。保持CLI完全无状态，使用引用标识符（如`--task <ID>`）。让后端服务维护长期运行的token上下文和对话历史，CLI仅作为快速传输机制。

### 3. 配置与上下文（Configuration and context）

CLI应理解自身环境，而无需每次调用都附加过多标志。

#### 遵循XDG规范
配置文件置于`~/.config/app/config.yaml`。不在主目录创建点文件，也不要使用专有格式的隐藏文件夹。

#### 命名环境
支持多个配置（local、staging、prod），通过简单`--env`标志切换。  
这对Agent尤其重要：编排器可直接设置`--env prod`，无需了解URL或token细节。

### 4. 错误引导（Error guidance）

不要仅报告错误，要提供解决路径。

#### 上下文提示
当命令因缺少先决条件而失败时，包含一行`Hint:`提示。  
Agent可解析这些提示进行自我纠正，人类用户也无需再去搜索文档。

#### 快速失败
在执行重量级逻辑前，先验证配置和连通性。不要让Agent等待30秒API调用后才发现认证token缺失。

#### 确定性退出码
0：成功  
1：一般错误  
2：无效用法/错误标志  
3：连接/认证失败  
如果CLI在操作失败时仍返回0（仅因“命令本身运行了”），就会破坏所有调用它的自动化流程。

### 5. 标志与参数一致性（Flag and argument consistency）

CLI语法应天生可预测。用户（或Agent）学会一条命令后，直觉应无缝迁移到整个应用。

标准化缩写：如果某命令中`-o`表示`--out-dir`，则绝不能在另一命令中表示`--output`。不一致会破坏Agent推理，也会让人类开发者沮丧。  
位置参数 vs 可选标志：核心必需实体使用位置参数（例如`a2acli get <task-id>`），可选修饰符严格使用`--flags`。  
安全默认值：默认行为永远是最安全、最常见的路径。破坏性操作必须要求明确标志。

### 6. 终端可视化设计（Visual design for terminals）

颜色应服务功能目的，而非纯美观。

#### 语义颜色令牌
使用基于意义的令牌，而非原始颜色名称。这样调色板保持一致，也便于支持亮/暗终端主题。  
- Accent（地标）：标题、组标题、章节标签  
- Command（扫描目标）：命令名、标志  
- Pass（成功）：已完成任务、成功状态  
- Warn（瞬态）：活跃任务、警告、待处理状态  
- Fail（错误）：失败任务、错误、拒绝状态  
- Muted（弱化）：元数据、类型、默认值、预览  
- ID（标识符）：唯一ID（任务ID、技能ID）

#### 经验法则
仅为状态保留颜色：绿色代表成功、黄色代表警告、红色代表错误、灰色代表元数据。  
不要为描述或帮助文本着色——过度着色会造成彩虹输出，什么都无法突出。  
用空白而非颜色表现层级：位置和对齐比颜色更能传达结构。  
同时支持亮暗终端，使用自适应颜色以保持任何主题下的对比度。

### 7. 版本控制与生命周期（Versioning and Lifecycle）

CLI不是静态产物。它运行在动态序列中，版本可能漂移、操作可能被突然中断、安全工作流需要审计轨迹。智能处理这些生命周期事件可防止自动化系统中出现静默失败。

CLI应支持`--version`显示自身版本，并可选择性通知用户自上次运行以来的重要更新。  
优雅处理`SIGINT`（Ctrl+C），避免任何人或Agent中断运行操作时造成数据损坏。  
在写操作中追踪“Actor”（从`git config user.name`或`$USER`派生），以实现可审计性。

### CLI cheatsheet for humans（人类用CLI速查表）

（原文提供视觉速查表，此处略，实际使用时可参考原文图片或描述）

### CLI cheatsheet for agents（Agent用CLI速查表）

人类可使用上述视觉速查表，但Agent需要结构化规则集。可使用Agent Skills格式定义这些交互。  
查看CLI Agent Skill完整示例，了解如何明确教导编码Agent主动且确定性地导航双重受众CLI。

### 端到端工作演示：YouTube CLI

YouTube CLI是最佳范例。  
现在你可以与Agent一起协作管理并审查YouTube频道。  
借助Agent-aware CLI skill，你和Agent可从YouTube API构建所需工具集：获取频道统计、切换视频可见性、上传新内容。  
以前这需要寻找特定MCP服务器或手动解析密集的YouTube API文档。  
完整视频演示已提供（原文附视频链接）。  

但若要Agent可靠执行此类工作流，底层CLI必须为此设计。  
如果`yt-cli`在上传时弹出交互式“Are you sure you want to upload? (Y/n)”提示，或返回无法解析的彩色终端文本，整个自动化就会失败。

### 参考资料

灵感来源于：https://github.com/steveyegge/beads/blob/main/docs/UI_PHILOSOPHY.md  
感谢优秀社区：https://clig.dev/

### 结语

2026年最好的CLI不是终端界面最漂亮的那些，而是人类键入命令和Agent以编程方式调用时同样好用的那些。

---

## 🔗 原文链接

[点击查看原文](https://x.com/GoogleCloudTech/status/2038778093104779537)

**🏷️ 标签：** #CLI设计 #AI Agent #双重受众 #harnessEngineering #结构化输出 #Agent优先互操作 #GoogleCloudTech
# Claude Code 源码泄露：自主Agent时代即将来临

> [!IMPORTANT] Claude Code泄露揭秘：KAIROS自主守护模式即将上线

**📅 发布时间：** 2026年3月31日 **👤 作者：** Elliot Arledge（@elliotarledge） **🔗 来源平台：** X (Twitter)

---

## 💡 核心观点

Anthropic今日因npm包构建失误，将Claude Code CLI v2.1.88的59.8MB source map文件（cli.js.map）一并发布，完整暴露1902个TypeScript源码文件，包含所有system prompt、工具逻辑与隐藏特性。  
源码显示Claude Code正向高度自主Agent演进：KAIROS守护模式将使其成为常驻后台daemon，PROACTIVE模式支持独立工作，COORDINATOR模式实现多Agent编排；同时权限提示有望通过AI分类器自动审批，Undercover模式为内部贡献提供隐身保护，还内置Tamagotchi式虚拟宠物系统。  
这一“意外”暴露了Anthropic对CLI未来的规划——从工具转向始终在线的智能伙伴，安全性、自主性与趣味性并重，用户将迎来更少摩擦、更强自主的编码体验。

---

## 📖 详细内容梳理

### 泄露事件背景

今日早些时候，X用户@Fried_rice发现Anthropic在npm包@anthropic-ai/claude-code v2.1.88中意外包含了一个59.8MB的cli.js.map源映射文件。  
该文件完整嵌入了原始TypeScript源码的sourcesContent字段，这并非黑客入侵，而是构建配置疏忽导致的调试产物被打包进生产环境。  
作者花数小时翻阅源码，总结了最值得关注的特性以及对用户的潜在影响。

> [!QUOTE] This isn't a hack—it's a build configuration oversight where debug artifacts got shipped to production. But it reveals a lot about where Claude Code is heading.

### 自主Agent即将到来（AUTONOMOUS AGENTS ARE COMING）

源码中最频繁出现的特性标志是**KAIROS**，共出现154次。  
根据代码逻辑，KAIROS是一个自主守护模式（autonomous daemon mode），可将Claude Code转变为始终在线的Agent，支持后台会话、“dream”记忆整合、GitHub webhook订阅、推送通知以及基于channel的通信。  

**PROACTIVE**模式出现37次，允许Claude在用户消息间隔中独立工作：系统会发送“tick”提示唤醒Agent，Claude根据提示自主决定下一步行动。  
其system prompt明确写道：“You are running autonomously”并指示模型“look for useful work”以及“act on your best judgment rather than asking for confirmation”。

**COORDINATOR_MODE**出现32次，进一步将Claude升级为编排器，可生成并管理多个并行worker Agent。  
协调器负责研究、实现与验证，并将任务委派给专精worker；system prompt包含详细指导：如何为worker撰写prompt、何时继续执行 vs 生成新Agent、何时处理worker失败等。

> [!TIP] 实用技巧  
> 这些特性暗示未来Claude Code将从被动工具转变为主动常驻伙伴，用户只需设定目标即可让Agent自主推进工作流。

### 权限提示有望消失（PERMISSION PROMPTS MAY DISAPPEAR）

标志**TRANSCRIPT_CLASSIFIER**出现107次，据上下文判断，这是一个“Auto Mode”功能，使用独立AI分类器自动审批工具权限。  
若正式上线，目前频繁中断工作流的权限确认提示将对可信操作实现可选或完全自动跳过。

### 模型代号与版本信息（MODEL CODENAMES AND VERSIONING）

源码暴露了Claude模型的内部代号：  
- **Capybara**：疑似Claude 4.6变体，注释提及“Capybara v8”已修复29-30%的虚假声明率（v4为16.7%）、过度注释代码倾向及“assertiveness counterweight”等问题。  
- **Fennec**：曾为代号，后迁移至Opus 4.6。  
- **Numbat**：尚未发布，某注释写明“Remove this section when we launch numbat。”  
此外还提及opus-4-7、sonnet-4-8等仅供内部使用的版本号，提示这些版本已在内部存在。

> [!NOTE] 补充说明  
> Capybara物种名称在Tamagotchi系统中使用String.fromCharCode()混淆，正是为了规避内部泄露检测扫描器，进一步印证其为敏感模型代号。

### 隐身贡献模式（UNDERCOVER MODE FOR STEALTH CONTRIBUTIONS）

**Undercover Mode**专为Anthropic员工向公开仓库贡献代码设计。  
启用后会自动剥离所有AI归因、隐藏模型代号、移除“Claude Code”或AI相关提及，甚至不告知模型自身身份。  
prompt明确要求：“You are operating UNDERCOVER in a PUBLIC/OPEN-SOURCE repository. Your commit messages, PR titles, and PR bodies MUST NOT contain ANY Anthropic-internal information. Do not blow your cover.”  
该模式无强制关闭开关，若系统无法确认处于Anthropic内部仓库，则默认保持开启。

### 语音模式（VOICE MODE）

**VOICE_MODE**出现46次，已集成语音转文本与文本转语音功能，支持用户通过语音与Claude Code交互。

### Tamagotchi宠物系统（A TAMAGOTCHI PET SYSTEM）

源码中隐藏了一个完整的**BUDDY**系统，相当于终端里的Tamagotchi虚拟宠物。  
包含18种物种（鸭子、鹅、blob、猫、龙、章鱼、猫头鹰、企鹅、乌龟、蜗牛、幽灵、斧头鱼、capybara、仙人掌、机器人、兔子、蘑菇、chonk），稀有度分级（传说级仅1%），以及帽子等装饰（皇冠、高顶帽、螺旋桨、光环、巫师帽、豆豆帽、小鸭子）。  
宠物拥有DEBUGGING、PATIENCE、CHAOS、WISDOM、SNARK等属性，并支持闪亮变体。  
capybara物种名被特别混淆，进一步证实其敏感性。

> [!QUOTE] The buddy system is delightful and suggests Claude Code will feel less like a tool and more like a companion.

### 其他值得注意的标志（OTHER NOTABLE FLAGS）

- **FORK_SUBAGENT**：支持将自身fork为并行Agent。  
- **VERIFICATION_AGENT**：提供独立对抗性验证。  
- **ULTRAPLAN**：高级规划能力。  
- **WEB_BROWSER_TOOL**：浏览器自动化工具。  
- **TOKEN_BUDGET**：显式token预算控制，支持“+500k”或“spend 2M tokens”等指令。  
- **TEAMMEM**：跨用户团队记忆同步。

### 整体意义与启示（WHAT THIS MEANS）

作者总结几点关键 takeaway：  
1. Claude Code正大幅提升自主性：KAIROS、PROACTIVE与COORDINATOR特性暗示未来可作为后台daemon监控仓库并自主行动。  
2. 权限摩擦正在被解决：transcript classifier有望让自动审批成为现实，减少反复确认。  
3. 模型版本远比公开API复杂：存在内部变体、快速模式及针对特定问题的优化。  
4. 安全性高度重视：仅Bash命令验证代码就超过2500行，另有沙箱、Undercover模式及输入清洗机制。  
5. 产品正注入人格化设计：Tamagotchi系统让工具更像伙伴而非冷冰冰的命令行。

### 如何自行查看（HOW TO SEE IT YOURSELF）

源码目前仍可从npm获取：下载@anthropic-ai/claude-code@2.1.88，找到cli.js.map文件，解析JSON并提取sourcesContent字段即可。  
作者强调不会重新分发代码，但公开讨论已可访问的产物属于合理范围。  
原始发现者为@Fried_rice。

---

## 🔗 原文链接

[点击查看原文](https://x.com/elliotarledge/status/2038934884761444838)

**🏷️ 标签：** #ClaudeCode #源码泄露 #自主Agent #KAIROS #PROACTIVE #COORDINATOR_MODE #UndercoverMode #Tamagotchi
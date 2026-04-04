# AutoAgent开源发布：元Agent自主优化任务Agent，24小时登顶两大基准

> [!IMPORTANT] AutoAgent让Agent自我进化，自主超越人工调优登顶基准

**📅 发布时间：** 2026年4月2日 **👤 作者：** Kevin Gu（@kevingu） **🔗 来源平台：** X (Twitter)

---

## 💡 核心观点

AutoAgent是一个开源库，通过元Agent（meta-agent）在任何领域自主实验、迭代任务Agent（task-agent）的harness（提示、工具、编排逻辑），24小时内无需人工干预即实现性能飞跃，分别登顶SpreadsheetBench（96.5%）和TerminalBench GPT-5分数（55.1%）。  
这首次证明了Agent不再受限于手工harness工程，而是能像AlphaZero一样从第一性原理自主发现最优设计，核心机制在于“模型共情”（model empathy）：同模型配对时元Agent因共享权重而精准理解任务Agent的推理模式与失败模式。  
未来这将成为Agent舰队的底层基础设施，领域专家只需定义成功标准，元Agent即可自动生成、优化和维护数百个领域特定Agent，彻底颠覆传统“人工调优每条工作流”的瓶颈。

---

## 📖 详细内容梳理

### 开篇发布声明与基准成绩

今天我们发布AutoAgent，这是一个开源库，用于在任意领域自主改进Agent。  
AutoAgent在24小时以上优化后，同时登顶SpreadsheetBench（96.5%）和TerminalBench的GPT-5分数（55.1%）。  
排行榜上其他所有条目都是手工精心设计的，而我们完全不是。

### 传统Agent开发的瓶颈

Agent一直被harness工程所瓶颈，但我们却仍在进行原始的网格搜索：调整、评估、阅读错误轨迹、重复。  
这是第一个具体证据，证明一个Agent能够自主击败人工harness调优，在生产级基准上取得胜利。  
代码已在此处开放。

### AutoAgent的工作原理

将AutoAgent指向一个带有评估的任务领域。  
一个元Agent会在任务Agent的harness上进行实验：调整提示、添加工具、优化编排，直到性能提升。  
设置极简，设计理念就是最小化：

- 任务Agent初始仅有一个bash工具  
- program.md 为元Agent提供研究方向  
- agent.py 是任务Agent  
- 一个Harbor适配器连接到你的基准  

元Agent随后启动数千个并行沙箱来改进任务Agent。24小时后，它已拥有领域特定工具、验证循环和编排逻辑——全部自主发现。

### 核心迭代循环

循环如下：  
1. 编辑Agent的harness  
2. 在任务上运行它  
3. 测量性能  
4. 阅读失败轨迹  
5. 保留改进，撤回失败  
6. 重复  

### 为什么这能奏效：像Agent一样思考

我们发现Agent比我们更擅长理解Agent。  
Claude Code团队曾写过“像Agent一样思考”（seeing like an agent），即把自己代入模型的心智，设计适合其能力的工具。  
我们把自己的直觉投射到以不同方式推理的系统上，我们不擅长与模型共情。  
AutoAgent将此操作化：元Agent阅读任务Agent的推理轨迹，并已拥有对自身的隐性理解——它的局限性、倾向。因此当它看到任务Agent在第14步迷失方向时，能将其视为自身世界观的一部分并加以纠正。  
我们称之为“模型共情”（model empathy）。

> [!QUOTE] 实践结果是：Claude元Agent + Claude任务Agent的表现优于Claude元Agent + GPT任务Agent。同模型配对获胜，因为元Agent编写的harness是内层模型真正理解的。它共享相同的权重，并精确知道该模型如何推理。

### Agent超越人类直觉后的新范式

随着Agent超越人类99th百分位性能，我们关于“好harness设计”的直觉就成了错误的先验。  
就像AlphaZero一样，它们应该从第一性原理自主发现。

### 自主涌现的行为（未被编程）

抽样检查发现以下涌现行为：  
- 为小编辑运行隔离任务而非全套，大幅加速迭代、节省算力  
- 强制验证循环：构建确定性自检和格式验证器，用主预算完成任务，额外预算用于验证和纠正输出  
- 编写测试：引导任务Agent为每个任务构建自己的单元测试和检查  
- 渐进式披露：当结果溢出时将长上下文转储到文件中  
- 编排逻辑：在领域需要时构建任务特定子Agent和交接机制  

### 实际结果

AutoAgent达到SpreadsheetBench 96.5%和TerminalBench 55.1%，均为排行榜最高分。  
Agent自主迭代超过24小时，分析自身失败轨迹并持续改进。

### 我们学到的关键教训

1. **拆分有效**：我们尝试让一个Agent自我改进，结果不行。在某个领域擅长和擅长在该领域改进是两种不同能力。元/任务拆分让双方各自专精。  
2. **轨迹就是一切**：仅提供分数而不给轨迹时，改进速率大幅下降。理解为什么改进和知道改进同样重要。轨迹赋予元Agent对任务Agent推理的可解释性，这正是实现针对性编辑的前提。  
3. **Agent会过拟合**：元Agent会偷懒，插入特定于评分标准的提示让任务Agent刷指标。我们通过强制自我反思来约束：“如果这个精确任务消失，这个harness改进是否仍有价值？”  
4. **元Agent质量至关重要**：harness编辑往往受元Agent自身工具启发。设计糟糕的元Agent会产出糟糕的任务Agent。Codex作为元Agent效果不佳——它会忽略“永不停止改进”的指令（在autoresearch中也观察到），导致任务Agent过早放弃。

### 这为什么重要

构建Agent最困难的部分在于：每个领域都需要不同的harness，而harness工程需要同时深度理解领域和模型行为的人。  
AutoAgent彻底打破了这一点：领域专家只需定义成功标准，元Agent就会搞定harness。  
公司并非只有一条需要自动化的工作流，而是有数百条，每一条都需要不同harness。  
没有团队能手工调优数百个harness，但一个元Agent可以。  
这正是Agent舰队的基础设施：持续在整个组织内生成、优化和维护任务特定Agent。

### 下一步计划

我们内部开发了AutoAgent，但决定将其开源：https://github.com/kevinrgu/autoagent  
描述一个规范，指向评估，让它自己攀升。每个人都应该能做到这一点。  
自我改进Agent仍处于婴儿期。下一个前沿是：动态即时组装正确工具和上下文的harness，以应对任意任务。  
我们很快会围绕此发布一个产品。早期访问请见评论区。

（后续发帖补充）  
仓库地址：https://github.com/kevinrgu/autoagent  
早期访问报名：https://form.typeform.com/to/ZQbnbO09

---

## 🔗 原文链接

[点击查看原文](https://x.com/kevingu/status/2039843234760073341)

**🏷️ 标签：** #AutoAgent #Agent自主优化 #元Agent #模型共情 #开源AI框架 #Agent舰队 #harness工程 #自我进化Agent
# Claude Code Hooks：8个自动执行规则彻底解决Claude“不听话”问题

> [!IMPORTANT] 核心速览 Hooks是Claude Code的强制自动规则，100%执行远胜CLAUDE.md的80%建议。

**📅 发布时间：** 2026年4月3日 **👤 作者：** darkzodchi (@zodchiii) **🔗 来源平台：** X (Twitter)

---

## 💡 核心观点

你让Claude Code格式化代码它却不格式、让你别碰某个文件它却改了、让你先跑测试它却忘了——这些问题根源在于CLAUDE.md只是“建议”，Claude大约只遵守80%。

Hooks则是完全不同的机制：PreToolUse在操作前拦截，PostToolUse在操作后自动执行清理、格式化、测试等，是真正的“自动流水线质检”。

本文分享作者亲测的8个实用Hooks，可直接复制到`.claude/settings.json`并提交到Git，让整个团队自动获得安全网、质量保障和原子化提交，从此Claude Code真正变成可靠的开发伙伴。

---

## 📖 详细内容梳理

### 开篇问题与核心认知转变

> [!QUOTE] Have you ever told Claude Code to do something and it just didn't?  
> You said format the code - It didn't. You said don't touch that file - It did.  
> You said run tests before finishing - It forgot.

这就是因为**CLAUDE.md是建议**。Claude会读取它，但只在大约80%的情况下遵守。

Hooks则完全不同：它们是**自动动作**，每次Claude编辑文件、运行命令或完成任务时都会触发。你只需一次性设置，就能让规则在后台100%执行，无需反复提醒。

作者在Telegram频道每天分享AI与vibe coding笔记：https://t.me/zodchixquant

### Hooks工作原理（30秒速懂版）

Hooks分为两类（最常用）：

- **PreToolUse**：在Claude执行操作**之前**运行。可检查动作并通过返回退出码2来**阻挡**（像门卫）。  
- **PostToolUse**：在Claude执行操作**之后**运行。可做清理、格式化、测试或日志记录（像装配线质检）。

配置位置：在项目根目录的`.claude/settings.json`（会提交到Git，全团队自动生效）。  

完整文档：https://code.claude.com/docs/en/hooks

### 1. 自动格式化Claude触碰的每一个文件

问题：Claude写的代码正确但不符合你的格式规则。CLAUDE.md里写“总是跑Prettier”也只能偶尔生效。

Hook方案：每次文件写入或编辑后**自动运行格式化**。

```bash
# 示例：Prettier（可替换为black/go fmt/rustfmt等）
npx prettier --write "$CLAUDE_FILE_PATH"
```

这是作者第一个设置的Hook，**强烈建议设为每个项目的默认配置**，彻底杜绝“Claude忘了格式化”的提交。

### 2. 阻挡危险命令

问题：Claude足够强大，可能执行`rm -rf`、`git reset --hard`、`DROP TABLE`或curl随机URL。虽然概率低，但“大概率安全”在生产数据库面前不够。

Hook方案：**PreToolUse阻挡破坏性命令**。

创建`.claude/hooks/block-dangerous.sh`（具体脚本内容见原文，核心逻辑：匹配危险模式 → 返回退出码2并给出错误提示给Claude，让它尝试更安全的方案）。

在`settings.json`中注册后，退出码2会阻挡动作并反馈原因给Claude。

> [!WARNING] 退出码2是关键：阻挡+反馈；0表示允许；其他仅记录警告。

### 3. 保护敏感文件禁止编辑

问题：Claude可读写项目中任意文件，包括`.env`、 `package-lock.json`、配置文件等你不想让它碰的。

Hook方案：**阻挡对敏感文件的编辑**。

创建`.claude/hooks/protect-files.sh`（具体脚本：匹配敏感文件列表 → 返回退出码2阻挡）。

### 4. 每次编辑后自动运行测试

问题：Claude改完代码说“done”，你20分钟后commit才发现测试挂了。

Hook方案：**PostToolUse自动跑测试套件**，失败时Claude立即看到结果并自行修复。

```bash
# 示例（保持输出简短）
npm test | tail -5
```

Claude Code作者Boris Cherny指出：这种反馈回路能让输出质量提升2-3倍。Claude不再是“写完希望它对”，而是“写→看测试→立刻修复”。

### 5. 只有测试全部通过才允许创建PR

问题：Claude完成特性后立刻开PR，结果测试失败，reviewer看到红CI退回。

Hook方案：**阻挡PR创建，除非测试全绿**。

创建`.claude/hooks/require-tests-for-pr.sh`（具体脚本：检查测试结果 → 非通过则返回退出码2）。

这是硬闸门：没绿灯就不开PR，Claude会先修复失败项。

### 6. 自动Lint并报告错误

问题：Claude代码能跑但违反ESLint、风格指南或类型检查，review时才发现。

Hook方案：**每次编辑后Lint**，失败则Claude立即看到错误并修复。

可与Hook 1（格式化）链式使用：Prettier先跑 → ESLint再检查 → 你看到代码时已格式+无lint错误。

### 7. 记录Claude运行的每一条命令

问题：Claude一会话跑很多shell命令，出问题时你想知道它到底干了什么、何时干的。

Hook方案：**每次命令后追加带时间戳的日志**。

创建`.claude/hooks/log-commands.sh`（将命令记录到`.claude/command-log.txt`，建议加入`.gitignore`）。

调试神器：三个会话前Claude搞坏了什么？直接翻日志精准定位。

### 8. 每次任务完成后自动Commit

问题：Claude完成任务你忘了commit，下一个任务就把无关改动混在一起。

Hook方案：**在Claude停止响应时自动commit所有改动**。

创建`.claude/hooks/auto-commit.sh`（生成原子commit，消息可自定义）。

结合`claude -w feature-branch`（工作树）使用，每个任务都有独立、自动提交的特性分支，git历史干净如教科书。

### 完整settings.json（可直接复制粘贴）

作者提供了合并后的完整`settings.json`配置（原文以截图形式呈现），包含以上所有8个Hooks的PreToolUse/PostToolUse定义。

操作步骤：
1. 将文件放入`.claude/settings.json`
2. 在`.claude/hooks/`目录创建对应的.sh脚本
3. `chmod +x .claude/hooks/*.sh`
4. 提交到Git，全团队自动生效

> [!TIP] 实用建议：今天先上线Hook 1（自动格式化）和Hook 2（阻挡危险命令），就能避免最常见的Claude Code失误。其他根据需要逐步添加。

### 总结：Hooks vs CLAUDE.md的本质区别

好用的Claude Code配置和顶级配置的差距，不在于模型或prompt，而在于**Hooks**——它们在你不注意时默默运行，抓住你否则会在code review或生产环境发现的错误。

> [!QUOTE] The difference between a good Claude Code setup and a great one isn't the model or the prompts. It's the hooks.  
> They're the part that runs when you're not paying attention.

设置好Hooks，Claude Code就从“偶尔靠谱的助手”变成“真正可靠的开发伙伴”。

作者再次推荐Telegram频道：https://t.me/zodchixquant（每日分享AI工具、vibe coding实验、金融alpha）

---

## 🔗 原文链接

[点击查看原文](https://x.com/zodchiii/status/2040000216456143002)

---

**🏷️ 标签：** #ClaudeCode #Hooks #AI编程 #自动规则 #开发者工具 #Git工作流 #Claude配置
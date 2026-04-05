[https://github.com/travisvn/awesome-claude-skills](https://github.com/travisvn/awesome-claude-skills) 这个仓库干啥的,怎么用  
[https://github.com/travisvn/awesome-claude-skills](https://github.com/travisvn/awesome-claude-skills) 仓库是用来做什么的，怎么用

18:04

这是一个 **Claude Skills（技能）的精选资源列表** ，类似于 GitHub 上各种 "awesome-xxx" 合集，专门收录了各类 Claude Skills、教程和工具。

---

## 🧠 什么是 Claude Skills？什么才是克劳德技能？

Skills 是教 Claude 以可重复方式执行任务的专属能力 [github](https://github.com/travisvn/awesome-claude-skills) ，本质上是一个 **文件夹** ，里面包含指令、脚本和资源，Claude 在判断任务需要时会自动加载对应的 Skill。

**工作原理（渐进式加载）：**

1. 先扫描所有 Skills 的元数据（~100 tokens），判断哪个相关
2. 需要时才加载完整指令（<5k tokens）
3. 脚本和资源按需加载

这样多个 Skills 可以同时待机，却不会撑爆 context window。

---

## 🚀 怎么用？

**在 Claude.ai 网页端：**

1. 进入 [Settings > Capabilities](https://claude.ai/settings/capabilities)  
	进入 [设置 > 功能](https://claude.ai/settings/capabilities)
2. 开启 Skills 开关
3. 浏览或上传自定义 Skills （注意：需要 Pro/Max/Team/Enterprise 账号，免费用户不支持）

**在 Claude Code CLI： 在 Claude Code CLI 中：**

```bash
# 从官方市场安装
/plugin marketplace add anthropics/skills

# 或从本地目录安装
/plugin add /path/to/skill-directory
```

---

## 📦 仓库收录了什么？

主要分三大类：

**官方 Skills（Anthropic 出品）：** 文档处理（docx/pdf/pptx/xlsx）、设计创作、前端开发、品牌规范等

**社区 Skills（第三方贡献）：** iOS 模拟器自动化、Playwright 浏览器控制、D3.js 可视化、安全审计、Expo 开发等

**工具和教程：** 如何自己创建 Skill、最佳实践、安全注意事项等

---

## ✏️ 自己写一个 Skill 最简结构

```
my-skill/
├── SKILL.md          ← 主文件，包含 YAML 元数据 + 指令
├── scripts/          ← 可选脚本（Python/JS 等）
└── resources/        ← 可选资源文件
```

`SKILL.md` 示例：

```yaml
---
name: my-skill
description: 一句话描述，用于让 Claude 发现这个 Skill
---

# 详细指令内容...
```

---

**一句话总结：** 这个仓库就是 Claude Skills 的"导航站"，帮你找到现成的 Skill 来扩展 Claude 的能力，或者学习如何自己制作一个。

> /plugin marketplace add anthropics/skills ⎿ Successfully added marketplace: anthropic-agent-skills ───────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────> ───────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────? for shortcuts  
> /plugin marketplace add anthropics/skills ⎿ 市场成功添加：anthropic-agent-skills ⎿ ───────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────> ───────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────?用于快捷键

18:05

看起来已经成功添加了官方 Skills 市场！现在可以浏览和安装具体的 Skill。

**下一步：查看可用的 Skills**

```bash
/plugin list
```

或者直接安装某个具体的 Skill，比如：

```bash
# 文档处理类
/plugin add anthropic-agent-skills/docx
/plugin add anthropic-agent-skills/pdf
/plugin add anthropic-agent-skills/pptx
/plugin add anthropic-agent-skills/xlsx

# 前端开发
/plugin add anthropic-agent-skills/frontend-design

# 创建你自己的 Skill（推荐新手用）
/plugin add anthropic-agent-skills/skill-creator
```

**安装后的用法很简单** ，直接用自然语言告诉 Claude 做事就行，它会自动识别并加载对应 Skill。比如：

- "帮我把这个 markdown 内容转成 Word 文档" → 自动触发 `docx` skill
- "帮我生成一份 PPT" → 自动触发 `pptx` skill

末尾的 `? for shortcuts` 提示你可以输入 `?` 查看所有快捷键，很有用。

都是英文看不懂啊

18:06

哈哈，Skills 的 **指令文件是英文的** ，但这不影响你用中文跟 Claude 对话！Skills 只是在幕后给 Claude 看的说明书，你自己还是正常说中文就行。

---

## 实际使用完全没有语言障碍

比如你安装了 `pptx` skill 之后，直接说：

> **"帮我做一个关于人工智能发展史的 PPT，10 页左右"**

Claude 会自动读取 skill 指令（英文），然后 **用中文回复你** ，生成的 PPT 内容也可以是中文。

---

## 你现在最可能用到的命令

```bash
# 查看已安装的 skills
/plugin list

# 安装 skill（复制粘贴就行，不用看懂英文）
/plugin add anthropic-agent-skills/docx
/plugin add anthropic-agent-skills/pdf  
/plugin add anthropic-agent-skills/pptx
/plugin add anthropic-agent-skills/xlsx
```

安装完之后， **忘掉那些英文** ，直接用中文跟 Claude 说你要做什么就行了。Skills 在后台自动工作，你感受不到它的存在，只会感受到 Claude 突然"变强了"。

---

有没有具体想做的事情？我帮你判断要装哪个 Skill 🙂

> /plugin list ─────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────── Plugins Discover Installed Marketplaces Errors Discover plugins (1/126) ╭─────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────╮ │ ⌕ Search… │ ╰─────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────╯ > ( ) adspirer-ads-agent · claude-plugins-official Cross-platform ad management for Google Ads, Meta Ads, TikT… ( ) agent-sdk-dev · claude-plugins-official Development kit for working with the Claude Agent SDK ( ) ai-firstify · claude-plugins-official AI-first project auditor and re-engineer based on the 9 des… ( ) ai-plugins · claude-plugins-official Set up endorctl and use Endor Labs to scan, prioritize, and… ( ) aikido · claude-plugins-official Aikido Security scanning for Claude Code — SAST, secrets, a… ↓ more below type to search · Space to toggle · Enter to details · Esc to back  
> /plugin list ────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────── 插件 Discover Installed Marketplaces Errors Discover plugins (1/126) ╭─────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────╮ │ ⌕ 搜索… │ ╰─────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────╯ > ( ) adspirer-ads-agent · claude-plugins-official 平台间广告管理，用于 Google Ads。 元广告TikT… ( ) agent-sdk-dev · claude-plugins-official 用于工作 Claude Agent SDK 的开发套件 ( ) ai-firstify · claude-plugins-official 基于 9 个设计的 AI-第一项目审核员和重新设计者 ( ) ai-plugins · claude-plugins-official 设置 endorctl 并使用 Endor Labs 进行扫描、优先级设置和… （）空手道·克劳德·官方插件 Aikido Security 检测 Claude Code — SAST、秘密、a… ↓ 下面更多 类型搜索·空格切换·详细信息输入·Esc 返回

18:06

这是 Claude Code 的插件市场界面！总共有 **126 个插件** 可以安装。

---

## 怎么操作这个界面

| 按键 | 作用 |
| --- | --- |
| **直接打字** | 搜索插件名 |
| **空格键** | 选中/取消选中插件 |
| **回车键** | 查看插件详情 |
| **↑↓ 方向键** | 上下移动 |
| **Esc** | 返回上一级 |

---

## 推荐你搜索这几个（直接输入关键词）

输入 `docx` → 找到文档处理插件 输入 `pdf` → 找到 PDF 插件  
输入 `pptx` → 找到 PPT 插件 输入 `xlsx` → 找到 Excel 插件 输入 `frontend` → 找到前端设计插件

找到后 **按空格选中** ，再按回车确认安装。

---

或者直接关掉这个界面（按 `Esc` 或 `q` ），用命令行安装更快：

```bash
/plugin add anthropic-agent-skills/pptx
```

你平时主要想用 Claude Code 做什么？我帮你推荐装哪几个最值。

插件市场对应的网页版在哪，我想去网页版看介绍，然后再选择是否下载，还有第三方插件怎么安装呢

18:08

找到了！目前有几个网页版的插件市场可以逛：

---

## 🌐 网页版插件市场（可以先看介绍再决定）

| 网站 | 特点 |
| --- | --- |
| **[buildwithclaude.com](https://buildwithclaude.com/)** | 收录 495+ 个插件，涵盖 Skills、MCP、命令等 [Buildwithclaude 用 Claude 构建](https://buildwithclaude.com/) |
| **[claudemarketplaces.com](https://claudemarketplaces.com/)** | 社区驱动，支持投票和评论，收录 95+ 个市场 [Claudemarketplaces 克劳德市场](https://claudemarketplaces.com/) |
| **[aitmpl.com/plugins aitmpl.com/插件](https://www.aitmpl.com/plugins/)** | 单一入口，汇聚 Skills、Agent、Commands、Hooks 等各类扩展 [Aitmpl](https://www.aitmpl.com/plugins/) |
| **[github.com/anthropics/claude-plugins-official](https://github.com/anthropics/claude-plugins-official)** | Anthropic 官方维护的高质量插件目录，分官方插件和第三方合作插件两类 [GitHub](https://github.com/anthropics/claude-plugins-official) |

---

## 📦 第三方插件怎么安装

看好了某个插件之后，安装方式有两种：

**方式一：先添加它的市场，再安装**

```bash
# 第一步：添加第三方市场（每个市场只需加一次）
/plugin marketplace add 作者名/市场名

# 第二步：安装具体插件
/plugin install 插件名@市场名
```

**方式二：直接从 GitHub 安装（不经过市场）**

```bash
/plugin add github地址
# 例如：
/plugin add https://github.com/obra/superpowers
```

---

举个例子，比如你在 buildwithclaude.com 看到一个插件，页面上通常会显示安装命令，直接复制粘贴到 Claude Code 就行，不需要自己拼命令。

我说官方的插件市场网页是哪个

18:12

找到了！官方插件市场的网页版地址是 **[claude.com/plugins](https://claude.com/plugins)** [Claude](https://code.claude.com/docs/en/discover-plugins) ，这是 Anthropic 官方提供的。

另外官方插件的 GitHub 目录也可以看介绍： **[github.com/anthropics/claude-plugins-official](https://github.com/anthropics/claude-plugins-official)**

---

不过说实话，目前官方插件市场的网页版内容比较简单，GitHub 页面上每个插件的介绍反而更详细一些。如果想看完整说明，建议直接去 GitHub 仓库点开对应插件的文件夹看。

已发出:12条 剩余:8条 (23:04重置)

[Claude is AI and can make mistakes. Please double-check cited sources.  
克劳德是人工智能，可能会犯错。请仔细核对来源。](https://support.anthropic.com/en/articles/8525154-claude-is-providing-incorrect-or-misleading-responses-what-s-going-on)
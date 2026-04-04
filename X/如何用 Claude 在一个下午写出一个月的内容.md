# 每周3-4小时内容创作？用Claude+n8n系统一下午搞定全年内容：20分钟/天即可完成写作

> [!IMPORTANT] 核心速览 Claude+n8n自动化内容研究与idea生成，保留人类写作真实性，内容创作从一天降至20分钟/天。

**📅 发布时间：** 2026年3月29日 **👤 作者：** Aaron (@IAmAaronWill) **🔗 来源平台：** X (Twitter)

---

## 💡 核心观点

大多数人每周花3-4小时（全年150+小时）写内容，作者过去每天写24条推文、一周168条内容耗费整整一天，现在靠自建系统只需20分钟/天即可完成写作。

系统核心在于**系统化除写作以外的所有步骤**：用Claude建立个人/受众/内容简报 → n8n+Apify每周抓取同 niche 顶级创作者推文 → Claude基于简报分析痛点、生成原创idea → Notion结构化存储，最后人工编辑筛选并亲自写作，确保声音真实而非AI泛化。

最终结论是：内容创作的瓶颈不是“写得慢”，而是“想什么、是否值得写、受众是否需要”等前置思考；自动化这些后，你只需执行写作，长期运行还能让系统越来越懂你的历史与受众，实现高质量内容机器化产出，同时保持个人独特视角。

---

## 📖 详细内容梳理

### 一、内容创作的真实痛点：过去每天168条耗费一整天

大多数人每周花3-4小时写内容、帖子、邮件和文案，一年累计150+小时。

作者回忆自己曾经每天写24条推文，每小时一条，一周7天共168条内容（还不包括newsletter、文章、长文等），仅X平台就如此。

> [!QUOTE] How long do you think that took me? Because I think it was around 1 entire day to get 168 pieces of content done.

很多人以为“写内容”只是想一个好点子就行，但其实是一个漫长过程：
- 搞清楚人们在挣扎什么
- 找出解决方案
- 用240字符以内表达出来
- 实际写出来
- 希望它能表现好

重复以上步骤，时间就累积起来了。

### 二、现在作者每天只用20分钟写内容：靠一套系统

作者最近每天写内容只需约20分钟，靠的就是他自建的一套系统。

> [!QUOTE] It systemises every single step except for the writing. I'll get to why I don't systemise the writing later.

系统把除“写作”之外的所有步骤全部系统化，写作部分留给人类自己完成。

### 三、系统搭建步骤详解

#### Step 1: Research（研究阶段）——在Claude中建立完整简报

打开Claude，明确告诉它：
- 你是谁、你做什么
- 你的目标受众是谁、你想帮助他们达成什么
- 你平时发什么类型的内容、它的职责是什么

让Claude为你写一份完整的**brief**（简报），涵盖你、你的受众和你的内容策略。

现在你就拥有了一份可复用的完整简报，后续所有自动化都基于此。

#### Step 2: Build（构建自动化管道）——用n8n搭建工作流

前往n8n，连接以下节点：
- Trigger（触发器）→ Apify → Claude → Notion

**各节点详细作用**：
- **Trigger**：每周固定某一天自动触发（作者推荐每周运行一次）。
- **Apify**：你的研究代理。它会抓取你指定的5-10个同niche创作者最近的50-100条推文，作为原始数据向下游传递。
  > [!QUOTE] You're not copying them. You're using them as a signal for what topics, formats, and angles are already working in your space.
- **Claude**：整个系统的大脑。把Step 1的简报喂给它，它会用你的受众、niche、内容目标为镜头，分析抓取到的内容，找出模式：
  - 反复出现的痛点
  - 没被好好回答的问题
  - 已经过度饱和的角度
  然后生成一批**原创内容idea**，按主题、格式、awareness level（意识层级）组织。不是改写或总结，而是基于真实信号的原创角度。
- **Notion**：输出层。所有idea自动存入结构化数据库，包含Hook、角度、格式、awareness level，并打好标签和排序。

当你坐下来写作时，不再从零开始，而是从菜单里挑选idea。

#### Step 3: Set Your Content Categories（设定内容分类）

在让系统乱跑之前，必须给它明确方向。

不是所有内容功能都一样：
- 有些建立认知
- 有些建立信任
- 有些直接转化

作者的分类方式：
- **Low awareness content**（低认知内容）：针对还不认识你的人。大问题陈述、可共鸣的痛苦、无行话。是你的增长帖。
- **Mid awareness content**（中认知内容）：针对知道问题但还没找到解决方案的人。教育性、实用性、稍具体。是建立权威的帖。
- **High trust content**（高信任内容）：针对已经在你世界里的人。案例研究、成果、观点、幕后。是转化帖。

明确告诉Claude生成哪一类，或要求混合生成。这样能避免内容日历全是相似声音的帖子。

#### Step 4: Editorial（编辑筛选阶段）

大多数人跳过这一步，导致AI生成的内容听起来像AI。

系统会产生比你需要的更多idea（这是故意的，给你选择空间）。

你的工作是**像编辑一样**，而不是作家：
- 打开Notion数据库
- 杀掉所有听起来不像你的idea
- 杀掉所有泛化、感觉AI味重的
- 杀掉所有无法让你自己中途停下来刷到的

剩下的就是真正值得写的短名单。

> [!QUOTE] This step takes about 10 minutes. Maybe 15 if you're being thorough. But it's what separates content that builds an audience from content that just fills a calendar.

#### Step 5: Write（写作阶段）

现在开始写作——这是作者**从未自动化**的部分。

> [!QUOTE] Here's the thing most people get wrong about AI content tools. They try to automate the writing... But what you end up with is content that sounds like everyone else using the same prompts.

人们关注你不是因为信息（信息到处免费），而是因为**你的视角、你的表达方式、你让别人感到被看见的独特框架**。这些无法靠prompt实现，必须你自己坐下、挑选idea，用自己的声音写出来。

系统已经帮你解决了“写什么”的难题，你只需执行写作。对作者来说，这部分每天只需20分钟（有时更少）：打开Notion，挑3-5个idea，写完、排期、完成。

认知负荷几乎为零，因为最难的部分已经提前完成。

### 四、系统实际移除的瓶颈

大多数人觉得写内容花时间是因为“写作本身慢”，其实**瓶颈在写作之前**：
- 今天发什么？
- 这个idea好吗？
- 之前有人做过吗？
- 该用什么角度？
- 这和受众相关吗？
- 它会表现好吗？

系统在你打开空白页之前就把这个循环全部关闭了。

> [!QUOTE] That's the shift. Not writing faster. Thinking less.

### 五、复利效应：系统越跑越聪明

长期运行后，Notion会积累你所有已覆盖的内容、角度、受众反馈。

你可以把这些历史作为新上下文喂给Claude，于是它不再只是从抓取内容生成idea，而是基于**你自己真正有效的内容历史**生成idea。

- 第1个月：输出量大幅提升
- 第3个月：内容质量提升
- 第6个月：你拥有一个比大多数全职社媒经理更懂你受众的内容机器

### 六、心态转变：从任务变成基础设施

大多数人把内容创作当成待办清单上的任务，需要硬着头皮完成。

一旦建立系统，它就不再是任务，而是**基础设施**：
- 基础设施不会消耗你，它在后台运行，安静复利。
- 你不再是拼命追赶的内容创作者，而是“出现、写作、让机器处理其余一切”的运营商。

> [!QUOTE] That's the actual unlock here. Not saving time. Changing your relationship with the work entirely.

### 七、启动所需工具与投入

- Claude账号（免费版可行，Pro更好）
- n8n（自托管或云端均可）
- Apify账号（按运行付费，此体量很便宜）
- Notion（免费版足够）
- 一次性投入2-3小时搭建

之后：每天20分钟。

搭建是一次性成本，回报是持续的。

### 八、为什么不自动化写作本身（作者的最后说明）

作者明确说：不是AI写不好，而是**不应该**全自动。

Claude能写，而且写得不错。但你真正构建的是：
- 一部个人作品集
- 你思考方式的记录
- 人们专门关注**你**而不是同niche其他50人的理由

一旦完全自动化写作，你就开始和别人混在一起。内容可能不错，但不再是**你的**。

> [!QUOTE] Keep the writing. Automate everything else. That's the whole framework.

最后行动号召：如果你想让作者帮你端到端搭建整个系统，回复“CLAUDE”，他会联系你。

---

## 🔗 原文链接

[点击查看原文](https://x.com/IAmAaronWill/status/2038197149867917419?s=20)

---

**🏷️ 标签：** #Claude内容系统 #内容自动化 #n8n工作流 #AI内容创作 #内容策略 #Apify抓取 #Notion数据库 #个人品牌内容机
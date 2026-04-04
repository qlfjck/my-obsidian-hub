> [!abstract] 全局总览
> 本笔记系统梳理了 **NPM（Node Package Manager）** 的核心概念、文件机制、目录影响规则，并与 Python 的 pip 进行了深度对比。核心要点在于理解 NPM "天生项目隔离"的特性，以及终端当前目录对包存储位置的绝对影响。
---
## 模块一：NPM 生态全貌
> [!abstract] 模块概要
> 从宏观视角理解 NPM 是什么，它由哪些部分组成，以及它在 JavaScript 生态中扮演的角色。
### 1.1 什么是 NPM
**NPM** 是 JavaScript 世界中最大的软件包管理系统。它不仅仅是一个命令行工具，更是一个完整的生态系统。
用一句话概括：NPM 是一个工具加一个平台，让开发者可以发布、共享、下载和管理可复用的 JavaScript 代码包。它随 **Node.js** 一起安装，注册表上目前已拥有超过 200 万个开源包。
在 NPM 出现之前，开发者需要手动去各个网站下载代码，人工比对版本，极易出错。NPM 彻底解决了代码复用难、依赖管理乱、版本控制难这三大痛点。
### 1.2 三大组成部分
NPM 生态系统由三个核心部分构成，各自承担不同职责：
- **NPM 官方网站**：搜索和浏览包的展示平台
- **NPM 注册表**：存储所有包源代码的巨型数据库
- **NPM 命令行工具 (CLI)**：开发者在终端中直接使用的交互工具
### 1.3 标准工作流程
NPM 的日常运转遵循一个标准的"生产-消费"闭环：
```text
开发者 A 写了一个实用工具
      │
      ▼
npm publish ──→ 上传到 NPM Registry
      │
      ▼
开发者 B 需要这个工具 ──→ npm install 工具名
      │
      ▼
自动下载到 node_modules/ 并记录到 package.json
      │
      ▼
在代码中通过 require/import 直接使用
```
> [!tip] 核心结论
> NPM = 包管理工具 + 全球最大的 JS 代码共享平台，它让"不重复造轮子"成为了 JS 开发的日常。
---
## 模块二：NPM 的核心文件与机制
> [!abstract] 模块概要
> 深入 NPM 项目的内部结构，理解 `package.json`、`node_modules` 和版本锁定机制是如何协同工作的。
### 2.1 package.json：项目的"身份证"
每个 NPM 项目的根目录下都必须有这个文件。它不仅记录依赖，还描述了项目的所有元信息和可执行脚本。
它的核心功能包括：声明项目名称与版本、定义可运行的脚本命令、区分**生产依赖**与**开发依赖**。
> [!example] package.json 核心结构
> ```json
> {
>   "name": "my-project",
>   "version": "1.0.0",
>   "scripts": {
>     "start": "node index.js",
>     "build": "webpack --config webpack.config.js"
>   },
>   "dependencies": {
>     "express": "^4.18.2"
>   },
>   "devDependencies": {
>     "webpack": "^5.88.0"
>   }
> }
> ```
### 2.2 存储与锁定机制
执行 `npm install` 后，会产生两个关键事物：
- **`node_modules/` 文件夹**：所有下载的包的实际存放地。层级极深，通常很大，坚决不提交到 Git。
- **`package-lock.json` 文件**：自动生成的版本快照，锁定每个依赖的精确版本号，确保团队所有成员安装的依赖完全一致。
### 2.3 语义化版本号
NPM 采用严格的版本号规范，格式为 `主版本号.次版本号.修订号`（如 `4.18.2`）。在 `package.json` 中，通过前缀符号控制更新范围：
| 符号 | 含义 | 示例说明 |
| :--- | :--- | :--- |
| `^` | 允许 Minor 和 Patch 更新 | `^4.18.2` 可更新到 4.19.0，不会到 5.0.0 |
| `~` | 只允许 Patch 更新 | `~4.18.2` 可更新到 4.18.9，不会到 4.19.0 |
| 无符号 | 锁定精确版本 | `4.18.2` 永远只用这个版本 |
> [!tip] 记忆要点
> `^` 像个折角，代表"折一下"（主版本不变）；`~` 像个波浪，代表"微微震荡"（次版本不变）。
---
## 模块三：终端目录对 NPM 的影响
> [!abstract] 模块概要
> 解答"在不同目录执行安装命令会发生什么"的核心疑惑，掌握本地安装与全局安装的本质区别。
### 3.1 两大黄金规则
终端当前目录对 NPM 有决定性影响，其行为受制于两条基本规则：
1. **本地安装（默认）**：包安装在 `当前执行命令的目录/node_modules/`
2. **全局安装（-g 参数）**：包安装在系统全局目录，与当前目录完全无关
> [!question] 疑惑：为什么我在不同目录装同一个包，结果不一样？
> **本质原因**：NPM 默认采用"项目隔离"策略。你在 A 目录装 lodash，只会放在 A 目录下；切到 B 目录再装，会生成一份完全独立的拷贝。
> **正确理解**：不要把 NPM 想象成一个全局仓库，而要把它当成每个项目专属的"工具箱"。
### 3.2 依赖查找顺序
当你在代码中写 `require('lodash')` 时，Node.js 并不是直接去全局找，而是执行一次**向上递归查找**：
1. 当前文件所在目录的 `node_modules/`
2. 父级目录的 `node_modules/`
3. 父级的父级目录...
4. 一直向上查找到系统根目录
5. 最后才去全局 `node_modules/` 查找
### 3.3 找不到 package.json 会怎样？
如果在没有 `package.json` 的目录（如桌面）执行 `npm install`，NPM 会发出警告，但**仍会强行创建** `node_modules/` 文件夹。这会产生"孤儿依赖"，下次执行 `npm install` 时不会自动安装它。
### 3.4 常见误区与最佳实践
> [!warning] 误区 1：在桌面或随意目录装包
> 在非项目目录执行安装，会导致 `node_modules` 散落各处，极难管理。
> **正确做法**：先 `cd` 到项目目录，确保有 `package.json`，再执行安装。
> [!warning] 误区 2：滥用全局安装
> 像 React、Vue 这类属于项目依赖的包，绝不能全局安装。全局安装仅适用于命令行工具（如 `typescript`、`nodemon`）。
**实用定位命令**：
- `npm root`：查看当前项目的 `node_modules` 路径
- `npm root -g`：查看全局安装路径
- `npm list <包名>`：查看某个包的具体安装位置和版本
---
## 模块四：NPM vs pip：跨语言视角的对比
> [!abstract] 模块概要
> 通过与 Python 的 pip 机制进行多维度对比，更深刻地理解 NPM 的设计哲学与先进性。
### 4.1 概念映射表
两个生态的核心概念几乎可以一一对应：
| JavaScript / Node.js | Python |
| :--- | :--- |
| `npm` | `pip` |
| `npmjs.com` | `pypi.org` (PyPI) |
| `package.json` | `requirements.txt` / `pyproject.toml` |
| `package-lock.json` | `pip freeze` 输出 / `poetry.lock` |
| `node_modules/` | `site-packages/` |
| `npx` | `pipx` |
| `nvm` | `pyenv` |
### 4.2 核心差异对比
虽然概念相似，但在底层机制上存在巨大差异：
| 维度 | NPM (JavaScript) | pip (Python) |
| :--- | :--- | :--- |
| **项目隔离** | 天生隔离（每个项目独立 `node_modules`） | 默认全局共享，需手动创建 `venv` 隔离 |
| **依赖声明** | `package.json`（功能强大，含脚本与元信息） | `requirements.txt`（极其简陋，仅是列表） |
| **版本锁定** | 自动生成 `package-lock.json` | 需手动执行 `pip freeze` |
| **依赖分类** | 原生区分 `dependencies` 和 `devDependencies` | 原生不支持，需手动维护多个 txt 文件 |
| **脚本管理** | 内置 `npm run` 机制 | 无原生等价功能，依赖 Makefile 等 |
### 4.3 为什么 NPM 体验更好？
原生 pip 功能相对有限，很多在 NPM 中开箱即用的功能，在 Python 中需要"拼凑"实现。
> [!note] Python 的进阶对标方案
> 如果你想在 Python 中获得类似 NPM 的丝滑体验，不要用原生 pip，直接使用 **Poetry**。
> - `pyproject.toml` 对标 `package.json`
> - `poetry.lock` 对标 `package-lock.json`
> - `poetry run` 对标 `npm run`
> - 且 Poetry 能自动管理虚拟环境，免去了手动 `venv` 的繁琐。
> [!tip] 一句话总结跨语言对比
> **NPM 约等于 pip + venv + Makefile + 版本锁定的集合体。**
---
## 易错点与误区汇总
> [!warning] 误区：不同项目可以混用依赖
> **现象**：在 project-A 装完包，直接 `cd ../project-B` 写代码引用。
> **后果**：本地开发可能不报错（因为向上查找找到了 A 的包），但部署到服务器时会直接崩溃。
> **正确理解**：永远假设每个项目的 `node_modules` 是一座孤岛，跨项目引用是绝对禁止的。
# npm 全局安装路径与自定义前缀详解
> [!abstract] 全局总览
> 本笔记梳理了 npm 全局包的默认安装位置（涵盖 Linux、macOS、Windows）以及如何通过 `--prefix` 参数自定义安装路径。核心在于理解“系统默认位置”与“自定义位置”的区别，以及自定义安装后必须手动配置 PATH 环境变量的关键步骤。
---
## 一、默认全局路径：npm 把包装到了哪里？
### 1.1 如何查看默认路径
最直接的方法是在终端运行以下命令，它会返回当前全局包的实际安装根目录。
```bash
npm root -g
```
> [!tip] 其他常用查看命令
> - **`npm prefix -g`**：查看全局安装前缀（即 `node_modules` 的上级目录）。
> - **`npm config list`**：查看所有 npm 配置，包含路径信息。
> - **`npm list -g --depth=0`**：列出当前已安装的全局包。
### 1.2 各操作系统的默认位置
不同系统下，npm 默认将包源码和可执行命令分离存放。
| 操作系统 | 包源代码路径 | 可执行命令路径 |
| :--- | :--- | :--- |
| **Linux** | `/usr/local/lib/node_modules/` | `/usr/local/bin/` |
| **macOS** | `/usr/local/lib/node_modules/` | `/usr/local/bin/` |
| **macOS (nvm)** | `~/.nvm/versions/node/<版本号>/lib/node_modules/` | `~/.nvm/versions/node/<版本号>/bin/` |
| **Windows** | `C:\Users\<用户名>\AppData\Roaming\npm\node_modules\` | `C:\Users\<用户名>\AppData\Roaming\npm\` |
> [!note] nvm 的特殊性
> 如果在 macOS 上使用了 **nvm** (Node Version Manager) 管理 Node.js 版本，全局包路径会变为用户目录下的 `.nvm` 文件夹，避免写入系统目录需要 `sudo` 权限的问题。
### 1.3 安装实例解析
以在 Linux 上全局安装 `typescript` 为例，npm 会自动处理源码与命令的映射关系。
```text
npm install -g typescript
```
执行后的目录变化：
- **源代码**：存放在 `/usr/local/lib/node_modules/typescript/`
- **命令文件**：链接到 `/usr/local/bin/tsc`
> [!example] 为什么可以直接运行 `tsc`？
> 因为 `/usr/local/bin` 通常已经在系统的环境变量 `PATH` 中。系统能够找到 `tsc` 这个可执行文件，所以你可以在终端任意位置直接输入 `tsc` 来运行。
---
## 二、自定义安装路径：`--prefix` 参数详解
> [!abstract] 模块概要
> 解析 `npm install -g --prefix` 命令的含义、生成的目录结构以及使用时的常见陷阱。
### 2.1 命令含义拆解
当我们想要将包安装到非系统默认目录时，会使用 `--prefix` 参数。
```bash
npm install -g <package-name> --prefix /path/to/directory
```
各参数作用如下：
- **`npm install`**：安装指令。
- **`-g`**：全局安装模式。
- **`--prefix /path/to/directory`**：核心参数，指定安装的根目录，替代默认的系统路径。
> [!tip] 一句话总结
> 这条命令的意思是：“把这个工具安装到我指定的文件夹里，别往系统文件夹里塞。”
### 2.2 自定义目录的结构变化
执行该命令后，npm 会在你指定的 `/path/to/directory` 下自动构建标准的文件结构。
```text
/path/to/directory/
├── bin/          # 存放可执行文件 (如 pm2, tsc)
├── lib/
│   └── node_modules/  # 存放包的实际源代码
└── share/        # 存放文档等其他资源
```
### 2.3 应用场景
通常在以下三种情况下会使用自定义路径：
1.  **权限受限**：没有 root 或管理员权限，无法写入系统默认目录。
2.  **环境隔离**：不想污染系统全局环境，将特定工具集成到特定项目中。
3.  **便携式开发**：将开发工具统一放在 U 盘或独立分区中，便于携带。
### 2.4 关键陷阱与解决方案
> [!warning] 命令找不到
> 使用 `--prefix` 安装后，指定的目录不在系统默认的 `PATH` 环境变量中。直接输入命令（如 `pm2`）会报错 `command not found`。
**解决方法：手动配置环境变量**
你需要将自定义目录下的 `bin` 文件夹添加到 `PATH` 中。
| 系统 | 操作方法 |
| :--- | :--- |
| **Linux / macOS** | 在配置文件（如 `.bashrc` 或 `.zshrc`）中添加：<br>`export PATH=$PATH:/path/to/directory/bin` |
| **Windows** | 在“系统属性 -> 环境变量”中，将 `C:\path\to\directory` 添加到 Path 变量。 |
> [!question] 疑惑：为什么不自动配置 PATH？
> npm 的职责是安装包，而修改系统环境变量（PATH）涉及操作系统层面的权限和配置，通常需要用户手动确认或操作，以防止软件恶意篡改系统设置。
---
## 三、常用命令速查表
> [!abstract] 模块概要
> 汇总查看 npm 路径和配置的高频命令，便于快速复习。
| 命令 | 作用 | 备注 |
| :--- | :--- | :--- |
| `npm root -g` | 查看全局包安装根目录 | 最常用 |
| `npm prefix -g` | 查看全局安装前缀 | 即 `node_modules` 的上级目录 |
| `npm config list` | 查看所有 npm 配置 | 包含路径信息 |
| `npm list -g --depth=0` | 查看已安装的全局包 | `depth=0` 防止显示依赖树 |
---
## 四、易错点与误区汇总
> [!warning] 误区：以为 `-g` 就是安装到当前目录
> **误区**：认为 `npm install -g` 会把包装在当前终端所在的目录。
> **正解**：`-g` 默认是指向系统预设的目录（如 `/usr/local/lib`），只有配合 `--prefix` 才能指定自定义路径。
> [!warning] 误区：忽略 nvm 的影响
> **误区**：在 macOS 上查找全局包路径时，忽略了 nvm 的存在，去 `/usr/local/lib` 下找。
> **正解**：如果使用了 nvm，路径通常在 `~/.nvm/versions/node/<版本号>/` 下，务必先运行 `npm root -g` 确认。

## 理解 Git 的机制

Git 把你的项目文件夹变成一个**有历史记录的仓库**。每次你执行 `commit`，Git 就给当前文件状态拍一张快照并记录下来，你可以随时回到任意一张快照。

文件在 Git 里有三个位置：

```
工作区（你直接编辑文件的地方）
  → git add →
    暂存区（打包好、准备提交的内容）
      → git commit →
        本地仓库（永久保存的快照历史）
          → git push →
            GitHub（云端备份 / 协作）
```

简单说：**改完文件 → add 打包 → commit 存档 → push 同步云端**。

---

## 一、把自己的项目上传同步到 GitHub

**第一次上传（只做一次）**

```bash
git init                          # 在项目文件夹初始化 Git
git add .                         # 暂存所有文件
git commit -m "第一次提交"         # 创建第一个快照
git remote add origin <仓库URL>   # 关联 GitHub 仓库
git push -u origin main           # 上传，并记住这个远程地址
```

> GitHub 上先手动新建一个空仓库，把仓库 URL 复制过来用。

**之后每次同步**

```bash
git add .
git commit -m "说明改了什么"
git push
```

---

## 二、Clone 别人的仓库

```bash
git clone <仓库URL>
```

就这一条命令。会在当前目录新建一个文件夹，把代码全部下载下来。

如果对方仓库有更新，想同步到本地：

```bash
git pull
```

---

## 三、版本控制：保存快照与复原

**保存当前版本**

```bash
git add .
git commit -m "完成了登录功能"
```

每次 commit 就是存一个快照，之后随时可以回来。

**查看历史版本**

```bash
git log --oneline
```

输出类似：

```
a3f2c1d 完成了登录功能
9c1b884 修复了搜索 bug
5e8d021 初始提交
```

**复原到某个版本**

```bash
# 只是看看某个版本的样子（不改变历史）
git checkout <commit-hash>

# 回到最新状态
git checkout main
```

**撤销单个文件的改动（还没 commit）**

```bash
git restore <文件名>    # 丢弃这个文件的修改，还原到上次 commit 的状态
```

**彻底回退到某个版本（慎用，会丢失之后的记录）**

```bash
git reset --hard <commit-hash>
```

---

## 命令速查

| 命令 | 作用 |
|------|------|
| `git init` | 初始化仓库 |
| `git clone <url>` | 克隆远程仓库 |
| `git add .` | 暂存所有改动 |
| `git add <file>` | 暂存指定文件 |
| `git commit -m "..."` | 提交快照并附说明 |
| `git push` | 推送到 GitHub |
| `git pull` | 拉取远程最新内容 |
| `git status` | 查看当前文件状态 |
| `git log --oneline` | 查看精简提交历史 |
| `git checkout <hash>` | 切换到某次提交查看 |
| `git checkout main` | 回到最新状态 |
| `git restore <file>` | 撤销文件的未提交改动 |
| `git reset --hard <hash>` | 强制回退到某次提交 ⚠️ |

---
tags:
  - docker
  - 容器化
  - 后端
  - 运维
  - 学习笔记
date: 2026-03-22
---

## Docker 的核心思想

Docker 只做一件事：**把程序和它运行所需的一切打包在一起，让它在任何地方都能跑起来。**

传统部署的痛点是"在我电脑上能跑，到你那边就挂了"——原因是 Python 版本不同、系统环境不同、缺少某个库。Docker 的解法是把整个运行环境也一起打包进去。

### 三个核心概念

| 概念 | 类比 | 说明 |
|---|---|---|
| **Dockerfile** | 菜谱 | 描述"怎么做这个镜像"的指令文件 |
| **Image（镜像）** | 安装包 | 只读的打包结果 |
| **Container（容器）** | 跑起来的程序 | Image 运行后的实例 |

### Docker vs 虚拟机

- 虚拟机模拟整套硬件 + 完整 OS，体积 GB 级，启动慢
- Docker 共用宿主机 OS 内核，只隔离进程，体积 MB 级，启动秒级
- 本质区别：不是"轻量级虚拟机"，是**进程隔离**

---

## Dockerfile 逐行解析

```dockerfile
FROM python:3.11-slim       # 1. 拿一个装好 Python 的基础环境
WORKDIR /app                # 2. 在容器内划定工作目录
COPY requirements.txt .     # 3. 先复制依赖清单
RUN pip install --no-cache-dir -r requirements.txt  # 4. 按清单装依赖
COPY . .                    # 5. 再复制所有代码
EXPOSE 8000                 # 6. 声明对外开放的端口
CMD ["python", "app.py"]    # 7. 容器启动时执行的命令
```

### 关键细节：为什么 COPY 分两步？

Docker 每一行指令都会缓存。将 `requirements.txt` 和代码分开复制的原因：

- 只要 `requirements.txt` 没变，`pip install` 那步直接走缓存，跳过
- 如果合并成一步 `COPY . .`，改任意一行代码都会触发重新安装所有依赖
- **结论：先 COPY 依赖文件 → RUN 安装 → 再 COPY 代码，是标准写法**

### FROM 和 Base Image

`FROM python:3.11-slim` 的含义：从 Docker Hub 下载一个预先打包好的镜像，内容是：

```
Linux 系统
└── Python 3.11        ← 这两层合起来就是 python:3.11-slim
    └── 你的依赖        ← 你的 Dockerfile 从这里开始叠加
        └── 你的代码
```

`slim` vs 完整版：去掉了用不到的系统工具，体积更小，Python 本身无差异，适合生产环境。

### 不同框架的 CMD 写法

| 框架 | CMD |
|---|---|
| 普通脚本 | `CMD ["python", "app.py"]` |
| Flask | `CMD ["flask", "run", "--host=0.0.0.0"]` |
| FastAPI | `CMD ["uvicorn", "app:app", "--host", "0.0.0.0", "--port", "8000"]` |

> `--host 0.0.0.0` 必须加，否则容器外无法访问。

---

## 完整封装与分享流程

整体路径：`代码 → Dockerfile → Image → Docker Hub → 别人 pull 运行`

### 第一步：整理项目

```
my-app/
├── app.py
├── requirements.txt   # pip freeze > requirements.txt
└── Dockerfile
```

### 第二步：Build 镜像

```bash
docker build -t my-app:v1 .
docker images                        # 验证是否成功
docker run -p 8000:8000 my-app:v1   # 本地测试
```

### 第三步：推送到 Docker Hub

```bash
docker login
docker tag my-app:v1 yourname/my-app:v1   # 改成 用户名/镜像名 格式
docker push yourname/my-app:v1
```

### 别人如何使用

```bash
docker run -p 8000:8000 yourname/my-app:v1
```

只需有 Docker，无需安装 Python 或任何依赖。

---

## 常见问题

| 问题 | 原因 | 解法 |
|---|---|---|
| build 时某包装不上 | slim 缺编译工具 | 换用 `python:3.11` 完整版 |
| 容器跑起来但访问不到 | 缺 `--host 0.0.0.0` | CMD 加上该参数，检查 `-p` 端口映射 |
| 代码更新后怎么办 | Image 是静态快照 | 重新 build，用新标签（如 `:v2`）推送 |

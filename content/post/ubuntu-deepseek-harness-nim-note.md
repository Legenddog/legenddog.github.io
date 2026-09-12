---
title: "DJI Mini 3 Pro 飞行技巧与拍摄全攻略"
description: "DJI Mini 3 Pro 飞行技巧与拍摄全攻略"
date: 2026-09-06
lastmod: 
weight: 3
categories:
    - 无人机
tags:
    - 无人机

---



# 在 Ubuntu 上安装 DeepSeek Harness 并接入 NVIDIA NIM 免费模型

> 记录一次实操经验：在 Ubuntu 上把 DeepSeek Harness 跑起来，再用 NVIDIA NIM 的免费 DeepSeek 模型当后端，白嫖一把推理算力。文中涉及的版本、命令均以官方仓库与官方文档为准。

---

## 一、背景

DeepSeek Harness（简称 DSH / `dsh`）是 DeepSeek AI 开源的智能体框架，把大模型、工具、文件系统、终端、会话记录、审批交互、插件和 Web UI 组合成一套可运行的 Agent 应用。它最吸引人的一点是「一切皆插件」，开发者能精确控制 Agent 的每一个环节。

但 DeepSeek 官方 API 常有限流或排队的问题。于是我想找一个**免费、稳定、兼容 OpenAI 协议**的推理后端。NVIDIA NIM 提供了一批免费模型（包括 DeepSeek-R1 系列），注册 NVIDIA 开发者计划即可免费调用云端 API，无需自备 GPU，非常适合个人博客/学习场景。

本文记录我在 **Ubuntu 22.04** 上从零到跑通的全过程。

---

## 二、环境要求

在开始前，先确认你的机器满足以下条件（以 Ubuntu 为准）：

| 项目 | 要求 |
| --- | --- |
| 操作系统 | Ubuntu 20.04+（官方推荐 22.04 LTS 或更新） |
| Node.js | `^22.19.0` 或 `>=24.0.0`（23.x 不支持） |
| pnpm | `11.x`（官方仓库锁定 `pnpm@11.7.0`） |
| 内存 | 建议 8GB 以上 |

> ⚠️ 注意：若只是调用 NIM 的**云端免费 API**，**不需要**本地 GPU；只有打算在本地自托管 NIM 容器时才需要 NVIDIA 驱动 + CUDA + Docker。

---

## 三、环境准备

### 3.1 安装 Node.js

推荐使用 NodeSource 安装 LTS 版本，一条命令搞定：

```bash
curl -fsSL https://deb.nodesource.com/setup_lts.x | sudo -E bash -
sudo apt-get install -y nodejs
```

如果希望锁定 22.x 线（最贴合官方要求），可以把版本号改成 `setup_22.x`。

验证安装：

```bash
node -v   # 期望输出 v22.19.0 或更高
npm -v
```

### 3.2 安装 pnpm

```bash
npm install -g pnpm
pnpm -v   # 期望 11.x
```

官方仓库锁定的是 `pnpm@11.7.0`，如需精确对齐：

```bash
corepack enable
# 或
npm install -g pnpm@11.7.0
```

### 3.3 安装 git（可选，源码安装时需要）

```bash
sudo apt-get install -y git
```

---

## 四、安装 DeepSeek Harness

DSH 提供多种安装方式，按使用场景选择即可。

### 方式一：npx 一行启动（最快尝鲜）

不想折腾、只想看界面，一条命令：

```bash
npx @deepseek-ai/dsh web
```

执行后会在本地启动 Web UI，默认地址 `http://127.0.0.1:3080`。

> 想加 `-y` 跳过确认：`npx -y @deepseek-ai/dsh web`

### 方式二：全局安装（推荐日常使用）

```bash
npm install -g @deepseek-ai/dsh
dsh --version     # 看到版本号即安装成功
dsh web           # 启动 Web UI
```

全局安装后你就有 `dsh` 命令，任意目录都能用，还能解锁 profile 管理、插件管理等能力。

### 方式三：源码编译（开发者/插件作者）

```bash
git clone https://github.com/deepseek-ai/deepseek-harness.git
cd deepseek-harness
pnpm install       # 装依赖
pnpm run build     # 编译 workspace 包，生成 lib/
pnpm dsh web       # 启动 Web UI
```

> 小提醒：本地缺少 `pnpm` 时先 `npm install -g pnpm`；构建末尾出现「chunk 大于 500 kB」的警告可以忽略，不影响运行。

---

## 五、获取 NVIDIA NIM 免费 API Key

NIM（NVIDIA Inference Microservice）把 DeepSeek 等模型预封装成标准化微服务，并提供 OpenAI 兼容的 API。注册开发者计划即可**免费调用云端推理**。

### 5.1 注册 NVIDIA 开发者计划

1. 打开 [https://build.nvidia.com](https://build.nvidia.com)。
2. 点击右上角 **Login**，用邮箱注册（建议企业邮箱，额度通常更多）。
3. 验证邮箱，进入开发者计划。

### 5.2 获取 API Key

1. 进入 DeepSeek-R1 对应页面，例如：
   - `https://build.nvidia.com/deepseek-ai/deepseek-r1`
   - 或 `https://build.nvidia.com/deepseek-ai/deepseek-r1-distill-llama-8b`
2. 页面右侧找到 **Get API Key**，生成专属密钥。
3. 复制并保存好，**不要硬编码到代码里**，建议用环境变量或密钥管理服务存放。

### 5.3 免费额度说明（实测/官方口径）

- 注册即可获得免费推理额度（个人邮箱常见为 1000 次，企业邮箱约 5000 次）。
- 云端免费 API 有速率限制（常见约 `40 RPM`，即每分钟请求数），无每日 token 上限，**无需信用卡**。
- 云端端点统一为 `https://integrate.api.nvidia.com/v1`。

> 说明：NVIDIA 开发者计划会员可免费下载 NIM 容器用于开发测试（最多支持 16 张 GPU）；用于正式商业产品需另行授权。

---

## 六、配置 DeepSeek Harness 使用 NIM 免费模型

### 6.1 启动服务

```bash
dsh web
# 或 npx @deepseek-ai/dsh web
```

终端会打印端口地址，默认 `http://127.0.0.1:3080`（端口被占用会自动换）。用 Chrome 打开。

### 6.2 添加自定义提供方（NVIDIA NIM）

进入 Web UI 后：

1. 打开左侧**侧边栏**，点击**设置**（Settings）。
2. 在**模型**里选择「添加自定义提供方」（Add Custom Provider）。

填写以下配置：

| 配置项 | 值 |
| --- | --- |
| Provider ID | `nvidia`（可自定义） |
| 显示名称 | `NVIDIA NIM`（可自定义，建议一目了然） |
| API 地址 | `https://integrate.api.nvidia.com/v1` |
| API 协议 | `openai-completions` |
| API 密钥 | 你的 NVIDIA NIM API Key |
| 模型 | `deepseek-ai/deepseek-r1`（或 distill 变体） |

填好后点击右下角**创建提供方**。

### 6.3 选择模型并开始会话

回到主界面，选择工作区，选择由 NVIDIA NIM 提供的模型，然后就可以开始对话了。

推荐模型 ID：

- `deepseek-ai/deepseek-r1` —— 完整版，强推理/代码能力强，但对上下文占用大。
- `deepseek-ai/deepseek-r1-distill-llama-8b` —— 蒸馏版，更轻量，响应更快。
- `deepseek-ai/deepseek-r1-distill-qwen-14b` —— 蒸馏版，中文表现较好。

初次用建议从蒸馏版（8b 或 14b）开始，速度更友好。

---

## 七、命令行/脚本调用方式（可选）

除了 Web UI，你也可以直接通过 OpenAI 兼容接口调用 NIM 免费模型：

```bash
curl https://integrate.api.nvidia.com/v1/chat/completions \
  -H "Authorization: Bearer $NVIDIA_API_KEY" \
  -d '{"model":"deepseek-ai/deepseek-r1","messages":[{"role":"user","content":"你好"}]}'
```

Python 方式：

```python
from openai import OpenAI

client = OpenAI(
    base_url="https://integrate.api.nvidia.com/v1",
    api_key="你的NVIDIA_API_KEY",
)

resp = client.chat.completions.create(
    model="deepseek-ai/deepseek-r1",
    messages=[{"role": "user", "content": "你好"}],
    temperature=0.5,
    max_tokens=1024,
)
print(resp.choices[0].message.content)
```

---

## 八、DSH 凭据读取优先级

如果遇到「模型不生效 / 报未授权」，先确认凭据写在正确的位置。DSH 按以下优先级读取：

1. 进程环境变量
2. `~/.dsh/.credentials.yaml`（托管存储）
3. 工作目录下的 `.env`
4. `~/.dsh/.env`（兜底）

在 Web UI 里配置提供方后，密钥会落入托管存储，通常无需再手动写 `.env`。

---

## 九、常用启动/端口命令

```bash
# 修改端口
npx @deepseek-ai/dsh web --port 8080

# 全局安装后启动
dsh web

# 查看版本
dsh --version
```

> 说明：DSH 服务默认绑定 `127.0.0.1`，不会自行对外发布到网络。如需局域网访问，请自行配置反向代理/端口转发并做好访问控制。

---

## 十、踩坑记录

1. **Node.js 版本过低**：`18.x`/`20.x` 或误装 `23.x` 时，安装可能不报错，但一跑就各种诡异问题。直接用 `22.x LTS` 或 `24.x`。
2. **缺 pnpm**：先 `npm install -g pnpm`。
3. **构建 chunk 警告**：源码安装时「chunk 大于 500 kB」属正常，忽略即可。
4. **API Key 泄露风险**：不要把 Key 写进公开仓库或客户端代码。用环境变量或密钥管理。
5. **本地自托管需要 GPU**：若你选择自托管 NIM 容器，需要 NVIDIA 驱动（≥535）+ CUDA 12.2+ + Docker 24+ + NVIDIA Container Toolkit；单纯用云端免费 API 则不需要。
6. **npx 首次下载慢**：`npx @deepseek-ai/dsh web` 第一次会拉一整包依赖（实测约 300MB+），且没有进度条，耐心等待；国内网络可考虑 `npm config set registry https://registry.npmmirror.com`。

---

## 十一、小结

至此，你已经：

- ✅ 在 Ubuntu 上装好了 Node.js、pnpm
- ✅ 用 `dsh` 或 `npx` 跑起了 DeepSeek Harness
- ✅ 注册 NVIDIA 开发者计划拿到免费 API Key
- ✅ 在 DSH 里配置好 NVIDIA NIM（OpenAI 兼容端点）并开始对话

整个过程不需要本地 GPU，成本为零，非常适合作为个人博客记录、学习 Agent 开发的起步配置。后续可进一步探索 DSH 的插件系统、profile 管理、模式切换（极简/标准/创造）等进阶能力。

祝你玩得开心！

内容由 AI 生成仅供参考
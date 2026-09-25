---
title: Hugo｜从零开始Hugo建站 2
description: hugo主题博客的网页配置和html文件生成
date: 2021-03-01
categories:
    - hugo
tags:
    - hugo
image: cover.jpg
---


# Hugo｜从零开始Hugo建站 2


>[!NOTE] 写在前面
>
>本篇是第二部分，主要讲如何用 Hugo ，从 0 到 1 完成一个静态博客：通过简单配置自定义博客界面，并把站点免费部署到 GitHub Pages。
>
>下一篇会讲 Hugo 的完整配置以及如何使用 GitHub Action 实现自动推送。

---

## 环境准备

### 安装 Git

1. 前往 Git 官网下载安装：[git-scm.com](https://git-scm.com/downloads)
2. 全部默认选项安装即可

### 安装 Hugo（Extended 版本）

1. 前往 Hugo GitHub Releases 页面：[Hugo Releases](https://github.com/gohugoio/hugo/releases)
2. 选择 **extended** 版本（带 extended 字样），Stack 主题需要 extended 版本的 SCSS 编译支持

不管在哪种系统中安装 Hugo，最后都可以使用以下命令查看是否安装成功：
```bash
>>> hugo version
Hugo Static Site Generator v0.68.3-157669A0 linux/amd64 BuildDate: 2020-03-24T12:05:34Z
```


> 本文编写时使用的操作系统Ubuntu  版本为wsl版24.04，Hugo 版本为 v0.165.0，Stack 主题版本为 v4。

## 创建 Hugo 站点

### 1. 初始化项目

使用 Hugo 创建博客项目：

```bash
# 创建新站点
hugo new site myblog  # 博客项目的名字为 myblog

# 进入站点目录
cd myblog
```

生成的目录结构：

```
myblog/
├── archetypes/    # 文章模板
├── assets/        # 静态资源（图片、CSS、JS）
├── content/       # 博客内容（文章）
├── data/          # 数据文件
├── i18n/          # 多语言
├── layouts/       # 布局模板
├── public/        # 构建输出（hugo 命令生成）
├── static/        # 静态文件（直接复制到 public）
├── themes/        # 主题
└── hugo.toml      # 站点配置
```

### 2. 安装主题

```bash
cd themes
git clone git@github.com:CaiJimmy/hugo-theme-stack.git
```
> 注意：hugo-theme-stack 对 Hugo 版本有要求，请确保版本兼容。


安装完成后，主题目录结构如下：

```
myblog/themes/
└── hugo-theme-stack-4.0.x/   # 带版本号的文件夹
```


### 3. 配置主题（关键步骤）

⚠️ **版本差异**：这一步最容易出错。新版 Stack（v4.x）和旧版（v3.x）的文件结构完全不同：

| 对比项 | 旧版 Stack（v3.x） | 新版 Stack（v4.x） |
| --- | --- | --- |
| 样例目录名 | `exampleSite/` | `demo/` |
| 配置文件 | 单个 `hugo.yaml` | 多个 `.toml` 文件在 `config/_default/` |
| 主题引用 | `theme: hugo-theme-stack` | `[[module.imports]]`（Hugo Modules 方式） |

#### 新版（v4.x）操作步骤

**Step 1：复制配置文件**

将主题目录下的 `config/_default/` 文件夹复制到博客根目录（myblog），其中包含 6 个配置文件：

| 文件 | 作用 |
| --- | --- |
| `hugo.toml` | 基础配置（baseURL、title、分页、永久链接等） |
| `languages.toml` | 多语言配置 |
| `markup.toml` | Markdown 渲染配置（代码高亮、目录等） |
| `menu.toml` | 菜单配置（导航栏、社交链接） |
| `params.toml` | 主题参数（侧边栏、评论、widgets 等） |
| `related.toml` | 相关内容推荐配置 |

同时，将主题目录下的 `demo/` 文件夹中的内容复制到博客根目录（myblog）。

**Step 2：修改主题引用方式**

打开 `config/_default/hugo.toml`，找到以下内容：

```toml
[[module.imports]]
    path = "github.com/CaiJimmy/hugo-theme-stack/v3"
```

将其替换为传统引用方式（无需 Go 环境和 Hugo Modules，更简单）：

```toml
theme = "hugo-theme-stack-4.0.2"
```

修改后，删除博客根目录下的原 `hugo.toml` 文件（避免配置冲突）。

> 旧版（v3.x）操作步骤可参考文末的资料链接。本篇内容编写时参考了相关资料，由于本博客之前由 v3 版本搭建，此次改为 v4 也花费了不少功夫。



### 4.启动 Hugo 博客服务

使用以下命令启动本地服务：

```bash
>>> hugo server
                   | EN  
-------------------+-----
  Pages            | 29  
  Paginator pages  |  0  
  Non-page files   |  0  
  Static files     |  1  
  Processed images |  0  
  Aliases          | 12  
  Sitemaps         |  1  
  Cleaned          |  0  

Built in 60 ms
Watching for changes in /home/wp/t/myblog/{archetypes,content,data,layouts,static,themes}
Watching for config changes in /home/wp/t/myblog/config.toml
Environment: "development"
Serving pages from memory
Running in Fast Render Mode. For full rebuilds on change: hugo server --disableFastRender
Web Server is available at http://localhost:1313/ (bind address 127.0.0.1)
Press Ctrl+C to stop

```

相关命令：

```bash
# 本地预览（包含草稿文章）
hugo server -D

# 生成 public 目录（静态文件）
hugo
```

服务默认占用 1313 端口，在浏览器中访问 `http://localhost:1313/` 即可预览。

### 5. 推送本地博客到远端（GitHub Pages）

将生成的 `public/` 目录作为独立仓库推送到 GitHub：

```bash
cd public
git init
git add .
git commit -m "first commit"
git branch -M main
git remote add origin https://github.com/你的用户名/你的仓库名.git
git push -u origin main
```

> 后续可通过 GitHub Action 实现推送博客源文件后自动构建并部署，将在下一篇中介绍。

---

## 附：常用 Linux 命令

```bash
# 查看 Hugo 安装路径
which hugo

# 查看当前目录路径
pwd

# 查看命令历史
history
```

---

wsl GUI GNOME


## 资料参考

1. [Hugo + Stack + GitHub Pages 博客搭建指南](https://akuamt.github.io/p/hugo--stack--github-pages-%E5%8D%9A%E5%AE%A2%E6%90%AD%E5%BB%BA%E6%8C%87%E5%8D%97/)




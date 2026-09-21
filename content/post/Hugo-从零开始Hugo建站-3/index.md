---
title: Hugo｜从零开始Hugo建站 3
description: hugo博客在github上的自动化流程
date: 2023-06-15
categories:
    - hugo
image: cover.jpg
---


# Hugo｜从零开始 Hugo 建站 3

## Hugo 站点文件配置

[Hugo 站点文件夹内容](https://gohugo.io/getting-started/directory-structure/)

## Hugo Stack 主题配置

[Hugo Stack 主题快速上手](https://stack.cai.im/zh/guide/getting-started)

我使用的主题是基于 Stack 改版的模板，由 [liu-houliang/hugo-stack-starter](https://liu-houliang.github.io/hugo-stack-starter/) 调整而来，文末会附上链接。

### 基础配置

所有配置文件位于 `config/_default/` 目录下：

```
config/_default/
├── config.toml          # 站点标题、域名 ← 必须修改
├── languages.toml       # 多语言设置
├── params.toml          # 主题参数（评论、首页布局）
├── params.zh.toml       # 中文专属参数（头像、副标题）← 建议修改
├── params.en.toml       # 英文专属参数
├── menu.zh.toml         # 中文导航菜单
└── menu.en.toml         # 英文导航菜单
```

## 在 GitHub Pages 上托管

将 Hugo 站点部署为 GitHub Pages 项目（或个人/组织站点），并通过 GitHub Actions 实现自动化构建与部署。

每次手动构建再推送很繁琐，可以利用 GitHub Actions 实现：推送代码 → 自动构建部署。

[Hugo 官方 GitHub Actions 模板](https://hugo.opendocs.io/hosting-and-deployment/hosting-on-github/)

在 Hugo 项目根目录创建 `.github/workflows/hugo.yaml`，使用官方推荐的 GitHub Pages Actions 方式，推送到 `main` 分支即可自动构建并部署。以下是完整的配置流程和可直接使用的 workflow 文件。

### 一、准备工作

1. **确保 Hugo 项目已推送到 GitHub 仓库**（注意：不要把 `public/` 目录提交进仓库，Hugo 构建时会自动生成，建议在 `.gitignore` 中添加 `public/` 和 `resources/`）。
2. **修改 `hugo.toml` 中的 `baseURL`**：
   - 用户主页站点（仓库名为 `用户名.github.io`）：`baseURL = "https://<用户名>.github.io/"`
   - 普通项目站点：`baseURL = "https://<用户名>.github.io/<仓库名>/"`
3. **如果主题以 Git Submodule 方式引入**，确认 Submodule 已正确提交（`git submodule add <主题地址> themes/<主题名>`）。

### 二、创建 Workflow 文件

在项目根目录创建 `.github/workflows/hugo.yaml`，以下是 Hugo 官方文档提供的模板：

```yaml
# 用于构建和部署Hugo网站到GitHub Pages的示例工作流程
name: 发布Hugo网站到Pages

on:
  # 在目标为默认分支的推送上运行
  push:
    branches:
      - main

  # 允许手动从Actions标签运行此工作流程
  workflow_dispatch:

# 设置GITHUB_TOKEN的权限，以允许部署到GitHub Pages
permissions:
  contents: read
  pages: write
  id-token: write

# 仅允许一个并发部署，跳过在进行中的运行与最新排队的运行之间排队的运行。
# 但不取消进行中的运行，以确保这些生产部署能够完成。
concurrency:
  group: "pages"
  cancel-in-progress: false

# 默认使用bash
defaults:
  run:
    shell: bash

jobs:
  # 构建作业
  build:
    runs-on: ubuntu-latest
    env:
      HUGO_VERSION: 0.120.2
    steps:
      - name: 安装Hugo CLI
        run: |
          wget -O ${{ runner.temp }}/hugo.deb https://github.com/gohugoio/hugo/releases/download/v${HUGO_VERSION}/hugo_extended_${HUGO_VERSION}_linux-amd64.deb \
          && sudo dpkg -i ${{ runner.temp }}/hugo.deb          
      - name: 安装Dart Sass
        run: sudo snap install dart-sass
      - name: 检出
        uses: actions/checkout@v4
        with:
          submodules: recursive
          fetch-depth: 0
      - name: 设置Pages
        id: pages
        uses: actions/configure-pages@v3
      - name: 安装Node.js依赖
        run: "[[ -f package-lock.json || -f npm-shrinkwrap.json ]] && npm ci || true"
      - name: 使用Hugo构建
        env:
          HUGO_ENVIRONMENT: production
          HUGO_ENV: production
        run: |
          hugo \
            --gc \
            --minify \
            --baseURL "${{ steps.pages.outputs.base_url }}/"          
      - name: 上传构建产物
        uses: actions/upload-pages-artifact@v2
        with:
          path: ./public

  # 部署作业
  deploy:
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}
    runs-on: ubuntu-latest
    needs: build
    steps:
      - name: 部署到GitHub Pages
        id: deployment
        uses: actions/deploy-pages@v2
```

### 几个关键点说明

- `submodules: recursive` + `fetch-depth: 0`：拉取主题子模块和完整 Git 历史（确保 `.GitInfo`、`.Lastmod` 正常工作）。
- `HUGO_ENVIRONMENT: production`：让主题的 GA 统计等生产环境配置生效。
- `--baseURL "${{ steps.pages.outputs.base_url }}/"`：自动获取 Pages 地址，无需手动写死 `baseURL`。
- `HUGO_VERSION`：建议改为本地 `hugo version` 输出的版本号，避免本地与 CI 构建结果不一致。

## 参考资料

- [liu-houliang 的 hugo-stack-starter在线预览 (Live Demo)](https://liu-houliang.github.io/hugo-stack-starter/)

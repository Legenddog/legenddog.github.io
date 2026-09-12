---
title: 从零开始Hugo建站（一）
description: 零基础学习git
date: 2023-06-15
categories:
    - hugo
    - git
    
---


# Hugo｜从零开始Hugo建站 1

## 写在前面

从一开始看到各路技术大牛的个人博客，开始萌发起了建站的想法。我不擅长写技术性博客文章（~~本文怕不是我第一篇也是最后一篇技术博客~~），只是粗略记录一下我建站的过程。如果你在对hugo博客建立的任何地方有问题，希望这篇可以帮助到你👌。

这个系列记录我从零搭建 Hugo 静态博客的过程。本篇是第一部分，主要讲建站前的准备工作：Git 的安装、本地基本操作、本地与远程仓库的连接，以及一些进阶技巧。如果你已经熟悉 Git，可以直接跳到下一篇。

下一篇会讲如何用 Hugo ，从 0 到 1 完成一个静态博客：通过简单配置自定义博客界面，并把站点免费部署到 GitHub Pages。


## 一、Git 入门：本地环境配置

镜像加速（推荐，最省事）​
如果你git下载慢，不想折腾代理，直接在原地址前加一个代理前缀即可

```bash
git config --global url."https://gh-proxy.com/https://github.com/".insteadOf "https://github.com/"
```

### 1. 安装与检查

```bash
git --version   # 查看版本号
git --help      # 查看帮助文档
```

### 2. 配置用户信息

```bash
git config --list                 # 查看所有配置
git config --list --show-origin   # 查看所有配置及其所在文件
```

接着配置用户名和邮箱：

```bash
git config --global user.name "yourname"                # 全局配置用户名
git config --global user.email "youremail@example.com"  # 全局配置邮箱
```

也可以单独查看用户配置：

```bash
git config user.name    # 查看用户名
git config user.email   # 查看邮箱
```

### 3. 常用基础命令

以下是最基本的 Git 操作，全部在本地完成，不涉及远程仓库：

| 命令 | 作用 |
| --- | --- |
| `git status` | 查看仓库状态 |
| `git init` | 创建空仓库或重新初始化已有仓库 |
| `git add` | 把文件添加到暂存区 |
| `git commit` | 提交改动到仓库 |
| `git log` | 打印提交日志 |
| `git branch` | 查看、添加、删除分支 |
| `git checkout` | 切换分支、标签 |
| `git merge` | 合并分支 |
| `git tag` | 新建、查看标签 |


## 二、连接本地与远程仓库

### 1. 配置 SSH 密钥

```bash
mkdir .ssh                                   # 创建 .ssh 目录（如已存在可跳过）
ssh-keygen -t rsa -C "youremail@example.com" # 生成 SSH 密钥
```

生成后，把公钥添加到 GitHub 的 Settings 中。

> **常见报错：[fatal: Could not read from remote repository.](https://blog.csdn.net/weixin_40922744/article/details/107576748)**

<details>
<summary>解决步骤</summary>

> 一般是 SSH 配置有问题，可按以下步骤排查：
> 1. 重新生成密钥：`ssh-keygen -t rsa -C "youremail@example.com"`
> 2. 将密钥添加到 ssh-agent：`ssh-add ~/.ssh/id_rsa`
> 3. 将公钥添加到你的 GitHub 账户
> 4. 用 `ssh -T git@github.com` 验证是否成功

</details>

### 2. 克隆远程仓库

```bash
git clone https://github.com/project/repo.git
```

### 3. 推送与拉取

- **git push**：把本地代码推送到远程仓库
- **git pull**：把远程仓库的最新代码拉到本地合并

```bash
git push origin master   # 把本地代码推送到远程 master 分支
git pull origin master   # 把远程最新代码更新到本地
```


<details>
<summary>本地推送远程库</summary>

```bash
cd public
git init 
git add .
git commit -m "first commit"
git branch -M main
git remote add origin https://gitee.com/Legenddog/legenddog.git
git push -u origin main
```

</details>

[fatal:remote origin already exiests.](https://blog.csdn.net/qq_34769162/article/details/116379638)

处理方法：`git remote rm origin`

## 三、Git 进阶技巧

### 1. 设置别名

用 `alias` 给常用命令起别名，可以简化输入。基本语法：

```bash
git config --global alias.别名 git命令
```

例如给 commit 和 status 起别名：

```bash
git config --global alias.ci commit   # 用 ci 代替 commit
git config --global alias.st status   # 用 st 代替 status
```

别名可以根据个人习惯自由设定。设置后，可以在 `.gitconfig` 文件里看到对应的 `alias` 配置；不需要时删掉即可。

### 2. 查看改动

```bash
git diff
```

### 3. 版本回滚

先用 `git log` 查看版本号（`commit` 后面那一长串就是版本号）：

```bash
git reset --hard 版本号   # 回退并丢弃当前工作区的修改
git reset --soft 版本号   # 回退但保留当前工作区的修改，可重新提交
```


## 四、GitHub 实用插件与工具

1. **Octotree**：目录树
2. **Sourcegraph**：在线代码查看器
3. **GITzip**

图床方案：使用 GitHub + PicGo 搭建图床。

## 参考资料

1. [GitHub-Tutorial（GitHub 小白入门）](https://github.com/CatOneTwo/GitHub-Tutorial)
2. [fatal: Could not read from remote repository.](https://blog.csdn.net/weixin_40922744/article/details/107576748)
3. [fatal: remote origin already exists.](https://blog.csdn.net/qq_34769162/article/details/116379638)

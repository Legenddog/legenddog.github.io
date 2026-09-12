---
title: 从零开始Hugo建站（二）
description: hugo网页配置
date: 2023-06-15
categories:
    - hugo
    
---


# Hugo｜从零开始Hugo建站 2



## hugo安装与学习

### 1. Hugo 是否安装成功

不管在哪种系统中安装Hugo，最后我们都可以使用下面命令查看Hugo 是否安装成功：
```bash
>>> hugo version
Hugo Static Site Generator v0.68.3-157669A0 linux/amd64 BuildDate: 2020-03-24T12:05:34Z
```

### 2.使用 Hugo 创建博客

hugo 安装成功后，使用 `hugo new site` 命令创建博客：
```bash
# 博客项目的名字为 myblog
hugo new site myblog  
```

### 3.主题配置 Installation

```bash
mkdir themes
cd themes
git clone git@github.com:CaiJimmy/hugo-theme-stack.git

# myblog下config.toml
# 设置网站的地址  baseurl="http://legenddog.github.io"
# 把主题目录下文件复制到根目录下 config.toml
# 注意hugo-theme-stack需要hugo version 0.86以上
# i18n配置 DefaultcontentLanguage ---- en
```

### 4.启动 Hugo 博客服务

使用下面命令启动服务：
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

# 本地预览        >>> hugo server -D 
# 生成public      >>> hugo
```

可以看到服务默认会在占用1313 端口，在浏览器中访问http://localhost:1313/ 地址。

### 5.把本地的blog推送远端

```bash
cd public
git init
git add .
git commit -m "first commit"
git branch -M main
git remote add origin ----
git push -u origin main 
```
一些关于linux的好用的bash命令
```bash
$ which hugo
$ pwd
$ history
```

wsl GUI GNOME

# 资料参考：

1.[如何用hugo 搭建博客](https://zhuanlan.zhihu.com/p/126298572)


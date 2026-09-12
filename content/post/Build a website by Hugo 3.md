---
title: 从零开始Hugo建站（三）
description: hugo网页配置
date: 2023-06-15
categories:
    - hugo
    
---


# Hugo｜从零开始Hugo建站 3


## HUGO站点文件配置

[Hugo站点文件夹内容](https://gohugo.io/getting-started/directory-structure/)

```bash

├── archetypes
│   └── default.md
├── config.toml/yaml          # 博客站点的配置文件
├── content                   # 博客文章所在目录
├── data                
├── layouts                   # 网站布局
├── static                    # 一些静态内容（存放网站使用的图片、音视频）
├── assets                    # 一般存放开发过程中自己写的静态资源  
├── public                    # hugo生成的静态html资源
└── themes                    # 博客主题

```

## HUGO stack theme 配置

[HUGO stack theme starter](https://stack.jimmycai.com/guide/getting-started)

| 配置                                | 含义                                |
|  -------                            | -------                             |
| Introduction                        | 简介                                |
| Site Configs                        | 站点配置                             |
| i18n Configs                        | i18n配置（国际语言设置）              |
| Custom Menu                         | 自定义菜单                           |
| Custom Header / Footer              | 自定义页眉/页脚                      |
| Date Format                         | 日期格式                            |
| Sidebar                             | 侧栏                                |
| Footer                              | 页脚                                |
| Article                             | 文章                                |
| Comments                            | 评论                                |
| Widgets                             | 部件                                |
| Open Graph                          | 打开图形                            |
| Default Image                       | 默认图像                            |
| Color Scheme                        | 配色方案                            |
| Image Processing                    | 图像处理                            |

## HUGO toml/yaml 文件配置

[yaml文件配置](https://www.cnblogs.com/legenddog/articles/17655494.html)
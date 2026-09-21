---
title: Hugo｜从零开始Hugo建站 5
description: markdown文章写作
date: 2026-09-21
categories:
    - hugo
image: cover.jpg  
---




# Hugo｜从零开始 Hugo 建站 5

> [!NOTE] 开始写博客：Markdown 的学习运用
>学习如何创建第一篇博客文章，掌握 Markdown 基本语法，以及如何使用hugo主题中的特殊用例

## 第一节：markdown基础语法


[Markdown 基本语法 | Markdown 教程](https://markdown.com.cn/basic-syntax/)


## 第二节：markdown进阶语法


[开始写博客：Markdown 入门与多语言写作](https://liu-houliang.github.io/hugo-stack-starter/post/start-writing/)


[Markdown 语法指南](https://demo.stack.cai.im/zh/p/markdown-%E8%AF%AD%E6%B3%95%E6%8C%87%E5%8D%97/)


## 第三节：Markdown 带提示的引用（Callout）语法实现指南

>[!TIP]
> 本文介绍 GitHub 风格 Callout（提示框）语法，以及 Hugo Stack 主题中的使用方法。

### 1.什么是 Callout

你看到的"带提示的引用"是通过 `> [!类型]` 的 Callout（提示框）语法实现的。

这是 GitHub 风格的 Callout 语法，Hugo Stack 主题对它做了原生支持。写法是在普通引用块（`>`）的第一行，紧跟一个方括号加感叹号加类型名的标记。

### 2.基础语法

```markdown
> [!NOTE]
> 突出显示用户在快速浏览时也应注意的信息。

> [!TIP]
> 可选信息，帮助用户更顺利地完成任务。

> [!IMPORTANT]
> 用户成功所必需的关键信息。

> [!WARNING]
> 由于潜在风险而需要用户立即关注的关键内容。

> [!CAUTION]
> 某个操作可能带来的负面后果。
```

### 3.五种内置类型

| 标记 | 显示效果 | 图标 |
| --- | --- | --- |
| `[!NOTE]` | 备注 | 📝 |
| `[!TIP]` | 提示 | 💡 |
| `[!IMPORTANT]` | 重要 | 📌 |
| `[!WARNING]` | 警告 | ⚠️ |
| `[!CAUTION]` | 注意 | 🚨 |

标记本身不区分大小写（`[!note]` 也可以），标题文字和图标、配色是由主题在渲染时自动套用的，不需要自己写。

### 4.自定义标题

如果不想用默认标题，在方括号标记后面直接加空格再写标题文本即可，也就是页面上演示的最后一种用法：

```markdown
> [!NOTE] 自定义标题
> 如果你想使用自定义标题，可以在方括号后面添加标题文本，如上所示。
```

### 5.内容部分照常写 Markdown

Callout 正文里可以继续用普通 Markdown 语法——加粗、行内代码、链接、列表等都没问题，只需保证每行都以 `>` 开头（或至少第一行带标记）。

例如：

```markdown
> [!TIP] 小技巧
> 支持 **加粗**、`行内代码`、[链接](https://example.com) 等：
> - 列表项一
> - 列表项二
```

### 6.兼容性说明

需要注意的是，这并非 Markdown 核心规范的一部分，而是各家平台的扩展语法：

- **GitHub**：原生支持上面这套 `[!NOTE]` 写法；
- **Hugo 的 Stack 主题**：兼容该语法；
- **其他渲染器**：某些静态博客主题或编辑器里可能不识别，会退化成普通引用块显示。

### 7.效果预览

> [!NOTE]**备注**
> 突出显示用户在快速浏览时也应注意的信息。

> [!TIP]**提示**
> 可选信息，帮助用户更顺利地完成任务。

> [!IMPORTANT]**重要**
> 用户成功所必需的关键信息。

> [!WARNING]**警告**
> 由于潜在风险而需要用户立即关注的关键内容。

> [!CAUTION]**注意**
> 某个操作可能带来的负面后果。

---


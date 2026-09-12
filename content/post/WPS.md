---
title: "wps公式运用"
description: 
date: 2023-06-13
lastmod: 
weight: 3
categories:
    - wps
tags:
    - wps

---



## WPS函数运用

1、SUM、ROUND
   
**要"先四舍五入后计算总和"，您需要使用数组公式或辅助列的方法**

这是因为WPS的SUM函数不能直接对ROUND函数的结果进行求和，需要特殊处理。

**用数组公式（推荐）​**

这是最直接的方法，但需要使用数组公式：

1. **选择目标单元格**
2. **输入公式**：`=SUM(ROUND(数据范围, 2))`
3. **按Ctrl+Shift+Enter**（而不是普通的回车键）
   - 在WPS中按这三个键会创建数组公式
   - 公式会自动加上大括号：`{=SUM(ROUND(数据范围, 2))}`


2、IF

    示例公式：=IF(B1/A1<1.5,10*B1/A1,15)
    注意：IF 函数配合其他函数可进行多层嵌套。

3、RANK

    示例公式：=RANK(A2,A$2:A$11,1)
    注意：$为绝对引用

4、LOOKUP

5、TEXTJOIN



## 快捷键

[快速找到最后和第一行](https://jingyan.baidu.com/article/afd8f4de536f9a75e286e9d4.html)

CTRL+F：搜索、替换等等

在WPS表格单元格内换行，要在同一个单元格内输入多行文字，可以使用 ​Alt + Enter​ 快捷键
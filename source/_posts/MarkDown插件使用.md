---
layout: post
title: VSCode MarkDown 插件使用
date: 2020-11-18 18:51:10
tags: [VScode插件,MarkDown]
comments: true
brief: "VSCode中好用的MarkDown插件"
---

  现在越来越多人使用VSCode作为自己的开发的编辑器，分享VScode中的几款比较好用的MarkDown插件
  
<!--more-->
## MarkDown All in One

这款插件是编辑markdown文件的利器，最主要的功能是提供了常用语法的快捷键，以及实用的命令和内容的格式化

- [MarkDown All in One](#markdown-all-in-one)
  - [常用的语法快捷键列表](#常用的语法快捷键列表)
  - [常用的命令](#常用的命令)
  - [MarkDown All in one 配置选项](#markdown-all-in-one-配置选项)

### 常用的语法快捷键列表

| 组合键           | 效果                                        |
| ---------------- | ------------------------------------------- |
| Ctrl + B         | 粗体                                        |
| Ctrl + I         | 斜体                                        |
| Alt + S          | 删除线                                      |
| Ctrl + Shift + ] | 标题(uplevel)                               |
| Ctrl + Shift + [ | 标题(downlevel)                             |
| Ctrl + M         | 切换数学表达式环境                          |
| Alt + C          | 选中/取消选中 列表选择框                    |
| Ctrl+ Shift+V    | 切换预览                                    |
| Ctrl+Shift+F     | 格式化表格（Linux上的键绑定是Ctrl+Shift+I） |

**快速链接功能：复制一段链接，如http://www.baidu.com；选中文字，如百度；然后按下粘贴键。**

### 常用的命令

一般用Ctrl+Shift+P打开命令行输入命令即可：

| 命令                           | 执行效果             |
| ------------------------------ | -------------------- |
| Create Table of Contents       | 生成目录结构         |
| pdate Table of Contents        | 更新目录结构         |
| Toggle code span               | 切换代码范围         |
| Print current document to HTML | 将当前文档打印到HTML |
| Toggle math environment        | 切换数学环境         |
| Toggle unordered list          | 切换无序列表         |

### MarkDown All in one 配置选项

setting.json里配置：

| 名称                                             | 默认值   | 描述                               |
| ------------------------------------------------ | -------- | ---------------------------------- |
| markdown.extension.italic.indicator              | *        | 使用*或_包含斜体文本               |
| markdown.extension.list.indentationSize          | adaptive | 对有序和无序列表使用不同的缩进大小 |
| markdown.extension.orderedList.autoRenumber      | true     | 编辑时自动修复列表标记             |
| markdown.extension.orderedList.marker            | adaptive | 或者one：始终使用1.有序列表标记    |
| markdown.extension.preview.autoShowPreviewToSide | false    | 打开Markdown文件时自动显示预览。   |
| markdown.extension.print.absoluteImgPath         | true     | 将图像路径转换为绝对路径           |

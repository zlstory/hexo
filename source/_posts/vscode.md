---
layout: post
title: "VSCode配置"
tags: [编辑器]
date: 2017-12-26
comments: true
---
最近我的sublime好像傻了一样，准备将编辑器换成VSCode了，工欲善其事必先利其器，总结一下VSCode的所需插件以及常用快捷键，下次换电脑的时候可以直接安装了。

## 插件

VSC所有插件地址 [插件](https://marketplace.visualstudio.com/VSCode) `https://marketplace.visualstudio.com/VSCode`

### 主题插件

颜色主题：One Dark Pro

文件图标主题：vscode-icons

备注：重启有效

Monokai-Midnight(好看是好看,但是在md中语法不高亮。)

### HCJ相关插件

初级的H5代码片段以及提示：HTML Snippets

html中关于CSS的class提示： IntelliSense for CSS class names

关于ES6的快捷键：JavaScript (ES6) code snippets

提示文件路径: Path Autocomplete

CSS中代码提示：HTML CSS Support

支持sass：sass

支持stylus：language-stylus

自动修改相匹配的HTML标签: Auto Rename Tag

颜色高亮显示：Color Highlight

package.json文件显示模块当前版本和最新版本: Version Lens

单词拼写检查：Spell Checker(这个插件太恶心了，还是不要装了，只支持英文，导致我写中文md文档时全是红色波浪下划线，惹不起惹不起)

格式化文件：beauty(可自定义快捷键，在文章底部)

浏览器预览页面：open In Browser

jq自动提示：jQuery Code Snippets

### VUE相关插件

格式化VUE文件：wpy-beautify

VUE代码高亮：Vetur

安装完成之后在 设置- 用户设置 中加入

```javascript
//为指定的语法定义配置文件或使用带有特定规则的配置文件。
"emmet.syntaxProfiles": {
    "vue-html": "html",
    "vue": "html"
}

```

### MD插件

MD文档格式检查: markdownlint(感觉离卸载不远了，跟lint在一起的都不是什么好用的东西)

## 常用快捷键

VSCode的所有快捷键可以在 文件-首选项-键盘快捷方式 中查看，以下是常用快捷键。

### 打开文件

Ctrl + N 新建文件

Ctrl + R 打开最近的项目文件夹

Ctrl + P 打开本项目中的文件

Ctrl + o 在windows中打开文件

Ctrl + K Ctrl + o 在windows中打开文件夹

Ctrl + K R 打开文件所在windows中的目录位置(英文输入法状态下)

Alt + B 在浏览器中打开文件(所需插件open In Browser)

### 文件中查找

Ctrl + G可以让你导航到文件中的特定行

Ctrl + Shift + F 在文当前工作目录所有文件中查找

Ctrl + Shift + H 在文当前工作目录所有文件中查找替换

Ctrl + F 在当前文件中查找

Ctrl + H 在当前文件中查找替换

Ctrl + Shift + O 查找文件中的特定符号

### 切换文件

Ctrl + Shift + Tab 切换打开的文件

Ctrl + PageDown转到右侧的编辑器。

Ctrl + PageUp转到左侧的编辑器。

Ctrl + Shift + Tab 可切换打开的文件

### 关闭文件

Ctrl + F4 关闭活动的编辑器。

Ctrl + K + W 关闭编辑组中的所有编辑器。

Ctrl + B 切换显示左边侧边栏

### 格式化代码

Ctrl + Shift + B 格式化HCJ文件(个人自定义快捷键，所需插件beauty，并不会删除文件中多余的空白行)

Ctrl + Shift + 6 格式化vue文件(所需插件wpy-beautify)

Alt + Shift + F 格式化文件(VSC自带快捷键)

### 其他常用

Ctrl + / 注释

Ctrl + Alt + / 多行注释(自定义)

Ctrl + \ 将活动编辑器分成两部分。

Ctrl + Shift + P可以访问VS Code的所有功能，包括最常见操作的键盘快捷键。

Ctrl + Shift + [ 折叠

Ctrl + Shift + ] 展开

Ctrl + Shift + K 删除当前行

Ctrl + Shift + C 打开CMD命令行

## 自定义快捷键

文件-首选项-键盘快捷方式-keybindings.json中：

```json
//ctrl+shift+/多行注释
{
    "key":"ctrl+alt+/",
    "command": "editor.action.blockComment",
    "when": "editorTextFocus"
},
//快捷键ctrl+shift+b格式化
 {
    "key": "ctrl+shift+b",
    "command": "HookyQR.beautify",
    "when": "editorFocus"
}
```

---
title: Markdown学习笔记
categories:
  - 其他
---
# 1.Markdown语法自带格式
## 1.1 代码块

<details>
<summary style="font-size: larger;">示例源码</summary>

```shell
# VSCode终端
hexo clean; hexo s
hexo clean; hexo g; hexo d
git add .; git commit -m "npm publish"; npm version patch; 
git push

# Cmder终端
hexo clean && hexo s
hexo clean && hexo g && hexo d
git add . && git commit -m "npm publish" && npm version patch
git push
```

</details>


## 1.2 多级标题

<details>
<summary style="font-size: larger;">示例源码</summary>

```shell
# H1
## H2
### H3
#### H4
##### H5
###### H6
```

</details>


## 1.3 文字样式

<details>
<summary style="font-size: larger;">示例源码</summary>

```shell
<u>下划线演示</u>

文字**加粗**演示

文字*斜体*演示

文本`高亮`演示

文本~~删除~~线演示

<font size = 5>5号字</font>
<font face="黑体">黑体</font>
<font color=blue>蓝色</font>

<table><tr><td bgcolor=MistyRose>这里的背景色是：MistyRosen，此处输入任意想输入的内容</td></tr></table>
```

</details>



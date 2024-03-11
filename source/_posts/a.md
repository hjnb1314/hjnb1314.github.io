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


<!-- <details>
<summary style="font-size: larger;">渲染演示</summary>


```shell

VSCode终端
hexo clean; hexo s
hexo clean; hexo g; hexo d
git add .; git commit -m "npm publish"; npm version patch;
git push

Cmder终端
hexo clean && hexo s
hexo clean && hexo g && hexo d
git add . && git commit -m "npm publish" && npm version patch
git push
```
</details> -->

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


<!-- <details>
<summary style="font-size: larger;">渲染演示</summary>


见本文章标题

</details> -->

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


<!-- <details>
<summary style="font-size: larger;">渲染演示</summary>

<u>下划线演示</u>

文字**加粗**演示

文字*斜体*演示

文本`高亮`演示

文本~~删除~~线演示

<font size = 5>5号字</font>
<font face="黑体">黑体</font>
<font color=blue>蓝色</font>

<table><tr><td bgcolor=MistyRose>这里的背景色是：MistyRosen，此处输入任意想输入的内容</td></tr></table>

</details> -->

## 1.4 引用

<details>
<summary style="font-size: larger;">示例源码</summary>

```shell

>  Java
> 二级引用演示
> MySQL
> >外键
> >
> >事务
> >
> >**行级锁**(引用内部一样可以用格式)
> 
> ....

```

</details>


<!-- <details>
<summary style="font-size: larger;">渲染演示</summary>

>  Java
> 二级引用演示
> MySQL
> >外键
> >
> >事务
> >
> >**行级锁**(引用内部一样可以用格式)
> 
> ....

</details> -->

## 1.5 分割线

<details>
<summary style="font-size: larger;">示例源码</summary>

```shell

---
***

```

</details>


<!-- <details>
<summary style="font-size: larger;">渲染演示</summary>

---
***

</details> -->

## 1.6 列表(*,+,-跟空格都可以)
# 1.6.1 无序列表
<details>
<summary style="font-size: larger;">示例源码</summary>

```shell


* Java
* Python
* ...

+ Java
+ Python
+ ...

- Java
- Python
- ...

```

</details>


<!-- <details>
<summary style="font-size: larger;">渲染演示</summary>

* Java
* Python
* ...

+ Java
+ Python
+ ...

- Java
- Python
- ...

</details> -->

# 1.6.2 有序列表

<details>
<summary style="font-size: larger;">示例源码</summary>

```shell

# 注意后面有空格
1. 
2. 
3. 
4. 

```

</details>


<!-- <details>
<summary style="font-size: larger;">渲染演示</summary>

# 注意后面有空格
1. 
2. 
3. 
4. 

</details> -->

## 1.7 图片

<details>
<summary style="font-size: larger;">示例源码</summary>

```shell

# 本地图片
<img src="/assets/pusheencode.webp" alt="示例图片" style="zoom:50%;" />
# 在线图片
![code](https://cdn.jsdelivr.net/gh/fomalhaut1998/markdown_pic/img/code.png)

```

</details>


<!-- <details>
<summary style="font-size: larger;">渲染演示</summary>

# 本地图片
<img src="/assets/pusheencode.webp" alt="示例图片" style="zoom:50%;" />
# 在线图片
![code](https://cdn.jsdelivr.net/gh/fomalhaut1998/markdown_pic/img/code.png)

</details> -->

## 1.8 表格

<details>
<summary style="font-size: larger;">示例源码</summary>

```shell

| 项目标号 | 资金     | 备注 |
| -------- | -------- | ---- |
| 1        | 100，000 | 无   |
| 2        | 200，000 | 无   |
| 3        | 300,600  | 重要 |

```

</details>


<!-- <details>
<summary style="font-size: larger;">渲染演示</summary>

| 项目标号 | 资金     | 备注 |
| -------- | -------- | ---- |
| 1        | 100，000 | 无   |
| 2        | 200，000 | 无   |
| 3        | 300,600  | 重要 |

</details> -->

## 1.9 公式

<details>
<summary style="font-size: larger;">示例源码</summary>

```shell

$$
\Gamma(z)=\int_0^\infty t^{z-1}e^{-t}dt.
$$

```

</details>


<!-- <details>
<summary style="font-size: larger;">渲染演示</summary>

$$
\Gamma(z)=\int_0^\infty t^{z-1}e^{-t}dt.
$$

</details> -->

# 2.Butterfly外挂标签
## 2.1 行内文本样式 text

<details>
<summary style="font-size: larger;">标签语法</summary>

```shell

{% u 文本内容 %}
{% emp 文本内容 %}
{% wavy 文本内容 %}
{% del 文本内容 %}
{% kbd 文本内容 %}
{% psw 文本内容 %}

```

</details>

<details>
<summary style="font-size: larger;">示例源码</summary>

```shell

1. 带 {% u 下划线 %} 的文本
2. 带 {% emp 着重号 %} 的文本
3. 带 {% wavy 波浪线 %} 的文本
4. 带 {% del 删除线 %} 的文本
5. 键盘样式的文本 {% kbd command %} + {% kbd D %}
6. 密码样式的文本：{% psw 这里没有验证码 %}

```

</details>


<!-- <details>
<summary style="font-size: larger;">渲染演示</summary>

1. 带 {% u 下划线 %} 的文本
2. 带 {% emp 着重号 %} 的文本
3. 带 {% wavy 波浪线 %} 的文本
4. 带 {% del 删除线 %} 的文本
5. 键盘样式的文本 {% kbd command %} + {% kbd D %}
6. 密码样式的文本：{% psw 这里没有验证码 %}

</details> -->

## 2.2 行内文本 span

<details>
<summary style="font-size: larger;">标签语法</summary>

```shell

{% p 样式参数(参数以空格划分), 文本内容 %}

```

</details>

<details>
<summary style="font-size: larger;">配置参数</summary>

```shell
1.字体: logo, code
2.颜色: red,yellow,green,cyan,blue,gray
3.大小: small, h4, h3, h2, h1, large, huge, ultra
4.对齐方向: left, center, right
```

</details>

<details>
<summary style="font-size: larger;">示例源码</summary>

```shell

- 彩色文字
在一段话中方便插入各种颜色的标签，包括：{% span red, 红色 %}、{% span yellow, 黄色 %}、{% span green, 绿色 %}、{% span cyan, 青色 %}、{% span blue, 蓝色 %}、{% span gray, 灰色 %}。
- 超大号文字
文档「开始」页面中的标题部分就是超大号文字。
{% span center logo large, Volantis %}
{% span center small, A Wonderful Theme for Hexo %}

```

</details>

<!-- <details>
<summary style="font-size: larger;">渲染演示</summary>


- 彩色文字
在一段话中方便插入各种颜色的标签，包括：{% span red, 红色 %}、{% span yellow, 黄色 %}、{% span green, 绿色 %}、{% span cyan, 青色 %}、{% span blue, 蓝色 %}、{% span gray, 灰色 %}。
- 超大号文字
文档「开始」页面中的标题部分就是超大号文字。
{% span center logo large, Volantis %}
{% span center small, A Wonderful Theme for Hexo %}


</details> -->


## 2.3 段落文本 p

<details>
<summary style="font-size: larger;">示例源码</summary>

```shell

{% p 样式参数(参数以空格划分), 文本内容 %}

```

</details>

<details>
<summary style="font-size: larger;">配置参数</summary>

```shell
1.字体: logo, code
2.颜色: red,yellow,green,cyan,blue,gray
3.大小: small, h4, h3, h2, h1, large, huge, ultra
4.对齐方向: left, center, right

```
</details>


<details>
<summary style="font-size: larger;">示例源码</summary>

```shell

- 彩色文字
在一段话中方便插入各种颜色的标签，包括：{% p red, 红色 %}、{% p yellow, 黄色 %}、{% p green, 绿色 %}、{% p cyan, 青色 %}、{% p blue, 蓝色 %}、{% p gray, 灰色 %}。
- 超大号文字
文档「开始」页面中的标题部分就是超大号文字。
{% p center logo large, Volantis %}
{% p center small, A Wonderful Theme for Hexo %}

```

</details>


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


<details>
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


<details>
<summary style="font-size: larger;">渲染演示</summary>


见本文章标题

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


<details>
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

</details>

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


<details>
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

</details>

## 1.5 分割线

<details>
<summary style="font-size: larger;">示例源码</summary>

```shell

---
***

```

</details>


<details>
<summary style="font-size: larger;">渲染演示</summary>

---
***

</details>

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


<details>
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

</details>

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


<details>
<summary style="font-size: larger;">渲染演示</summary>

# 注意后面有空格
1. 
2. 
3. 
4. 

</details>

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


<details>
<summary style="font-size: larger;">渲染演示</summary>

# 本地图片
<img src="/assets/pusheencode.webp" alt="示例图片" style="zoom:50%;" />
# 在线图片
![code](https://cdn.jsdelivr.net/gh/fomalhaut1998/markdown_pic/img/code.png)

</details>

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


<details>
<summary style="font-size: larger;">渲染演示</summary>

| 项目标号 | 资金     | 备注 |
| -------- | -------- | ---- |
| 1        | 100，000 | 无   |
| 2        | 200，000 | 无   |
| 3        | 300,600  | 重要 |

</details>

## 1.9 公式

<details>
<summary style="font-size: larger;">示例源码</summary>

```shell

$$
\Gamma(z)=\int_0^\infty t^{z-1}e^{-t}dt.
$$

```

</details>


<details>
<summary style="font-size: larger;">渲染演示</summary>

$$
\Gamma(z)=\int_0^\infty t^{z-1}e^{-t}dt.
$$

</details>

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


<details>
<summary style="font-size: larger;">渲染演示</summary>

1. 带 {% u 下划线 %} 的文本
2. 带 {% emp 着重号 %} 的文本
3. 带 {% wavy 波浪线 %} 的文本
4. 带 {% del 删除线 %} 的文本
5. 键盘样式的文本 {% kbd command %} + {% kbd D %}
6. 密码样式的文本：{% psw 这里没有验证码 %}

</details>

## 2.2 行内文本 span

<details>
<summary style="font-size: larger;">标签语法</summary>

```shell

{% p 样式参数(参数以空格划分), 文本内容 %}

```

</details>

<details>
<summary style="font-size: larger;">配置参数</summary>



1.字体: logo, code
2.颜色: red,yellow,green,cyan,blue,gray
3.大小: small, h4, h3, h2, h1, large, huge, ultra
4.对齐方向: left, center, right




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

<details>
<summary style="font-size: larger;">渲染演示</summary>


- 彩色文字
在一段话中方便插入各种颜色的标签，包括：{% span red, 红色 %}、{% span yellow, 黄色 %}、{% span green, 绿色 %}、{% span cyan, 青色 %}、{% span blue, 蓝色 %}、{% span gray, 灰色 %}。
- 超大号文字
文档「开始」页面中的标题部分就是超大号文字。
{% span center logo large, Volantis %}
{% span center small, A Wonderful Theme for Hexo %}


</details>


## 2.3 段落文本 p

<details>
<summary style="font-size: larger;">示例源码</summary>

```shell

{% p 样式参数(参数以空格划分), 文本内容 %}

```

</details>

<details>
<summary style="font-size: larger;">配置参数</summary>

1.字体: logo, code
2.颜色: red,yellow,green,cyan,blue,gray
3.大小: small, h4, h3, h2, h1, large, huge, ultra
4.对齐方向: left, center, right

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


<details>
<summary style="font-size: larger;">渲染演示</summary>

- 彩色文字
在一段话中方便插入各种颜色的标签，包括：{% p red, 红色 %}、{% p yellow, 黄色 %}、{% p green, 绿色 %}、{% p cyan, 青色 %}、{% p blue, 蓝色 %}、{% p gray, 灰色 %}。
- 超大号文字
文档「开始」页面中的标题部分就是超大号文字。
{% p center logo large, Volantis %}
{% p center small, A Wonderful Theme for Hexo %}

</details>

## 2.4 引用note

<details>
<summary style="font-size: larger;">通用配置</summary>

```shell

note:
  # Note tag style values:
  #  - simple    bs-callout old alert style. Default.
  #  - modern    bs-callout new (v2-v3) alert style.
  #  - flat      flat callout style with background, like on Mozilla or StackOverflow.
  #  - disabled  disable all CSS styles import of note tag.
  style: simple
  icons: false
  border_radius: 3
  # Offset lighter of background in % for modern and flat styles (modern: -12 | 12; flat: -18 | 6).
  # Offset also applied to label tag variables. This option can work with disabled note tag.
  light_bg_offset: 0

```

</details>

<details>
<summary style="font-size: larger;">语法格式</summary>

```shell

# 自带icon
{% note [class] [no-icon] [style] %}
Any content (support inline tags too.io).
{% endnote %}
# 外部icon
{% note [color] [icon] [style] %}
Any content (support inline tags too.io).
{% endnote %}

```

</details>

<details>
<summary style="font-size: larger;">参数配置</summary>

1.自带icon

| 参数      | 用法     |
| --------  | -------- |
| class     |【可选】标识，不同的标识有不同的配色 （ default / primary / success / info / warning / danger ） |
| no-icon   | 【可选】不显示 icon|
| style     | 【可选】可以覆盖配置中的 style （simple/modern/flat/disabled）  |


2.外部icon

| 参数      | 用法     |
| --------  | -------- |
| class     | 【可选】标识，不同的标识有不同的配色 （ default / blue / pink / red / purple / orange / green ）） |
| no-icon   | 【可选】可配置自定义 icon (只支持 fontawesome 图标, 也可以配置 no-icon )|
| style     | 【可选】可以覆盖配置中的 style （simple/modern/flat/disabled）  |






</details>

<details>
<summary style="font-size: larger;">示例源码(自带icon)</summary>
1.simple样式

```shell

{% note simple %}默认 提示块标签{% endnote %}

{% note default simple %}default 提示块标签{% endnote %}

{% note primary simple %}primary 提示块标签{% endnote %}

{% note success simple %}success 提示块标签{% endnote %}

{% note info simple %}info 提示块标签{% endnote %}

{% note warning simple %}warning 提示块标签{% endnote %}

{% note danger simple %}danger 提示块标签{% endnote %}

```
2.modern样式

```shell
{% note modern %}默认 提示块标签{% endnote %}

{% note default modern %}default 提示块标签{% endnote %}

{% note primary modern %}primary 提示块标签{% endnote %}

{% note success modern %}success 提示块标签{% endnote %}

{% note info modern %}info 提示块标签{% endnote %}

{% note warning modern %}warning 提示块标签{% endnote %}

{% note danger modern %}danger 提示块标签{% endnote %}

```
3.flat样式

```shell
{% note flat %}默认 提示块标签{% endnote %}

{% note default flat %}default 提示块标签{% endnote %}

{% note primary flat %}primary 提示块标签{% endnote %}

{% note success flat %}success 提示块标签{% endnote %}

{% note info flat %}info 提示块标签{% endnote %}

{% note warning flat %}warning 提示块标签{% endnote %}

{% note danger flat %}danger 提示块标签{% endnote %}

```
4.disabled样式

```shell
{% note disabled %}默认 提示块标签{% endnote %}

{% note default disabled %}default 提示块标签{% endnote %}

{% note primary disabled %}primary 提示块标签{% endnote %}

{% note success disabled %}success 提示块标签{% endnote %}

{% note info disabled %}info 提示块标签{% endnote %}

{% note warning disabled %}warning 提示块标签{% endnote %}

{% note danger disabled %}danger 提示块标签{% endnote %}

```
5.no-icon样式

```shell
{% note no-icon %}默认 提示块标签{% endnote %}

{% note default no-icon %}default 提示块标签{% endnote %}

{% note primary no-icon %}primary 提示块标签{% endnote %}

{% note success no-icon %}success 提示块标签{% endnote %}

{% note info no-icon %}info 提示块标签{% endnote %}

{% note warning no-icon %}warning 提示块标签{% endnote %}

{% note danger no-icon %}danger 提示块标签{% endnote %}

```


</details>


<details>
<summary style="font-size: larger;">渲染演示(自带icon)</summary>

1.simple样式

{% note simple %}默认 提示块标签{% endnote %}

{% note default simple %}default 提示块标签{% endnote %}

{% note primary simple %}primary 提示块标签{% endnote %}

{% note success simple %}success 提示块标签{% endnote %}

{% note info simple %}info 提示块标签{% endnote %}

{% note warning simple %}warning 提示块标签{% endnote %}

{% note danger simple %}danger 提示块标签{% endnote %}

2.modern样式

{% note modern %}默认 提示块标签{% endnote %}

{% note default modern %}default 提示块标签{% endnote %}

{% note primary modern %}primary 提示块标签{% endnote %}

{% note success modern %}success 提示块标签{% endnote %}

{% note info modern %}info 提示块标签{% endnote %}

{% note warning modern %}warning 提示块标签{% endnote %}

{% note danger modern %}danger 提示块标签{% endnote %}

3.flat样式

{% note flat %}默认 提示块标签{% endnote %}

{% note default flat %}default 提示块标签{% endnote %}

{% note primary flat %}primary 提示块标签{% endnote %}

{% note success flat %}success 提示块标签{% endnote %}

{% note info flat %}info 提示块标签{% endnote %}

{% note warning flat %}warning 提示块标签{% endnote %}

{% note danger flat %}danger 提示块标签{% endnote %}

4.disabled样式

{% note disabled %}默认 提示块标签{% endnote %}

{% note default disabled %}default 提示块标签{% endnote %}

{% note primary disabled %}primary 提示块标签{% endnote %}

{% note success disabled %}success 提示块标签{% endnote %}

{% note info disabled %}info 提示块标签{% endnote %}

{% note warning disabled %}warning 提示块标签{% endnote %}

{% note danger disabled %}danger 提示块标签{% endnote %}

5.no-icon样式

{% note no-icon %}默认 提示块标签{% endnote %}

{% note default no-icon %}default 提示块标签{% endnote %}

{% note primary no-icon %}primary 提示块标签{% endnote %}

{% note success no-icon %}success 提示块标签{% endnote %}

{% note info no-icon %}info 提示块标签{% endnote %}

{% note warning no-icon %}warning 提示块标签{% endnote %}

{% note danger no-icon %}danger 提示块标签{% endnote %}

</details>

## 2.5 上标标签 tip

<details>
<summary style="font-size: larger;">标签语法</summary>

```shell

{% tip [参数，可选] %}文本内容{% endtip %}

```

</details>

<details>
<summary style="font-size: larger;">配置参数</summary>

1.样式: success,error,warning,bolt,ban,home,sync,cogs,key,bell
2.自定义图标: 支持fontawesome。

</details>

<details>
<summary style="font-size: larger;">示例源码</summary>

```shell

{% tip %}default{% endtip %}
{% tip info %}info{% endtip %}
{% tip success %}success{% endtip %}
{% tip error %}error{% endtip %}
{% tip warning %}warning{% endtip %}
{% tip bolt %}bolt{% endtip %}
{% tip ban %}ban{% endtip %}
{% tip home %}home{% endtip %}
{% tip sync %}sync{% endtip %}
{% tip cogs %}cogs{% endtip %}
{% tip key %}key{% endtip %}
{% tip bell %}bell{% endtip %}
{% tip fa-atom %}自定义font awesome图标{% endtip %}

```

</details>


<details>
<summary style="font-size: larger;">渲染演示</summary>

{% tip %}default{% endtip %}
{% tip info %}info{% endtip %}
{% tip success %}success{% endtip %}
{% tip error %}error{% endtip %}
{% tip warning %}warning{% endtip %}
{% tip bolt %}bolt{% endtip %}
{% tip ban %}ban{% endtip %}
{% tip home %}home{% endtip %}
{% tip sync %}sync{% endtip %}
{% tip cogs %}cogs{% endtip %}
{% tip key %}key{% endtip %}
{% tip bell %}bell{% endtip %}
{% tip fa-atom %}自定义font awesome图标{% endtip %}

</details>

## 2.6 动态标签 anima

<details>
<summary style="font-size: larger;">标签语法</summary>

```shell

{% tip [参数，可选] %}文本内容{% endtip %}

```

</details>

<details>
<summary style="font-size: larger;">配置参数</summary>

1.将所需的CSS类添加到图标（或DOM中的任何元素）。
2.对于父级悬停样式，需要给目标元素添加指定CSS类，同时还要给目标元素的父级元素添加CSS类faa-parent animated-hover。（详情见示例及示例源码）
You can regulate the speed of the animation by adding the CSS class or . faa-fastfaa-slow
3.可以通过给目标元素添加CSS类faa-fast或faa-slow来控制动画快慢。



</details>

<details>
<summary style="font-size: larger;">示例源码</summary>

1.On DOM load（当页面加载时显示动画）
```shell
{% tip warning faa-horizontal animated %}warning{% endtip %}
{% tip ban faa-flash animated %}ban{% endtip %}

```

2.调整动画速度
```shell
{% tip warning faa-horizontal animated faa-fast %}warning{% endtip %}
{% tip ban faa-flash animated faa-slow %}ban{% endtip %}

```
3.On hover（当鼠标悬停时显示动画）

```shell
{% tip warning faa-horizontal animated-hover %}warning{% endtip %}
{% tip ban faa-flash animated-hover %}ban{% endtip %}

```
4.On parent hover（当鼠标悬停在父级元素时显示动画）

```shell
{% tip warning faa-parent animated-hover %}<p class="faa-horizontal">warning</p>{% endtip %}
{% tip ban faa-parent animated-hover %}<p class="faa-flash">ban</p>{% endtip %}

```

</details>

<details>
<summary style="font-size: larger;">渲染演示</summary>

1.On DOM load（当页面加载时显示动画）

{% tip warning faa-horizontal animated %}warning{% endtip %}
{% tip ban faa-flash animated %}ban{% endtip %}



2.调整动画速度

{% tip warning faa-horizontal animated faa-fast %}warning{% endtip %}
{% tip ban faa-flash animated faa-slow %}ban{% endtip %}


3.On hover（当鼠标悬停时显示动画）


{% tip warning faa-horizontal animated-hover %}warning{% endtip %}
{% tip ban faa-flash animated-hover %}ban{% endtip %}


4.On parent hover（当鼠标悬停在父级元素时显示动画）


{% tip warning faa-parent animated-hover %}<p class="faa-horizontal">warning</p>{% endtip %}
{% tip ban faa-parent animated-hover %}<p class="faa-flash">ban</p>{% endtip %}



</details>

## 2.7 复选列表 checkbox


<details>
<summary style="font-size: larger;">标签语法</summary>

```shell

{% checkbox 样式参数（可选）, 文本（支持简单md） %}

```

</details>



<details>
<summary style="font-size: larger;">配置参数</summary>

1.样式: plus, minus, times
2.颜色: red,yellow,green,cyan,blue,gray
3.选中状态: checked

</details>

<details>
<summary style="font-size: larger;">示例源码</summary>

```shell
{% checkbox 纯文本测试 %}
{% checkbox checked, 支持简单的 [markdown](https://guides.github.com/features/mastering-markdown/) 语法 %}
{% checkbox red, 支持自定义颜色 %}
{% checkbox green checked, 绿色 + 默认选中 %}
{% checkbox yellow checked, 黄色 + 默认选中 %}
{% checkbox cyan checked, 青色 + 默认选中 %}
{% checkbox blue checked, 蓝色 + 默认选中 %}
{% checkbox plus green checked, 增加 %}
{% checkbox minus yellow checked, 减少 %}
{% checkbox times red checked, 叉 %}

```

</details>


<details>
<summary style="font-size: larger;">渲染演示</summary>
{% checkbox 纯文本测试 %}
{% checkbox checked, 支持简单的 [markdown](https://guides.github.com/features/mastering-markdown/) 语法 %}
{% checkbox red, 支持自定义颜色 %}
{% checkbox green checked, 绿色 + 默认选中 %}
{% checkbox yellow checked, 黄色 + 默认选中 %}
{% checkbox cyan checked, 青色 + 默认选中 %}
{% checkbox blue checked, 蓝色 + 默认选中 %}
{% checkbox plus green checked, 增加 %}
{% checkbox minus yellow checked, 减少 %}
{% checkbox times red checked, 叉 %}

</details>


<!-- 
## 1.5 分割线

<details>
<summary style="font-size: larger;">示例源码</summary>

```shell

---
***

```

</details>


<details>
<summary style="font-size: larger;">渲染演示</summary>

---
***

</details>

## 1.5 分割线

<details>
<summary style="font-size: larger;">示例源码</summary>

```shell

---
***

```

</details>


<details>
<summary style="font-size: larger;">渲染演示</summary>

---
***

</details>

## 1.5 分割线

<details>
<summary style="font-size: larger;">示例源码</summary>

```shell

---
***

```

</details>


<details>
<summary style="font-size: larger;">渲染演示</summary>

---
***

</details>

## 1.5 分割线

<details>
<summary style="font-size: larger;">示例源码</summary>

```shell

---
***

```

</details>


<details>
<summary style="font-size: larger;">渲染演示</summary>

---
***

</details>

## 1.5 分割线

<details>
<summary style="font-size: larger;">示例源码</summary>

```shell

---
***

```

</details>


<details>
<summary style="font-size: larger;">渲染演示</summary>

---
***

</details>

## 1.5 分割线

<details>
<summary style="font-size: larger;">示例源码</summary>

```shell

---
***

```

</details>


<details>
<summary style="font-size: larger;">渲染演示</summary>

---
***

</details>

## 1.5 分割线

<details>
<summary style="font-size: larger;">示例源码</summary>

```shell

---
***

```

</details>


<details>
<summary style="font-size: larger;">渲染演示</summary>

---
***

</details>

## 1.5 分割线

<details>
<summary style="font-size: larger;">示例源码</summary>

```shell

---
***

```

</details>


<details>
<summary style="font-size: larger;">渲染演示</summary>

---
***

</details> -->
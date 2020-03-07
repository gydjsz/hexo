---
title: css学习
tags:
- css
---

# 优先级

行内样式 > 内嵌样式 > 链接样式 > 导入样式

<!--more-->

---

# 选择器

## 标签选择器

```html
<style>
    p{
        color: red
    }
</style>
<p>hello world</p>
```

## 类选择器

```html
<style>
    .pr{
            color: red;
        }
</style>
<p class="pr">hello world</p>
```

## id选择器

优先级: id选择器 > 类选择器

```html
<style>
    #pr{
            color: red;
        }
</style>
<p id="pr">hello world</p>
```

## 全局选择器

对所有html元素起作用

```html
<style>
    *{
        color: red;
    }
</style>
<p>hello world</p>
```

## 后代选择器

div的后代中的所有p标签生效

```html
<style>
div p {
        color: red;
}
</style>
<div>
    <p>hello world</p>
    <h1>helloworld</h1>
</div>
```

## 交集选择器

<标签一 id="标签二">
或 <标签一 class="标签二">
或 <标签 id="标签一" class="标签二">

```html
<style>
    p.pr {
        color: red;
    }
</style>
<p class=".pr">hello world</p>
```

## 并集选择器

对逗号连接的所有元素都起作用

```html
<style>
    p, h1 {
            color: red;
    }
</style>
<p>hello world</p>
<h1>hello world</h1>
```

## 兄弟选择器

+ +选择器

某元素后相邻的兄弟元素，紧挨着的，单个的

```html
<style>
    h1 + p{
        color: red;
    }
</style>
<h1></h1>
<span>2233</span>
<span>2233</span>
```

<span style="color:red;">2233</span>
<span style="color: black;">2233</span>

+ ~选择器

某元素后所有同级的指定元素

```html
<style>
    h1 + p{
        color: red;
    }
</style>
<h1></h1>
<span>2233</span>
<span>2233</span>
```

<span style="color:red;">2233</span>
<span style="color:red;">2233</span>

## 子选择器

选择某个元素的子元素

```html
<style>
    h1>p {
        color: red;
    }
</style>
<h1>
    <span>2233</span>
    <div>
        <span>2233</span>
    </div>
</h1>
```

<span style="color:red;">2233</span>
<span>2233</span>

## 伪类

1. 在 CSS 定义中，a:hover 必须被置于 a:link 和 a:visited 之后，才是有效的。

2. 在 CSS 定义中，a:active 必须被置于 a:hover 之后，才是有效的。

3. 伪类名称对大小写不敏感。

|伪类|说明|
|:-:|:-:|:-:|
|:first-child|第一个元素|
|:link|未访问链接|
|:visited|已访问链接|
|:hover|鼠标停留在链接上|
|:active|激活链接|
|:focus|获得焦点|

```html
<style>
    a:link{
        color: red;
    }
    a:visited{
        color: green;
    }
    a:hover{
        color: blue;
    }
    a:active{
        color: orange;
    }
</style>
<a href="">本页</a>
<a href="http://www.baidu.com">百度</a>
```

<style>
    a:link{
        color: red;
    }
    a:visited{
        color: green;
    }
    a:hover{
        color: blue;
    }
    a:active{
        color: orange;
    }
</style>
<a href="">本页</a>
<a href="http://www.baidu.com">百度</a>

## 属性选择器

根据某个属性是否存在或属性值来寻找元素

E[]: 选择匹配E的元素且该元素定义了[]属性,E可以省略，表示任意元素

|选择器|说明|
|:-:|:-:|:-:|
|E[foo]|选择匹配了E的元素，且该元素定义了foo属性|
|E[foo=bar]|选择匹配了E的元素，且该元素将foo的属性值定义为bar|
|E[foo~=bar]|foo的属性值是一个以空格符分隔的列表，其中一个列表的值为bar|
|E[foo=en]|foo的属性值是一个用连字符(-)分隔的列表,值开头的字符为en|
|E[foo^=bar]|foo属性值包含了前缀为bar的子字符串|
|E[foo$=bar]|foo属性值包含了后缀为bar的子字符串|
|E[foo*=bar]|foo属性值包含b的子字符串|

## 结构伪类选择器

|选择器|说明|
|:-:|:-:|:-:|
|E:last-child|选择父元素的倒数第一个子元素E，相当于E:nth-last-child(1)|
|E:nth-child(n)|选择父元素的第n个子元素，n从1开始计算|
|E:nth-last-child(n)|选择父元素的倒数第n个子元素，n从1开始计算|
|E:first-of-type|选择父元素下同种标签的第一个元素，相当于E:nth-of-type(1)|
|E:last-of-type|选择父元素下同种标签的倒数第一个元素，相当于E:nth-last-of-type(1)|
|E:nth-of-type(n)|与:nth-child(n)作用类似，用作选择使用同种标签的第n个元素|
|E:nth-last-of-type|与:nth-last-child作用类似，用作选择同种标签的倒数第一个元素|
|E:only-child|选择父元素下仅有的一个子元素，相当于E:first-child:last-child或E:nth-child(1):nth-last-child(1)|
|E:only-of-type|选择父元素下使用同种标签的唯一子元素，相当于E:first-of-type:last-of-type或E:nth-of-type(1):nth-last-of-type(1)|
|E:empty|选择空节点，即没有子元素的元素，而且该元素也不包含任何文本节点|
|E:root|选择文档的根元素，对于HTML文档，根元素永远HTML|

## UI元素状态伪类选择器

UI元素一般指form表单元素

|选择器|说明|
|:-:|:-:|:-:|
|E:enabled|匹配E的所有可用元素|
|E:disabled|匹配E的所有不可用元素|
|E:checked|匹配所有选中状态的元素|

---

# 字体与段落属性

## 字体属性

### 字体: font-family

使用字体名称，按优先顺序以逗号隔开

```css
p{
    font-family: 华文彩云, 黑体, 宋体
}
```

当字体类型由多个字符串和空格组成，该值需要使用双引号引起来

```css
p{
    font-family: "Times New Roman"
}
```

### 字号: font-size

```css
font-size: 数值|inherit|xx-small|x-smail|small|medium|large|x-large|xx-large|larger|smaller|length
```

### 字体风格: font-style

```css
font-style: normal | italic | oblique | inherit
```

|属性值|含义|
|:-:|:-:|
|normal|默认值,显示一个标准的样式|
|italic|斜体(斜体字)|
|oblique|倾斜(让文字倾斜)|
|inherit|从父元素继承字体样式|

### 加粗: font-weight

```css
font-weight: 100-900 | bold | bolder | lighter | normal
```

|属性值|含义|
|:-:|:-:|
|bold|定义粗体字|
|bolder|更粗的字|
|lighter|更细的字|
|normal|默认，标准字体|

### 小写字母转大写字母: font-variant

|属性值|含义|
|:-:|:-:|
|normal|默认值|
|small-caps|小型大写字母|
|inherit|从父元素继承font-variant属性的值|

### 字体复合属性: font

```css
font: font-style font-variant font-weight font-size font-family
```

+ 属性之间使用空格隔开
+ font-size和font-family需要同时出现，且顺序不能改变

### 字体颜色: color

|属性值|含义|
|:-:|:-:|
|color_name|颜色名称(red)|
|hex_number|十六进制颜色(#fff)|
|rgb_number|rgb代码颜色rgb(255, 0, 0)|
|inherit|从父元素继承颜色|
|hsl_number|hsl代码颜色(hsl(0, 75%, 50%))|
|hsla_number|hsla代码颜色(hsla(120, 50%, 50%, 1))|
|rgba_number|rhba代码颜色(rgba(125, 10, 45, 0.5))|

---

## 文本样式

### 阴影: text-shadow

4个属性值

+ 第一个值表示阴影的水平位移,　可取正负值
+ 第二个值表示阴影垂直位移，　可取正负值
+ 第三个值表示阴影模糊半径, 可选
+ 第四个值表示阴影颜色值，可选

### 溢出文本: text-overflow

需要其它样式属性辅助:

+ 强制文本在一行显示: white-space: nowrap
+ 溢出内容隐藏: overflow: hidden

```css
text-overflow: clip | ellipsis
```

|属性值|含义|
|:-:|:-:|
|clip|不显示省略标记(...)|
|ellipsis|显示省略标记(...)|

### 控制换行: word-wrap

```css
word-wrap: normal | break-word
```

|属性值|含义|
|:-:|:-:|
|normal|根据文本的内容换行|
|break-word|内容将在边界内换行，|

### 保持字体尺寸不变: font-size-adjust

定义字体序列中的所有字体的大小保持同一尺寸

---

## 段落属性

### 单词间隔: word-spacing

增大词之间的空格大小

```css
word-spacing: normal | length
```

length: 可为正或负值

### 字符间隔: letter-spacing

```css
letter-spacing: normal | length
```

### 文字修饰: text-decoration

|属性值|含义|
|:-:|:-:|
|underline|下划线|
|overline|上划线|
|line-through|删除线|

### 垂直对齐方式: vertial-align

|属性值|含义|
|:-:|:-:|
|baseline|默认值|
|sub|垂直对齐文本的下标|
|super|垂直对齐文本的上标|
|top|把元素顶端与行中最高元素的顶端对齐|
|text-top|把元素的顶端与父元素字体的顶端对齐|
|middle|把此元素放置在父元素的中部|
|bottom|把元素的顶端与行中最低的元素的顶端对齐|
|text-bottom|把元素的底端与父元素字体的底端对齐|
|%|百分比排列元素,允许为负值|

### 文本转换: text-transform

|属性值|说明|
|:-:|:-:|
|capitalize|将每个单词的第一个字母转换成大写，其余无转换发生|
|uppercase|转换成大写|
|lowercase|转换成小写|

### 水平对齐: text-align

|属性值|说明|
|:-:|:-:|
|start|文本向行的开始边缘对齐|
|end|文本向行的结束边缘对齐|
|left|文本向行的左边缘对齐|
|right|文本向行的右边缘对齐|
|center|文本向行内居中对齐|
|justify|根据text-justify属性分散对齐, 即两端对齐，均匀分布|
|match-parent|继承父元素的对齐方式,start和end的值根据父元素的direction确定，并被替换为恰当的left或right。|
|inherit|继承父元素的对齐方式|

### 文本缩进: text-indent

属性值可以为百分比数字或者浮点数字,可以为负值

### 文本行高: line-height

属性值可以为百分比数字或者浮点数字,可以为负值

### 处理空白: white-space

|属性值|说明|
|:-:|:-:|
|normal|默认值,空白会被浏览器忽略|
|pre|空白会被保留|
|nowrap|文本不会换行，而是在同一行上继续,直到遇到\<br>标签为止|
|pre-wrap|保留空白字符序列，但是正常地换行|
|pre-line|合并空白符序列，但是保留换行符|
|inherit|从父元素继承属性值|

### 文本反排unicode-bidi和direction

+ unicode-bidi

|属性值|说明|
|:-:|:-:|
|normal|默认值|
|bidi-override|严格按照direction属性的值重排序。忽略隐式双向运算规则|
|embed|对象打开附加的嵌入层，direction属性的值指定嵌入层，在对象内部进行隐式重排序|

+ direction

|属性值|说明|
|:-:|:-:|
|ltr|从左到右|
|rtl|从右到左|
|inherit|值不可继承|

# 图片

## 图片样式

### 图片缩放

1. width和height

值可以为数值或百分比

2. max-width和max-height

设置图片宽高的最大值
值一般为数值类型

---

## 对齐图片

### 横向对齐

text-align

```css
text-align: left | right | center
```

### 纵向对齐

vertical-align

```css
vertical-align: baseline | top | middle | bottom
```

|值|描述|
|:-:|:-:|
|baseline|默认, 元素放置在父元素的基线上|
|top|把元素的顶端与行中最高元素的顶端对齐|
|middle|把此元素放置在父元素的中部|
|bottom|把元素的顶端与行中最低的元素的顶端对齐|

---

## 图文混排

### 文字环绕

```css
float: none | left | right
```

### 设置图片与文字间距

```css
padding: padding-top | padding-right | padding-bottom | padding-left
```
---

# 背景与边框

## 背景相关属性

### 背景颜色

```css
background-color: transparent | color
```

transparent为默认值，表示透明

### 背景图片

```css
backgroun-image: none | url(url)
```

### 背景图片重复

background-repeat

|属性值|说明|
|:-:|:-:|
|repeat|水平和垂直方向都重复平铺|
|repeat-x|水平方向重复平铺|
|repeat-y|垂直方向重复平铺|
|no-repeat|不重复平铺|

### 背景图片显示

background-attachment

|属性值|说明|
|:-:|:-:|
|scroll|默认值，页面滚动时，背景图片随页面一起滚动|
|fixed|背景图片固定在页面的可见区域|

### 背景图片位置

background-position

|属性值|说明|
|:-:|:-:|
|length|设置图片与边距水平和垂直方向的距离长度|
|percentage|以页面元素框的宽度或高度的百分比放置图片|
|top|背景图片顶部居中显示|
|center|背景图片居中显示|
|bottom|背景图片底部居中显示|
|left|背景图片左部居中显示|
|right|背景图片右部居中显示|

垂直对齐可以和水平对齐一起使用

```css
background-position: top right;
background-position: 20px 30px;
```

### 图片大小

background-size

|属性值|说明|
|:-:|:-:|
|length|数值，不可为负数|
|%|0%~100%|
|cover|保持图像的宽高比例，将图片所放到正好完全覆盖所定义的背景区域|
|contain|保持宽高比例,将图片缩放到宽高正好适应所定义的背景区域|

可以设置两个值,分别为宽和高
```css
background-size: 900 800
```

### 背景显示区域

background-origin

|属性值|说明|
|:-:|:-:|
|border-box|从border区域开始显示背景|
|padding-box|从padding区域开始显示背景|
|content-box|从content区域开始显示背景|

### 背景图像裁剪区域

background-clip

|属性值|说明|
|:-:|:-:|
|border-box|从border区域开始显示背景|
|padding-box|从padding区域开始显示背景|
|content-box|从content区域开始显示背景|
|no-clip|从边框区域外裁剪背景|

### 背景复合属性

```css
background: background-color background-image background-repeat background-attachment
background-position background-size background-clip
background-origin
```

## 边框

### 边框样式

border-style

|属性值|说明|
|:-:|:-:|
|dotted|点线式|
|dashed|破折线式|
|solid|直线式|
|double|双线式|
|groove|槽线式|
|ridge|脊线式|
|inset|内嵌效果|
|outset|凸起效果|

单独设置边框一边:

1. border-top-style
2. border-bottom-style
3. border-right-style
4. border-left-style

### 边框颜色

background-color

单独设置边框一边:

1. border-top-color
2. border-bottom-color
3. border-right-color
4. border-left-color

### 边框线宽

border-width

|属性值|说明|
|:-:|:-:|
|medium|默认值|
|thin|细|
|thick|粗|
|length|自定义宽度|

### 边框复合属性

```css
border: border-width bordery-style border-color
```

## 圆角边框

### 圆角边框属性

border-radius

每个半径的四个值的顺序是：左上角，右上角，右下角，左下角。如果省略左下角，和右上角是相同的。如果省略右下角，和左上角是相同的。如果省略右上角，和左上角是相同的。

可以指定两个圆角的半径:

+ 第一个参数表示水平半径
+ 第二个参数表示垂直半径
+ 两个参数用"/"隔开

单独定义圆角
+ border-top-right-radius
+ border-top-left-radius
+ border-bottom-right-radius
+ border-bottom-left-radius

## 图像边框

### 图像边框属性

border-image

chrome: -webkit-border-image
firfox: -moz-border-image

|属性值|说明|
|:-:|:-:|
|none|默认值|
|url|绝对或相对url的图像|
|num|边框宽度, 使用像素值或百分比|
|stretch、repeat、round|拉伸(默认)、重复、平铺|

派生属性

+ border-top-image
+ border-right-image
+ border-left-image
+ border-bottom-image
+ border-top-left-image
+ border-top-right-image
+ border-bottom-left-image
+ border-bottom-left-image
+ border-image-source: url
+ border-image-slice 裁切图像
+ border-image-repeat 是否重复
+ border-image-width 图像大小
+ border-image-outset 图像偏移位置

---

# 超级链接和鼠标

## 超级链接伪类

|伪类|说明|
|:-:|:-:|
|a:link|未访问链接|
|a:visited|已访问链接|
|a:hover|鼠标停留在链接上|
|a:active|激活链接|

### 超级链接背景图设置

background-image

### 按钮效果

当鼠标经过链接时，将链接向下、向右移动一个像素

```css
<style>
a{
    background-color: aqua;
}
a:active{
    padding-top: 1px;
    padding-left: 1px;
}
</style>
<a href="" >链接</a>
```

## 鼠标特效

cursor

|属性值|说明|
|:-:|:-:|
|auto|按照默认状态自行改变|
|crosshair|精确定位十字|
|default|默认鼠标指针|
|hand|手型|
|move|移动|
|help|帮助|
|wait|等待|
|text|文本|
|n-resize|箭头朝上双向|
|s-resize|箭头朝下双向|
|w-resize|箭头朝左双向|
|e-resize|箭头朝右双向|
|ne-resize|箭头朝右上双向|
|se-resize|箭头朝右下双向|
|nw-resize|箭头朝左上双向|
|sw-resize|箭头朝左下双向|
|pointer|指示|
|url(url)|自定义鼠标指针|

# 表格

## 表格样式

### 表格边框样式

border-collapse

|属性值|说明|
|:-:|:-:|
|separate|默认值, 不会忽略border-spacing和empty-cells|
|collapse|边框合并为一个单一的边框,会忽略border-spacing和empty-cells|

### 表格边框宽度

border-width

### 表格颜色

color设置文本颜色
background-color设置表格背景色

设置表格隔行变色
```css
tr:nth-child(even){
    background-color: #f5fafe;
}
```

---

# 菜单

## 列表

### 无序列表

list-style-type

|属性值|说明|
|:-:|:-:|
|disc|实心园|
|circle|空心圆|
|square|实心方块|
|none|不使用任何标号|

### 有序列表

|属性值|说明|
|:-:|:-:|
|decimal|阿拉伯数字|
|lower-roman|小写罗马数字|
|upper-roman|大写罗马数字|
|lower-alpha|小写英文字母|
|upper-alpha|大写英文字母|
|none|不适用项目符号|

### 图片列表

```css
list-style-image: none | url(url)
```

### 列表缩进

list-style-position

|属性值|说明|
|:-:|:-:|
|outside|列表项目标记放置在文本以外,且环绕文本不根据标记对齐|
|inside|列表项目标记放置在文本以内,且环绕文本根据标记对齐|

### 列表复合属性

list-style

+ list-style-image
+ list-style-position
+ list-style-type

---

# 滤镜

## 滤镜概述

**filter**

|滤镜名称|说明|
|:-:|:-:|
|Alpha|设置透明度|
|BlendTrans|实现图像之间的淡入和淡出效果|
|Blur|建立模糊效果|
|Chroma|设置对象中指定的颜色为透明色|
|DropShadow|建立阴影效果|
|FlipH|将元素水平翻转|
|FlipV|将元素垂直翻转|
|Glow|建立外发光效果|
|Gray|显示为黑白图像|
|Invert|图像反相, 包括色彩、饱和度和亮度值, 类似底片效果|
|Light|设置光源效果|
|Mask|建立透明遮罩|
|RevealTransh|建立切换效果|
|Shadow|建立另一种阴影效果|
|Wave|波纹效果|
|Xray|显示图片的轮廓,类似X光片效果|

# 定位与布局

## 定位方式属性

|属性|说明|
|:-:|:-:|
|position|定义位置|
|left|指定元素的横向距左部距离|
|right|指定元素的横向距右部距离|
|top|指定元素的纵向距顶部距离|
|bottom|指定元素的纵向距底部距离|
|z-index|设置元素的层叠顺序|
|width|设置元素框宽度|
|height|设置元素框高度|
|overflow|内容溢出控制|
|clip|剪切|

### position定位

|属性值|说明|
|:-:|:-:|
|static|默认值, 无特殊定位, 不能通过z-index进行层次分级|
|relative|相对定位,对象不可重叠, 可以通过left、right、bottom和top等属性在正常文档中偏移位置, 可以通过z-index进行层次分级|
|absolute|生成绝对定位的元素,相对于static定位以外的第一个父元素进行定位.元素的位置可以通过left、right、bottom和top规定|
|fixed|生成绝对定位的元素, 相对于浏览器窗口进行定位, 元素的位置可以通过left、right、bottom和top规定|

### 层叠顺序z-index

将每个元素指定一个数字, 数字较大的元素将叠加在数字较小的元素之上

```css
z-index: auto | number
```

+ auto表示遵循父对象的定位
+ number是一个无单位的整数值，可以为负
+ 该属性只作用于position的属性值为relative或absolute的对象，不能作用在窗口组件上

### 边偏移属性

left、right、top和bottom

```css
left: auto | length
```

+ auto 表示系统自动取值
+ length 表示由浮点数字和单位标识符组成的长度值或百分数

## float浮动定位

float

|属性值|说明|
|:-:|:-:|
|none|元素不浮动|
|left|浮动在左边|
|right|浮动在右边|

float属性不但改变元素的显示位置，同时会对相邻内容造成影响，定义了float属性的元素会覆盖在其他元素上，而被覆盖的区域将处于不可见状态.

如果不想让float下面的其他元素浮动环绕在该元素周围

可以使用clear

+ none表示允许两边都有浮动对象
+ both表示不允许有浮动对象
+ left表示不允许左边有浮动对象
+ right表示不允许右边有浮动对象

## overflow溢出定位

overflow

|属性值|说明|
|:-:|:-:|
|visible|若内容溢出，则溢出内容可见|
|hidden|若内容溢出，则溢出内容隐藏|
|scroll|保持元素框大小，在框内应用滚动条显示内容|
|auto|等同于scroll, 表示在需要时应用滚动条|

## visibility隐藏定位

visibility

指定是否显示一个元素生成的元素框

|属性值|说明|
|:-:|:-:|
|visible|元素可见|
|hidden|元素隐藏|
|collapse|主要用来隐藏表格的行或列。隐藏的行或列能够被其他内容使用。对于表格外的其他对象，其作用等同于hidden|

## 块和行内元素display

display

|属性值|说明|
|:-:|:-:|
|block|以块元素方式显示|
|inline|以内联元素方式显示|
|none|元素隐藏|
|list-item|以列表方式显示|
|compact|分配对象为块对像或基于内容之上的内联对象|
|marker|指定内容在容器对象之前或之后|
|inline-table|将表格显示为无前后换行的内联对象或内联容器|
|list-item|将块对象指定为列表项目, 可以添加可选项目标志|
|run-in|分配对象为块对象或基于内容之上的内联对象|
|table|将对象作为块元素级的表格显示|
|table-caption|将对象作为表格标题显示|
|table-cell|将对象作为表格单元格显示|
|table-column|将对象作为表格列显示|
|table-column-group|将对象作为表格列组显示|
|table-header-group|将对象作为表格标题组显示|
|table-footer-group|将对象作为表格脚注组显示|
|table-row|将对象作为表格行显示|
|table-row-group|将对象作为表格行组显示|

---

# 盒子和布局

## div层

### div和span的区别

div是一个块级元素，其包含的元素会自动换行;span标记是一个行内标记,其前后都不会发生换行.div可以包含span，span一般不包含div

## 盒子模型

![](box.png)

### border边框

border

### padding内边距

padding

属性可以为数值或者百分比, 不能为负数

### margin外边距

```css
margin: auto | length
```

auto表示根据内容自动调整

+ 一个参数，作用于4条边
+ 两个参数，第一个作用于上下两边，第二个作用于左右两边
+ 三个参数，第一个作用于上边，第二个作用于左右两边，第三个作用于下边

1. 非行内元素块之间, margin-bottom会被margin-top取较大者(第一个margin-bottom为50px, 第二个margin-top为10px, 很显然50px的空位是满足10px的)
2. 父子块之间的margin, 一个div包含在另一个div时, 子块margin设置以父块的content为参考

---

## 旧版弹性盒子模型

```css
display: box
```

### 弹性盒子模型属性

|属性|说明|
|:-:|:-:|
|box-orient|定义盒子分布的坐标轴|
|box-align|定义子元素在盒子内垂直方向上的空间分配方式|
|box-direction|定义盒子的显示顺序|
|box-flex|定义子元素在盒子内的自适应尺寸|
|box-flex-group|定义自适应子元素群组|
|box-lines|定义子元素分布显示|
|box-oridinal-group|定义子元素在盒子内的显示位置|
|box-pack|定义子元素在盒子内的水平方向上的空间分配方式|

### 盒子布局取向box-orient

box-orient

|属性值|说明|
|:-:|:-:|
|horizontal|盒子元素从左到右在一条水平线上显示它的子元素|
|vertical|盒子元素从上到下在一条垂直线上显示它的子元素|
|inline-axis|盒子元素沿着内联轴显示它的子元素|
|block-axis|盒子元素沿着块轴显示它的子元素|

### 盒子布局顺序box-direction

box-direction

|属性值|说明|
|:-:|:-:|
|normal|正常显示顺序|
|reverse|反向显示|
|inherit|继承上级元素的显示顺序|

### 盒子布局位置box-ordinal-group

box-ordinal-group

```css
box-ordinal-group: <integer>
```

+ 参数integer是一个自然数, 从1开始，用来设置子元素的位置序号,子元素根据这个属性从小到大排列
+ 默认序号为1, 并且序号相同的元素按照文档中加载的顺序进行排列

### 盒子弹性空间box-flex

```css
box-flex: <number>
```

+ number是一个整数或者小数
+ 当盒子中包含多个定义了box-flex属性的子元素时, 浏览器会把这些值相加, 然后根据各自的值占总值的比例来分配空间
+ box-flex只有在盒子定义了具体的width和height时才能正确解析

### 管理盒子空间box-pack和box-align

box-pack

|属性值|说明|
|:-:|:-:|
|start|所有子容器都分布在父容器的左侧，右侧留空|
|end|所有子容器都分布在父容器的右侧，左侧留空|
|justify|所有子容器平均分布|
|center|平均分配父容器剩余的空间(能压缩子容器的大小, 并拥有全局居中的效果)|

box-align

|属性值|说明|
|:-:|:-:|
|start|子容器从父容器顶部开始排列,富余空间显示在盒子底部|
|end|子容器从父容器底部开始排列,富余空间显示在盒子顶部|
|center|子容器横向居中, 富余空间在子容器两侧分配, 上面一半, 下面一半|
|baseline|所有盒子沿着他们的基线排列, 富余的空间可前可后显示|
|stretch|每个子元素的高度被调整到合适盒子的高度显示, 即所有子容器和父容器保持同一高度|

### 空间溢出管理box-lines

box-lines

|属性值|说明|
|:-:|:-:|
|single|子元素都单行或单列显示|
|multiple|子元素可以多行或多列显示|

## 新版弹性盒子模型

```css
display: flex;
```

### 元素方向flex-direction

flex-direction

|属性值|说明|
|:-:|:-:|
|row|默认, 从左到右排列|
|row-reverse|反转排列|
|column|纵向排列|
|column-reverse|反转纵向排列|

### 内容对齐justify-content

justify-content

|属性值|说明|
|:-:|:-:|
|flex-start|默认, 向行头紧挨着填充|
|flex-end|向行尾紧挨着填充|
|center|居中紧挨着填充(如果剩余的自由空间为负, 则将在两个方向上同时溢出)|
|space-between|平均分布在该行上|
|space-around|平均分布在该行上, 两边留有一半的间隔空间|

### 纵轴方向的对齐align-items

align-items

|属性值|说明|
|:-:|:-:|
|flex-start|纵轴起始位置的边界紧靠该行的纵轴起始边界|
|flex-end|纵轴起始位置的边界紧靠该行的纵轴结束边界|
|center|在纵轴上居中放置|
|baseline|如果行内轴与纵轴为同一条, 则与flex-start等效, 其它情况下该值将参与基线对齐|
|stretch|默认值|

### 换行flex-wrap

flex-wrap

|属性值|说明|
|:-:|:-:|
|nowrap|默认, 弹性容器为单行, 子项可能会溢出|
|wrap|多行, 溢出项会放置到新行|
|wrap-reverse|反转wrap排列|

### 设置各行的对齐align-content

align-content

|属性值|说明|
|:-:|:-:|
|stretch|默认, 各行会伸展以占用剩余的空间|
|flex-start|各行向起始位置堆叠|
|flex-end|各行向结束位置堆叠|
|center|各行向中间位置堆叠|
|space-between|各行平均分布|
|space-around|各行平均分布, 两端保留子元素与子元素之间间距大小的一半|

### 排序order

order

```css
order: <integer>
```

用整数值来定义排列顺序, 数值小的排列在前面, 可以为负

### 元素自身在纵轴上的对齐方式align-self

align-self

```css
align-self: auto | flex-start | flex-end | center | baseline | stretch
```

---

## 多列布局

### 列宽度column-width

```css
column-width: length | auto
```

length为数值，不可为负数

### 列数column-count

```css
column-count: auto | length
```

length为大于０的整数

### 列间距

column-gap

```css
column-gap: normal | length
```

length为数值,不可为负

### 列边框column-rule

```css
column-rule: length | style | color
```

length: 定义边框宽度,数值, 不可为负数, 功能和column-rule-width相同
style: 定义边框样式, 值包括none、hidden、dotted、dashed、solid、double、groove、ridge、inset和ouset功能和column-rule-style相同
color: 定义边框颜色, 功能和column-rule-color相同

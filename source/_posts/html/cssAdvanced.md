---
title: css进阶
tags:
- css
---

# 属性书写顺序

1. 布局定位属性：display / position / float / clear / visibility / overflow（建议 display 第一个写，毕竟关系到模式）
2. 自身属性：width / height / margin / padding / border / background
3. 文本属性：color / font / text-decoration / text-align / vertical-align / white- space / break-word
4. 其他属性（CSS3）：content / cursor / border-radius / box-shadow / text-shadow / background:linear-gradient …

<!--more-->	

# display

+ 转化为块元素: display: block
+ 转化为行内元素: display: inline
+ 转化为行内块: diplay: inline-block

a、span

# snipaste

F1: 截图
F3: 贴图
Alt: 取色
Alt-c: 复制颜色

# icomoon

网址: https://icomoon.io/

1. 进入Icomoon App

![](icomoonApp.png)

2. 编辑项目

![](editProject.png)

3. 添加图标库

![](addIcons.png)

4. 可以编辑图标

![](selectIcon.png)

![](editIcon.png)

5. 点击generator font

会下载一个txt文件, 之后将该文件后缀改为zip

解压后就可以看到一些文件, 其中的fonts文件夹就是字体图标

![](fontFile.png)

6. html中引入

注意url中的fonts路径和本地保持一致
```css
<style>
    @font-face {
      font-family: 'icomoon';
      src: url('fonts/icomoon.eot?ydpfa1');
      src: url('fonts/icomoon.eot?ydpfa1#iefix')
          format('embedded-opentype'),
        url('fonts/icomoon.ttf?ydpfa1') format('truetype'),
        url('fonts/icomoon.woff?ydpfa1') format('woff'),
        url('fonts/icomoon.svg?ydpfa1#icomoon') format('svg');
      font-weight: normal;
      font-style: normal;
      font-display: block;
    }
    span {
      font-family: 'icomoon';
    }
  </style>
<body>
  <span></span>
</body>
```

其中的图标可以在解压后的demo.html中查看, 其中的小方块即为字体图标

7. 字体图标的用处

字体图标可以设置大小颜色

8. 追加字体图标

在Icomoon App中导入压缩包中的selection.json, 之后添加新的图标, 重新下载替换原来的即可

# 背景

背景透明
```css
background-color: transparent;
```

# 继承性

text-、font-、line-、 color

# 选择器权重

|选择器|选择器权重|
|:-:|:-:|
|继承或*|0,0,0,0|
|元素选择器|0,0,0,1|
|类选择器、伪类选择器|0,0,1,0|
|ID选择器|0,1,0,0|
|行内样式|1,0,0,0|
|!important|∞|

```css
/* 权重 0,0,0,1 + 0,0,0,1 = 0,0,0,2 */
ul li {}
/* 权重 0,0,0,1 */
li {}
```

# css设置padding不会撑大父级

```css
box-sizing: border-box;
-moz-box-sizing: border-box;
-webkit-box-sizing: border-box;
```

# 解决css块元素塌陷

对于两个嵌套关系的块元素, 父元素有上边距同时子元素也有上边距, 此时父元素会有较大的外边距值

+ 为父元素设置上边框

```css
border: 1px solid transparent;
```

+ 为父元素设置上内边距

```css
padding-top: 1px;
```

+ 为父元素添加overflow: hidden

```css
overflow: hidden;
```

# box-shadow

|值|说明|
|:-:|:-:|
|h-shadow|必须, 水平阴影的位置, 可以为负|
|v-shadow|必须, 垂直阴影的位置, 可以为负|
|blur|可选, 模糊距离|
|spread|可选, 阴影的尺寸|
|color|可选, 阴影的颜色|
|inset|可选, 可将外部阴影(outset)改为内部阴影|

# 浮动

div是块级元素, 占据一行

如果要让多个div横向排列, 使用float: left

# 清除浮动

1. 额外标签

```html
<style>
    .first {
      float: left;
      background-color: red;
    }
    .second {
      float: left;
      background-color: yellow;
    }
    .thrid {
      float: left;
      background-color: blue;
    }
    .box {
      width: 200px;
      height: 200px;
      background-color: black;
    }
    .clear {
      clear: both;
    }
</style>
```

```html
<body>
  <div>
    <div class="first">a</div>
    <div class="second">b</div>
    <div class="thrid">c</div>
    <div class="clear"></div>
  </div>
  <div class="box">hello</div>
</body>
```

通过添加一个额外的div并使用clear属性来清除浮动

2. 使用overflow清除浮动

缺点: 无法显示溢出的部分

```css
.father {
    overflow: hidden;
}
```

3. :after伪元素法

```css
.clearfix:after {
  content: '';
  display: block;
  height: 0;
  clear: both;
  visibility: hidden;
}
/* IE6, 7 */
.clearfix {
  *zoom: 1;
}
```

直接在父标签中添加该类选择器clearfix即可

4. 双伪元素清除浮动

```css
.clearfix:before,
.clearfix:after {
  content: '';
  display: table;
}
.clearfix:after {
  clear: both;
}
```

# 粘性定位sticky

特点:
1. 以浏览器的可视窗口为参照点移动元素
2. 粘性定位战友原先的位置
3. 必须添加top、left、right、bottom其中一个才有效

```css
position: sticky
```

# 定位总结

|定位模式|是否占有位置|移动位置|
|:-:|:-:|:-:|
|relative(相对定位)|占有位置|相对于自身位置移动|
|absolute(绝对定位)|不占有位置|带有定位的父级|
|fixed(固定定位)|不占有位置|浏览器可视区|
|sticky(粘性定位)|占有位置|浏览器可视区|

浮动定位只会压住标准流的盒子, 不会压住标准流盒子中的文字
绝对定位(固定定位)会压住标准流的所有内容

# css高级

## 制作三角形

```css
<style>
  .box {
    width: 0;
    height: 0;
    border-top: 100px solid transparent;
    border-bottom: 100px solid blue;
    border-left: 100px solid transparent;
    border-right: 100px solid transparent;
  }
</style>
<body>
  <div class="box"></div>
</body>
```

## 表单设置

1. 轮廓线outline

取消输入框蓝色的边框
```css
input {
    outline: none;
}
```

2. 防止拖拽文本域resize

```css
textarea {
    resize: none;
}
```

## 解决图片底部默认空白缝隙问题

问题来源: 行内块元素默认会与文字的基线对齐

顶线、中线、基线、底线

解决方式一(推荐):

给图片添加vertical-align: top | middle | bottom

解决方式二:

将图片转化为块内元素: display: block

## 溢出的文字用省略号显示

1. 单行文本溢出显示省略号

```html
<style>
  div {
    width: 150px;
    height: 80px;
    background-color: rgb(111, 185, 190);
    margin: 100px auto;
    /* 强制文字一行显示 */
    white-space: nowrap;
    /* 溢出的部分隐藏起来 */
    overflow: hidden;
    /* 文字用省略号代替超出的部分 */
    text-overflow: ellipsis;
  }
</style>
<body>
  <div>测试单行文本溢出显示省略号</div>
</body>
```

2. 多行文本溢出显示省略号

```html
<style>
 div {
    width: 150px;
    height: 80px;
    background-color: rgb(111, 185, 190);
    margin: 100px auto;
    overflow: hidden;
    text-overflow: ellipsis;
    /* 弹性伸缩盒子模型显示 */
    display: -webkit-box;
    /* 限制在一个块元素显示的文本的行数 */
    -webkit-line-clamp: 2;
    /* 设置或检索神作盒子对象的子元素的排列方式 */
    -webkit-box-orient: vertical;
  }
</style>
<body>
  <div>测试单行文本溢出显示省略号</div>
</body>
```

## 清除重叠的边框

```html
<style>
  ul li {
    float: left;
    list-style: none;
    width: 150px;
    height: 200px;
    border: 1px solid red;
    margin-left: -1px;
  }
</style>
<body>
  <ul>
    <li>1</li>
    <li>2</li>
    <li>3</li>
    <li>4</li>
    <li>5</li>
  </ul>
</body>
```

## 通过提高层级使得边框产生变化

```html
<style>
  ul li {
    position: relative;
    float: left;
    list-style: none;
    width: 150px;
    height: 200px;
    border: 1px solid red;
    margin-left: -1px;
  }
  ul li:hover {
    z-index: 1;
    border: 1px solid blue;
  }
</style>
<body>
  <ul>
    <li>1</li>
    <li>2</li>
    <li>3</li>
    <li>4</li>
    <li>5</li>
  </ul>
</body>
```

## 文字围绕浮动元素

```html
<style>
  .box {
    width: 300px;
    height: 200px;
    background-color: aquamarine;
  }
  .box img {
    float: left;
    width: 150px;
    height: 200px;
    margin-right: 10px;
  }
</style>
<body>
  <div class="box">
    <img src="images/pic.png" />
    <p>
      美国国土安全部2月因新冠肺炎疫情共拒绝241人入境美国 环球网
    </p>
  </div>
</body>
```

## 行内块的运用

```html
<style>
  .box {
    text-align: center;
  }
  .box a {
    display: inline-block;
    width: 36px;
    height: 36px;
    background-color: #f7f7f7;
    border: 1px solid #ccc;
    text-align: center;
    line-height: 36px;
    text-decoration: none;
    color: #333;
  }
  .box .prev,
  .box .next {
    width: 85px;
  }
  .box .current,
  .box .elp {
    background-color: #fff;
    border: none;
  }
  .box input {
    width: 45px;
    height: 25px;
    border: 1px solid #ccc;
    outline: none;
  }
  .box button {
    width: 60px;
    height: 36px;
    background-color: #f7f7f7;
    border: 1px solid #ccc;
  }
</style>
<body>
  <div class="box">
    <a href="#" class="prev">&lt;&lt;上一页</a>
    <a href="#">1</a>
    <a href="#" class="current">2</a>
    <a href="#">3</a>
    <a href="#">4</a>
    <a href="#">5</a>
    <a href="#">6</a>
    <a href="#" class="elp">...</a>
    <a href="#" class="next">&gt;&gt;下一页</a>
    到第
    <input type="text" />
    页
    <button>提交</button>
  </div>
</body>
```

## 直角三角

```html
<style>
  .box {
    width: 0;
    height: 0;
    border-top: 100px solid transparent;
    border-right: 50px solid skyblue;
  }
</style>
<body>
  <div class="box"></div>
</body>
```

```html
<style>
  .box {
    width: 0;
    height: 0;
    border-color: transparent skyblue transparent transparent;
    border-style: solid;
    border-width: 100px 50px 0 0;
  }
</style>
<body>
  <div class="box"></div>
</body>
```

京东价格条

```html
<style>
  .box {
    width: 160px;
    height: 25px;
    line-height: 24px;
    text-align: center;
    border: 2px solid red;
  }
  .box .pay {
    position: relative;
    float: left;
    width: 90px;
    height: 100%;
    margin-right: -8px;
    background-color: red;
    color: #fff;
    font-weight: 700;
  }
  .pay i {
    position: absolute;
    width: 0;
    height: 0;
    top: 0;
    right: 0;
    border-color: transparent #fff transparent transparent;
    border-style: solid;
    border-width: 24px 10px 0 0;
  }
  .origin {
    font-size: 13px;
    color: gray;
    text-decoration: line-through;
  }
</style>
<body>
  <div class="box">
    <span class="pay"
      >￥1650
      <i></i>
    </span>
    <span class="origin">￥2450</span>
  </div>
</body>
```

# h5新特性

## 标签

+ <header\>: 头部标签
+ <nav\>: 导航标签
+ <article\>: 内容标签
+ <section\>: 定义文档某个区域
+ <aside\>: 侧边栏标签
+ <footer\>: 尾部标签

## 多媒体标签

### <video\>

|属性|值|描述|
|:-:|:-:|:-:|
|autoplay|autoplay|视频就绪自动播放|
|controls|controls|向用户显示播放控件|
|width、height|像素|设置播放器宽高|
|loop|loop|是否循环播放|
|preload|auto、none|是否预加载该视频(有了autoplay则忽略)|
|src|url|视频url地址|
|poster|imgUrl|加载等待的画面图片|
|muted|muted|静音播放|

### <audio\>

|属性|值|描述|
|:-:|:-:|:-:|
|autoplay|autoplay|自动播放|
|controls|controls|显示控件|
|loop|loop|循环播放|
|src|url|音频url|

### input类型

设置type

+ email: 限制输入为email
+ url: 限制输入为url
+ date: 限制输入为date
+ time: 限制输入为time
+ month: 限制输入为month
+ week: 限制输入为week
+ number: 限制输入number
+ tel: 手机号
+ search: 搜索框
+ color: 生成一个颜色表单

### 表单属性

|属性|值|说明|
|:-:|:-:|:-:|
|required|required|表单拥有该属性表示其内容不能为空|
|placeholder|提示文本|表单的提示信息|
|autofocus|autofocus|自动聚焦属性, 页面加载完成自动聚焦到指定表单|
|autocomplete|off/on|当用户在字段开始键入时, 浏览器基于之间键入过的值, 显示出在字段中填写的选项, 默认开启|
|multiple|multiple|可以多选文件提交|

设置placeholder样式
```css
input::placeholder {
    color: red;
}
```

# css3新特性

## 属性选择器

选择input中含有value属性的元素设置color样式
```html
<style>
 input[value] {
   color: red;
 }
</style>
<body>
  <input type="text" value="123" />
  <input type="number" />
</body>
```

选择input中type属性的值为number的标签设置其color
```html
<style>
  input[type='number'] {
    color: blue;
  }
</style>
</head>
<body>
  <input type="number" />
  <input type="text" />
</body>
```

选择class中以icon开头的

```html
<style>
  div[class^=icon] {
      color: red;
  }
</style>
<body>
    <div class="icon1"></div>
    <div class="icon2"></div>
    <div class="test"></div>
</body>
```

选择class中以data开头的

```html
<style>
  div[class$=data] {
      color: red;
  }
</style>
<body>
    <div class="icon1-data"></div>
    <div class="icon2-data"></div>
    <div class="icon3"></div>
</body>
```

选择class中含icon的

```html
<style>
  div[class*=icon] {
      color: red;
  }
</style>
<body>
    <div class="icon1-data"></div>
    <div class="icon2-data"></div>
    <div class="test"></div>
</body>
```

类选择器、属性选择器、伪类选择器权重为10

## 结构伪类选择器

nth-child(n): 选择某个父元素的一个或多个特定的子元素

+ n可以是数字, 数字从1开始
+ 可以是关键字, even是偶数, odd是奇数
+ 可以是公式, 变量只能是n, 从0开始, 公式值为0或超出大小则忽略

1. 选择偶数行变红色

```html
<style>
 ul li:nth-child(even) {
   color: red;
 }
</style>
<body>
  <ul>
    <li>1</li>
    <li>2</li>
    <li>3</li>
    <li>4</li>
    <li>5</li>
    <li>6</li>
  </ul>
</body>
```

2. 所有行变红
```html
<style>
 ul li:nth-child(n) {
   color: red;
 }
</style>
<body>
  <ul>
    <li>1</li>
    <li>2</li>
    <li>3</li>
    <li>4</li>
    <li>5</li>
    <li>6</li>
  </ul>
</body>
```

3. 公式

|公式|取值|
|:-:|:-:|
|2n|偶数|
|2n+1|奇数|
|5n|5 10 15 ...|
|n + 5|从第5个开始到最后|
|-n + 5|前5个|

前3列变红

```html
<style>
  ul li:nth-child(-n + 3) {
    color: red;
  }
</style>
<body>
  <ul>
    <li>1</li>
    <li>2</li>
    <li>3</li>
    <li>4</li>
    <li>5</li>
    <li>6</li>
  </ul>
</body>
```

4. nth-child和nth-of-type的区别

nth-child: 选择父元素的第n个子元素
nth-of-type: 选择使用同种标签的第n个元素

区别:
+ nth-child先将盒子都排序号, 然后再选择盒子
+ nth-of-type先将指定元素的盒子都排序号, 然后再选择盒子


使用nth-child可以发现样式没有反应
```css
<style>
    div div:nth-child(1) {
      color: red;
    }
  </style>
<body>
<div>
  <span></span>
  <div>1</div>
  <div>2</div>
  <div>3</div>
</div>
```

使用nth-of-type可以发现样式改变
```css
<style>
    div div:nth-of-type(1) {
      color: red;
    }
  </style>
<body>
<div>
  <span></span>
  <div>1</div>
  <div>2</div>
  <div>3</div>
</div>
```

div权重为1, nth-of-type权重为10, 总权重1 + 1 + 10 = 12
```css
div div:nth-of-type(1)
```

## 伪元素选择器

### 说明

伪元素选择器可以帮助我们利用css创建新标签元素, 而不需要html标签, 从而简化html结构

|选择符|说明|
|::before|在元素内部的前面插入内容|
|::after|在元素内部的后面插入内容|

+ before和after创建一个元素, 属于行内元素
+ 新创建的元素在文档树中找不到
+ 语法 element::before{}
+ before和after必须有content属性
+ before在父元素内容的前面创建元素, after在父元素内容的后面插入元素
+ 伪元素选择器和标签选择器一样, 权重为1

### 案例

```html
<style>
  div {
    width: 200px;
    height: 200px;
    background-color: aqua;
  }
  div::before {
    content: 'my ';
  }
  div::after {
    content: ' 张三';
  }
</style>
<body>
  <div>name is</div>
</body>
```

在div中添加图标
其中after中的content内容可以用相应的代码来替代小方块
```html
<style>
    @font-face {
        font-family: 'icomoon';
        src: url('css/fonts/icomoon.eot?ydpfa1');
        src: url('css/fonts/icomoon.eot?ydpfa1#iefix') format('embedded-opentype'),
            url('css/fonts/icomoon.ttf?ydpfa1') format('truetype'),
            url('css/fonts/icomoon.woff?ydpfa1') format('woff'),
            url('css/fonts/icomoon.svg?ydpfa1#icomoon') format('svg');
        font-weight: normal;
        font-style: normal;
        font-display: block;
    }

    div {
        position: relative;
        width: 200px;
        height: 35px;
        border: 1px solid red;
    }

    div::after {
        position: absolute;
        top: 10px;
        right: 10px;
        font-family: 'icomoon';
        content: '\e900';
        color: blue;
        font-size: 18px;
    }

</style>
<body>
    <div></div>
</body>
```

### 仿土豆鼠标移入显示遮罩和播放按钮

利用before伪元素为图片添加遮罩

初始时设置其为不可见, 鼠标移入后设置可见

```html
<style>
    .show {
        position: relative;
        width: 448px;
        height: 252px;
        margin: 0 auto;
    }

    .show img {
        width: 100%;
        height: 100%;
    }

    .show::before {
        display: none;
        position: absolute;
        content: '';
        top: 0;
        left: 0;
        width: 100%;
        height: 100%;
        background: rgba(0, 0, 0, .4) url(images/arr.png) no-repeat center;
    }

    .show:hover::before {
        display: block;
    }

</style>
<body>
    <div class="show">
        <img src="images/tudou.jpg">
    </div>
</body>
```

### 使用伪元素清除浮动

```html
<style>
    .clearfix::before,
    .clearfix::after {
        content: '';
        display: table;
    }

    .clearfix::after {
        clear: both;
    }
</style>
```

## 滤镜

设置图片模糊

```css
img {
    filter: blur(5px);
}
```

## calc函数

借助calc函数进行+、-、\*、/的计算
```css
width: calc(100% - 80px);
```

## 过渡

```css
transition: 要过渡的属性 花费的时间 运动曲线 何时开始;
```

+ 属性: 要变化的css属性, 宽度、高度、背景颜色和内外边距.如果想要所有的属性都变化过渡, 使用all
+ 花费时间: 单位秒, 必须写单位, 0.5s
+ 运动曲线: 默认是ease, 可以省略
+ 何时开始: 单位是秒, 必须写单位, 可以设置延迟触发时间, 默认是0s, 可以省略

盒子动画过渡

```html
<style>
    .box {
        width: 100px;
        height: 100px;
        background-color: aqua;
        transition: width 0.5s ease 1s;
    }
    .box:hover {
        width: 500px;
    }
</style>
<body>
    <div class="box"></div>
</body>
```

直接写all
```html
<style>
    .box {
        width: 100px;
        height: 100px;
        background-color: aqua;
        transition: all 0.5s;
    }
    .box:hover {
        width: 500px;
        height: 400px;
    }
</style>
<body>
    <div class="box"></div>
</body>
```

# 2D转换

## 移动

translate:

+ 移动不会影响到其他元素的位置
+ 百分比单位是相对于自身元素的
+ 对行内标签没有效果

```css
transform: translate(200px, 200px)
transfrom: translateX(100px)
transfrom: translateY(100px)
```

## 旋转

```css
transform: rotate(度数)
```

+ rotate里面跟度数, 单位是deg, 如rotate(45deg)
+ 角度为正, 顺时针;为负, 逆时针
+ 默认旋转的中心店是元素的中心点

## 2D转换中心点

```css
transform-origin: x y;
```

+ 默认中心点为(50% 50%)
+ 可以设置像素或者方位(top bottom left right center)

```css
transform-origin: 50px 50px;
transform-origin: left bottom;
```

## 缩放

```css
transform: scale(x, y);
```

+ transform: scale(1, 1), 宽高放大1倍, 相当于没有放大
+ transform: scale(2, 2), 宽高放大2倍
+ transform: scale(2), 宽高放大2倍
+ transform: scale(0.5, 0.5), 宽高缩小0.5倍
+ 可以设置转换中心点, 默认以中心点缩放, 缩放不影响其他盒子

## 2D转换的综合写法

```css
transform: translate() rotate() scale()
```

+ 顺序会影响转换效果(先旋转会改变坐标轴方向)
+ 同时有唯一和其他属性时, 先将位移放到最前

# 动画

1. 定义动画

先定义动画, 再使用动画

```css
@keyframes 动画名称 {
    0% {
        width: 100px;
    }
    100% {
        width: 200px;
    }
}
```

2. 动画序列

+ 0%是动画的开始, 100%是动画的完成.
+ 在@keyframes中规定某项css样式, 就能创建由当前样式逐渐改为新样式的动画效果
+ 使用百分比来规定变化发生的时间, 或用from和to, 等同于0%和100%

3. 使用动画

```css
div {
    animation-name: 动画名称;
    animation-duration: 持续时间;
}
```

## 常用属性

|属性|描述|
|:-:|:-:|
|@keyframes|规定动画|
|animation|所有动画属性的简写属性, 处理animation-play-state属性|
|animation-name|规定@keyframes动画的名称(必须)|
|animation-duration|规定动画完成一个周期所花费的秒或毫秒, 默认是0(必须)|
|animation-timing-function|规定动画的速度曲线, 默认是ease|
|animation-delay|规定动画何时开始, 默认是0|
|animation-iteration-count|规定动画被播放的次数, 默认是1, 还有infinite|
|animation-direction|规定动画是否在下一周期逆向播放, 默认是normal, 逆播放alternate|
|animation-play-state|规定动画是否正在运行或暂停, 默认是running, 暂停pause|
|animation-fill-mode|规定动画结束后状态, 保持forwards, 回到起始backwards|

## 动画简写属性

```css
aniamtion: 动画名称 持续时间 运动曲线 何时开始 播放次数 是否反方向 动画起始或结束的状态;
aniamtion: name duration timing-function delay iteration-count direction fil-mode;
```

暂停动画经常和鼠标经过等其他使用

```css
div:hover {
    animation-play-state: pause
}
```

## 速度曲线调节

animation-timing-function

|值|描述|
|:-:|:-:|
|linear|匀速|
|ease|默认.动画以低速开始, 然后加快, 在结束前变慢|
|ease-in|动画以低速开始|
|ease-out|动画以低速结束|
|ease-in-out|动画以低速开始和结束|
|steps()|指定了时间函数中的间隔数量(步长)|

# 浏览器私有前缀

+ -moz-: 代表firebox浏览器私有属性
+ -webkit-: safari、chrome
+ -ms-: ie
+ -o-: Opera


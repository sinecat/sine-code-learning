# css3快速上手

# 布局

![](http://mdrs.yuanjin.tech/img/20210511102802.png#crop=0&crop=0&crop=1&crop=1&id=oX9F9&originHeight=488&originWidth=2302&originalType=binary&ratio=1&rotation=0&showTitle=false&status=done&style=none&title=)

**浮动**：做文字环绕效果

**弹性盒**：单行或单列布局

**网格**：多行多列布局

## 弹性盒

> 详细文档见[MDN](https://developer.mozilla.org/zh-CN/docs/Web/CSS/CSS_Flexible_Box_Layout)
>  
> [弹性盒小游戏](https://flexboxfroggy.com/)


### 生成弹性容器和弹性项目

![](http://mdrs.yuanjin.tech/img/20210511112624.png#crop=0&crop=0&crop=1&crop=1&id=z03rU&originHeight=650&originWidth=908&originalType=binary&ratio=1&rotation=0&showTitle=false&status=done&style=none&title=)

**默认情况下，**弹性项目沿着主轴依次排列，侧轴拉伸

### 更改方向

通过`flex-direction`可更改主轴方向

![](http://mdrs.yuanjin.tech/img/20210511112510.png#crop=0&crop=0&crop=1&crop=1&id=Wf8qJ&originHeight=1058&originWidth=1522&originalType=binary&ratio=1&rotation=0&showTitle=false&status=done&style=none&title=)

### 主轴排列

通过`justify-content`属性，可以影响主轴的排列方式

![](http://mdrs.yuanjin.tech/img/20210511113617.png#crop=0&crop=0&crop=1&crop=1&id=USua5&originHeight=1224&originWidth=1482&originalType=binary&ratio=1&rotation=0&showTitle=false&status=done&style=none&title=)

### 侧轴排列

通过`align-items`属性，可以影响侧轴的排列方式

![](http://mdrs.yuanjin.tech/img/20210511114016.png#crop=0&crop=0&crop=1&crop=1&id=d00AR&originHeight=854&originWidth=1494&originalType=binary&ratio=1&rotation=0&showTitle=false&status=done&style=none&title=)

### 弹性项目伸缩

所谓伸缩，是指在**主轴方向**上，当**弹性容器**有**额外空间**时，是否需要拉伸，当**空间不足**时，是否需要**压缩**

在**弹性项目**上使用`flex`属性，可设置拉伸和压缩比例：`flex: 拉伸比例 压缩比例 初始尺寸`

拉伸示例：

![](http://mdrs.yuanjin.tech/img/20210511120916.png#crop=0&crop=0&crop=1&crop=1&id=draQa&originHeight=850&originWidth=1540&originalType=binary&ratio=1&rotation=0&showTitle=false&status=done&style=none&title=)

压缩示例：

![](http://mdrs.yuanjin.tech/img/20210511121459.png#crop=0&crop=0&crop=1&crop=1&id=DIRgo&originHeight=860&originWidth=1598&originalType=binary&ratio=1&rotation=0&showTitle=false&status=done&style=none&title=)

默认情况下，`flex: 0 1 auto`

### 主轴换行

默认情况，当主轴剩余空间不足时，按照压缩比例进行压缩，但如果设置了主轴换行，则不会压缩，直接换行显示

给**弹性容器**设置`flex-wrap: wrap`，即可主轴换行

![](http://mdrs.yuanjin.tech/img/20210511123310.png#crop=0&crop=0&crop=1&crop=1&id=dC2wX&originHeight=572&originWidth=858&originalType=binary&ratio=1&rotation=0&showTitle=false&status=done&style=none&title=)

> 尽管如此，多行多列仍然推荐使用网格布局


## 网格

> [MDN详细文档](https://developer.mozilla.org/zh-CN/docs/Web/CSS/CSS_Grid_Layout)
>  
> [阮一峰网格布局教程](http://www.ruanyifeng.com/blog/2019/03/grid-layout-tutorial.html)
>  
> [网格布局小游戏](https://cssgridgarden.com/)


**网格布局是多行多列布局的终极解决方案**

### 生成网格布局

![](http://mdrs.yuanjin.tech/img/20210511165317.png#crop=0&crop=0&crop=1&crop=1&id=MXWn8&originHeight=786&originWidth=1082&originalType=binary&ratio=1&rotation=0&showTitle=false&status=done&style=none&title=)

容器生成网格布局后，其所有子元素为**网格项目**

### 定义行和列

`grid-template-rows`：定义行

`grid-template-columns`：定义列

**它们的语法是相同的**

![](http://mdrs.yuanjin.tech/img/20210511172305.png#crop=0&crop=0&crop=1&crop=1&id=TKHSr&originHeight=1176&originWidth=1386&originalType=binary&ratio=1&rotation=0&showTitle=false&status=done&style=none&title=)

### 改变排列方向

使用属性`grid-auto-flow: column`，可使子元素按列排放

![](http://mdrs.yuanjin.tech/img/20210511173447.png#crop=0&crop=0&crop=1&crop=1&id=hC91P&originHeight=652&originWidth=660&originalType=binary&ratio=1&rotation=0&showTitle=false&status=done&style=none&title=)

### 单元格之间的间隙

```css
row-gap: 10px; /* 行间隙为10px */
column-gap: 20px; /* 列间隙为20px */
gap: 10px 20px; /* 行间隙为10px，列间隙为20px */
```

![](http://mdrs.yuanjin.tech/img/20210512132025.png#crop=0&crop=0&crop=1&crop=1&id=A8uDP&originHeight=768&originWidth=816&originalType=binary&ratio=1&rotation=0&showTitle=false&status=done&style=none&title=)

### 单元格内部的对齐

默认情况下，网格项目在单元格内部水平和垂直拉伸，以撑满单元格

可以使用属性`justify-items`设置**水平方向**的排列方式

可以使用属性`align-items`设置**垂直方向**的排列方式

它们的可取值是相同的：

```css
justify-items: start 左 | end 右 | center 中 | stretch 拉伸;
align-items: start 上 | end 下 | center 中 | stretch 拉伸;
```

![](http://mdrs.yuanjin.tech/img/20210511174450.png#crop=0&crop=0&crop=1&crop=1&id=OAfU3&originHeight=696&originWidth=658&originalType=binary&ratio=1&rotation=0&showTitle=false&status=done&style=none&title=)

可以使用速写属性`place-items: 垂直对齐方式 水平对齐方式`同时设置这两个值

```css
place-items: start center; /* 垂直靠上，水平居中 */
```

### 网格项目定位

默认情况下，网格项目依次排列到单元格中，每个网格占据一个单元格

但可以对网格项目设置`grid-area`属性来改变这一行为

使用方式为：

```css
grid-area: 起始行线编号/起始列线编号/结束行线编号/结束列线编号;
```

![](http://mdrs.yuanjin.tech/img/20210511180028.png#crop=0&crop=0&crop=1&crop=1&id=S9HFv&originHeight=856&originWidth=926&originalType=binary&ratio=1&rotation=0&showTitle=false&status=done&style=none&title=)

# 视觉

> 所谓视觉类样式，是指不影响盒子位置、尺寸的样式，例如文字颜色、背景颜色、背景图片等


## 阴影

### 盒子阴影

> [MDN详细文档](https://developer.mozilla.org/zh-CN/docs/Web/CSS/box-shadow)


通过`box-shadow`属性可以设置整个盒子的阴影

下面是一些示例
### 文字阴影

> [MDN详细文档](https://developer.mozilla.org/zh-CN/docs/Web/CSS/text-shadow)


通过`text-shadow`可以设置文字阴影

下面是一些示例
## 圆角

> [MDN详细文档](https://developer.mozilla.org/zh-CN/docs/Web/CSS/border-radius)


通过设置`border-radius`，可以设置盒子的圆角

![](http://mdrs.yuanjin.tech/img/20210512131026.png#crop=0&crop=0&crop=1&crop=1&id=DAdgm&originHeight=432&originWidth=818&originalType=binary&ratio=1&rotation=0&showTitle=false&status=done&style=none&title=)

`border-radius`可以有很多灵活的用法，将下面的代码依次粘贴到页面中测试一下

```css
border-radius: 10px; /* 同时设置4个角的圆角，半径为10px */
border-radius: 50%; /* 同时设置4个角的圆角，圆的横向半径为宽度一半，纵向半径为高度一半 */
border-radius: 10px 20px 30px 40px; /* 分别设置左上、右上、右下、左下的圆角 */
```
## 背景渐变

> [MDN详细文档](https://developer.mozilla.org/zh-CN/docs/Web/CSS/linear-gradient())


在设置**背景图片**时，除了可以使用`url()`加载一张背景图，还可以使用`linear-gradient()`函数设置背景渐变

`linear-gradient()`用于创建一张渐变的图片，语法为：

```css
/* 设置渐变背景，方向：从上到下，颜色：从#e66465渐变到#9198e5 */
background: linear-gradient(to bottom, #e66465, #9198e5);
```

![](http://mdrs.yuanjin.tech/img/20210512135028.png#crop=0&crop=0&crop=1&crop=1&id=vhqjd&originHeight=1048&originWidth=1796&originalType=binary&ratio=1&rotation=0&showTitle=false&status=done&style=none&title=)

## 变形

> [MDN详细文档](https://developer.mozilla.org/zh-CN/docs/Web/CSS/transform)


通过`transform`属性，可以使盒子的形态发生变化

该属性支持多种变形方案，常见的有:

- translate，平移
- scale，缩放
- rotate，旋转

**无论是哪一种transform，都只是视觉效果的变化，不会影响盒子的布局**

**transform不会导致浏览器reflow和rerender，因此效率极高**

### translate 平移

使用`translate`可以让盒子在原来位置上产生位移，类似于相对定位

![](http://mdrs.yuanjin.tech/img/20210512140643.png#crop=0&crop=0&crop=1&crop=1&id=NtK7b&originHeight=756&originWidth=1498&originalType=binary&ratio=1&rotation=0&showTitle=false&status=done&style=none&title=)

### scale 缩放

使用`translate`可以让盒子在基于原来的尺寸发生缩放

![](http://mdrs.yuanjin.tech/img/20210512141500.png#crop=0&crop=0&crop=1&crop=1&id=SMzyf&originHeight=836&originWidth=1476&originalType=binary&ratio=1&rotation=0&showTitle=false&status=done&style=none&title=)

### rotate 旋转

使用`rotate`属性可以在原图基础上进行旋转

```css
/* 在原图的基础上，顺时针旋转45度角 */
transform: rotate(45deg); 
/* 在原图的基础上，顺时针旋转半圈 */
transform: rotate(0.5turn);
```

可以点击下面的按钮试一下旋转效果
### 改变变形原点

> [MDN详细文档](https://developer.mozilla.org/zh-CN/docs/Web/CSS/transform-origin)


变形原点的位置，会影响到具体的变形行为

默认情况下，变形的原点在盒子中心，你可以通过`transform-origin`来改变它

```css
transform-origin: center; /* 设置原点在盒子中心 */
transform-origin: left top; /* 设置原点在盒子左上角 */
transform-origin: right bottom; /* 设置原点在盒子右下角 */
transform-origin: 30px 60px; /* 设置原点在盒子坐标 (30, 60) 位置 */
```

试一试，先点击设置原点的按钮来设置原点(已在图片中使用红色小点标记)，然后点击变形按钮进行变形
### 多种变形叠加

可以一次性设置多种变形效果

```css
/* 先旋转45度，再平移(100,100) */
transform: rotate(45deg) translate(100px, 100px);
/* 先平移(100, 100)，再旋转45度 */
transform: translate(100px, 100px) rotate(45deg);
```

注意：旋转会导致坐标系也跟着旋转，从而可能影响到后续的变形效果

下面的例子可以很好的说明这一点

[http://mdrs.yuanjin.tech/html/css-manual/multi-transform.html](http://mdrs.yuanjin.tech/html/css-manual/multi-transform.html)

> 本来打算把这个效果嵌入到markdown，但由于嵌入后出现一些未知的bug，因此只能粘贴效果地址了


# 过渡和动画

使用过渡和动画，可以让css属性变化更加丝滑

**过渡和动画无法对所有的CSS属性产生影响，能够产生影响的只有数值类属性**，例如：颜色、宽高、字体大小等等

## 过渡

> [MDN详细文档](https://developer.mozilla.org/zh-CN/docs/Web/CSS/transition)


```css
transition: 过渡属性 持续时间 过渡函数 过渡延迟
```

-  **过渡属性**
针对哪个css属性应用过渡。例如填写`transform`，则表示仅针对**transform**属性应用过渡。
若填写`all`或不填写，则表示针对所有css属性都应用过渡 
-  **持续时间**
css属性变化所持续的时间，需要带上单位。例如`3s`表示3秒，`0.5s`或`500ms`均表示500毫秒 
-  **过渡函数**
本质是css属性变化的贝塞尔曲线函数，通常直接使用预设值：
`ease-in-out`：平滑开始，平滑结束
`linear`：线性变化
`ease-in`：仅平滑开始
`ease-out`：仅平滑结束 
| linear | 规定以相同速度开始至结束的过渡效果（等于 cubic-bezier(0,0,1,1)）。(匀速) |
| --- | --- |
| ease | 规定慢速开始，然后变快，然后慢速结束的过渡效果（cubibezier(0.25,0.1,0.25,1)）（相对于匀速，中间快，两头慢）。 |
| ease-in | 规定以慢速开始的过渡效果（等于 cubic-bezier(0.42,0,1,1)）（相对于匀速，开始的时候慢，之后快）。 |
| ease-out | 规定以慢速结束的过渡效果（等于 cubic-bezier(0,0,0.58,1)）（相对于匀速，开始时快，结束时候间慢，）。 |
| ease-in-out | 规定以慢速开始和结束的过渡效果（等于 cubic-bezier(0.42,0,0.58,1)）（相对于匀速，（开始和结束都慢）两头慢）。 |

-  **过渡延迟**
书写规则和持续时间一样，表示过渡效果延迟多久后触发，不填则无延迟 

**在JS中，可以监听元素的**`**transitionstart**`**和**`**transitionend**`**事件，从而在过渡开始和过渡结束时做一些别的事情**

## 动画

> [MDN详细文档](https://developer.mozilla.org/zh-CN/docs/Web/CSS/CSS_Animations)


**动画的本质是预先定义的一套css变化规则，然后给该规则取个名字**

![](http://mdrs.yuanjin.tech/img/20210513172902.png#crop=0&crop=0&crop=1&crop=1&id=Dvfqr&originHeight=1012&originWidth=1852&originalType=binary&ratio=1&rotation=0&showTitle=false&status=done&style=none&title=)

然后，其他元素即可使用这样的规则：

```css
animation: 规则名 持续时间;
```

在应用规则时，还可以指定更多的信息

```css
animation: 规则名 持续时间 重复次数 时间函数 动画方向 延迟时间
```

一些细节：

- 定义规则时，`0%`可以书写为`from`
- 定义规则时，`100%`可以书写为`to`
- 重复次数为`infinite`时，表示无限重复
- 动画方向为`alternate`时，表示交替反向，第1次正向，第2次反向，第3次正向，第4次方向，以此类推

**在JS中，可以监听元素的**`**animationstart**`**和**`**animationnend**`**事件，从而在过渡开始和过渡结束时做一些别的事情**

# 其他

## box-sizing

一图胜千言

![](http://mdrs.yuanjin.tech/img/20210514150015.png#crop=0&crop=0&crop=1&crop=1&id=ljY6V&originHeight=1188&originWidth=1324&originalType=binary&ratio=1&rotation=0&showTitle=false&status=done&style=none&title=)

使用`border-box`控制尺寸更加直观，因此，很多网站都会加入下面的代码

```css
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
```

## 字体图标

> [MDN font-face 指令](https://developer.mozilla.org/zh-CN/docs/Web/CSS/@font-face)


css3新增了`font-face`指令，该指令可以让我们加载网络字体

其最常见的应用就是字体图标

**字体图标本质上是文字，即通过**`**color**`**设置颜色，通过**`**font-size**`**设置尺寸**

国内使用最多的字体图标平台是[阿里巴巴矢量图标库](https://www.iconfont.cn/)

登录平台后即可免费使用其所有字体图标

## 图像内容适应

> [MDN详细文档](https://developer.mozilla.org/zh-CN/docs/Web/CSS/object-fit)


css3属性`object-fit`可以控制**多媒体内容和与元素**的适应方式，通常应用在`img`或`video`元素中

一图胜千言

下图中的所有`img`元素均被固定了宽高，溢出img的部分实际上均不会显示

![](http://mdrs.yuanjin.tech/img/20210514134908.png#crop=0&crop=0&crop=1&crop=1&id=PThFD&originHeight=1036&originWidth=1624&originalType=binary&ratio=1&rotation=0&showTitle=false&status=done&style=none&title=)

## 视口单位

css3支持使用`vw`和`vh`作为单位，分别表示`视口宽度`和`视口高度`

例如`1vh`表示视口高度的`1%`，`100vw`表示视口宽度的`100%`

## 伪元素选择器

通过`::before`和`::after`选择器，可以通过css给元素生成两个子元素

![](http://mdrs.yuanjin.tech/img/20210514140049.png#crop=0&crop=0&crop=1&crop=1&id=vxBY4&originHeight=772&originWidth=1132&originalType=binary&ratio=1&rotation=0&showTitle=false&status=done&style=none&title=)

使用伪元素可以避免在HTML中使用过多的空元素

**伪元素必须要有**`**content**`**属性，否则不能生效，如果不需要有元素内容，设置**`**content:''**`

## 平滑滚动

> [MDN详细文档](https://developer.mozilla.org/zh-CN/docs/Web/CSS/scroll-behavior)


使用`scroll-behavior: smooth`，可以让滚动更加丝滑

参见MDN效果即可

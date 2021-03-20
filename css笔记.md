## 选择器 Selector

### 通配符选择器

`*`，如：* {}

### 标签选择器

`tag`，如：div {}

### 类选择器

`.`，如：.block {}

### ID 选择器

`#`，如：#username {}

### 关系选择器

#### 子元素选择器

`>`，如： div > p {}

#### 后代选择器

`  `，如：div p {}

#### 相邻元素选择器

`+`，相同父级下，当前元素后面紧挨着的指定元素（一个）

`~`，相同父级下，当前元素后面的所有指定元素（多个）

### 属性选择器

seletor[arrt 条件 ]，`=`，`*=`，`^=`，`$=`，如：a[href$=".tech"] {}

### 伪类选择器

:first-child

:last-child

:nth-child(n | even | odd)

:first-of-type

:last-of-type

:hover

:visited

:link

:focus

### 伪元素选择器

::after

::before

::first-letter

::first-line

::selection

### 权重

伪类、类和属性选择器都是 10，伪元素和元素选择器都是 1，ID 选择器为 100，其他组合进行相加即可

> tips

1. 在 vscode 中鼠标悬浮在样式表选择器上，可查看权重

### 优先级

行内 > style 样式块 > 外部样式表

#### 提升优先级至最高

使用 `!important` 可提升优先级至最高，尽量不要用，如：

```
p {
	color: chocolate !important;
}
```



## 色彩 Color

hsl(标准色盘偏移角度(色调)，饱和度，亮度)

如：道奇蓝 hsl(210deg 100% 56%)

透明度：hsla(210deg 100% 56% / 50%)，半透明

### 渐变

径向渐变 radius-gradient

线性渐变 line-gradient

### shadow

box-shadow 常设置为，如： box-shaow: 0 0 20px gray，虚化阴影显得更自然，偏移量设置为 0 则是为四周都加上阴影的小技巧

text-shadow 常设置为，如： box-shaow: 10px 10px  rgba(0, 0, 0, 0.2), 透明能更好的和背景融合

### Lighten / Darken

less 或 sasse 的函数，用于让颜色变浅或变暗

## 布局 Layout

### box-sizing

如何计算一个元素的宽和高

常用 box-sizing: border，会将 border 和 padding 计算在宽高内。content 自动压缩，如果压缩后依旧不够，会按照 border 和 padding 的实际尺寸来计算

不能继承，伪元素需要单独设置

```
*, *::before, *::after {
  box-sizing: border-box;
}
```

### 行内元素

行内元素不能设置宽和高（设置无效），需要使用 `inline-block` 转为行内块元素才可以设置

### 尺寸单位

vh, vw: 视窗的高和宽，100vh 表示当前窗口高度的 100% 

em:  当前元素字体大小的多少倍，如：默认字体大小 16px，10em 则为 16px * 10 ，即 160px

rem: 根元素，即 html 字体大小的多少倍

**tips**： 使用 em 或 rem 时将 html 的 font-size 设置为 62.5%，方便换算

### position

定位

relative：相对于其正常位置进行定位，不会被移出文档流

absolute：相对于 static 定位以外的第一个父元素进行定位

fixed：相对于视口进行定位，如果不设置 top，会相对其父元素进行定位

**tips**：

定位时，同时设置left和right，宽度设置为width，可实现宽度自适应布局

```
.box {
  position: absolute;
  left: 2rem;
  right: 2rem;
  top: 2rem;
  width: auto;
}
```

### float

浮动

**tips**：

清除浮动的小技巧，添加 ::after 伪元素，并设置clear: both

```
.clearfix::after {
    content: "";
    display: block;
    clear: both;
}
```

overfolw 也可以清除浮动但可能会有有副作用

### flex 

container 有两根轴，main(主)轴, cross(交叉) 轴

container属性：

1. flex-direction 属性可以设置主轴方向和正反，flex-flow 是 flex-direction 和 flex-warp 的缩写，

2. justify-content设置主轴对齐方式，align-items设置交叉轴对齐方式，在flex-warp（自适应换行）的情况下有多根交叉轴，可以通过align-content设置多根交叉轴的对齐方式

3. column-gap 设置 item 列间隔大小， row-gap 设置行间隔大小

item属性： 

1. align-slef 设置 item 在交叉轴上的对齐方式
2. order，仅在视觉上进行排序，默认值 0，值越小越靠前，值相同按源码顺序 
3. flex-grow 设置扩展比，指定 flex 容器中剩余空间的多少应该分配给 item
4. flex-shrink 设置收缩比， item默认宽度之和大于容器时会进行收缩
5. flex-basis 指定 item 在主轴上的初始尺寸，优先级比width更高
6. flex 是 flex-grow，flex-shrink，flex-basis 三个值的缩写，默认值 0 1 auto，item会将自身缩短以适应container，0 0 auto ，既不会伸缩也不会扩展，1 1 auto，既会伸缩又会扩展

参考：https://developer.mozilla.org/zh-CN/docs/Web/CSS/CSS_Flexible_Box_Layout

### grid

container 属性：

1. grid 是 grid-template、grid-auto-columns、grid-auto-rows、grid-auto-flow 的 缩写

2. grid-template 是 grid-template-columns 、grid-template-rows 和 grid-template-ares 的缩写，可使用 repeat + 个数 或 auto-fill自动填充，尺寸单位，百分比，fr (缩放比)，auto

3. grid-template-ares 区域，没有名字的区域可用 `. ` 表示，如

   ```
   grid-template-areas:
       "header header header"
       "asider main ."
       "footer footer footer";
   ```

4. grid-auto-columns / grid-auto-rows，设置多余自动创建的 网格 的宽高，写法与 grid-template-columns / grid-template-rows 相同

5. grid-auto-flow，填充顺序， 默认“自左向右”，可以跟上第二个参数 `dense` 合适的元素会紧挨着密集填充

6. place-items 是 align-items 和 justifiy-items 的缩写，设置所有 `item` 在 `网格` 中的位置

7. place-content 是 align-content 和 justifiy-content 的缩写，设置 `网格` 在 `容器` 中的位置

8. gap 是 column-gap 和 row-gap 的缩写，表示间距

item 属性：

1. grid-are 指定 item 区域， 可使用已定义的区域名，或网格线划分（即 grid-column 和 grid-row 的缩写形式，用 `/` 隔开， 顺序是`row start / column start / row end / column end`

2. grid-column 是 grid-column-start 和 grid-column-end 的缩写，grid-row 是grid-row-start 和 grid-row-end 的缩写 。它们之间使用 `/` 隔开，如下表示处于第一根网格线和最后一根网格线之间

   ```
   grid-row: 1 / -1;
   ```

3. place-self 是 align-self 和 justifiy-self 的缩写，设置当前 `item` 在 `网格` 中 的位置 ，可设置 start | end | center，默认值 `stretch`表示拉伸至整个 网格 大小，用法效果与 place-items 相同

### visibility

能见度

设置为 hidden，会隐藏该元素，保留默认位置

在 table 中，设置为 collapse 可折叠行或列

### 媒体查询

@media print 打印样式

@media screen 屏幕查询样式

```
.box {
	display: flex;
	flex-direction: column;
}

@media screen and (min-width: 600px) {
	.box {
		flex-direction: row;
	}
}
```

参考：https://ricostacruz.com/til/css-media-query-breakpoints

```
@custom-media --viewport-4 (min-width: 480px);
@custom-media --viewport-7 (min-width: 768px);
@custom-media --viewport-9 (min-width: 992px);
@custom-media --viewport-12 (min-width: 1200px);


@media (--viewport-4) {
  /* ... */
}
```



## 排版 Typography

### 文本超过几行省略

超过宽度省略以 ... 代替

```
/* 超过单行 */
text {
	white-space: nowrap;
    overflow: hidden;
    text-overflow: ellipsis;
}

/*  超过多行 */
text {
  overflow: hidden;
  text-overflow: ellipsis;
  display: -webkit-box;
  -webkit-line-clamp: 2; /*超过几行*/
  -webkit-box-orient: vertical; /*垂直对齐*/
}
```

### 自定义字体

需要字体文件，名称可以自定义（最好按照规范命名），这里以瘦金体为例

1. 下载需要的字体，例 `/font/瘦金简体.ttf`

2. 具体使用如下

```
@font-face {
	/* 自定义名称 */
    font-family: '瘦金简体';
    /* 字体文件路径 */
    src: url('./font/瘦金简体.ttf');
}

body {
	/* 使用自定义的字体 */
	font-family: '瘦金简体';
}
```



  效果



   <img src="images/qsbg.png" alt="青山不改绿水长流(瘦金体效果截图)" />

### 网络字体

在线工具的使用，ttf 字体，regular 和 bold，以及 width 和 style 属性

字体网站

### tips

1. vscode 生成英文乱数假文，输入 ` Lorem 字数`，中文插件`Chinese Lorem`，输入 `jw 字数`

## 图像 Image

### 图片类型的对比

webp，可以说最好，无论是具有透明度，还是文件大小，兼容大多数浏览器，但不兼容 ie

gif，色彩范围最广

### 背景图

### CSS Sprite

雪碧图，使用在线工具生成或 ps 手工制作

### 高清图

二倍图，三倍图的制作与使用

img，srcset 属性，size 属性

picture 标签，source 标签的用法

### SVG

AI 绘制

### Canvas

## 表单 Form

滑块，使用 range 实现

clear 可清除表单

file，multiple 属性可以多选文件，accept 属性可以对文件类型进行约束

image，图像按钮，用于构建热图很方便

datalist 标签， option 标签，text 文本框 list 属性，自动补全输入框

## 动画 Animation

animation，关键帧动画 keyframe

translate，translation，cubic-bezier() 函数，贝塞尔曲线，在线工具

## CSS 规范

### BEM

block element modifie，使用 `__` 连接，`——` 分类，如

```
<div class="header">
	<span class="header__price"></span>
	<span class="header__price--sell"></span>
</div>
```

### CSS 变量

```
:root {
	--color-primary: gold;
}

.box {
	background: var(--color-primary);
}
```


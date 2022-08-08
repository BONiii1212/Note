# CSS

⚠️非常重要

👑重要

⭐️一般

📎了解

## 👑CSS3新增特性

- 2D，3D变换
- animation
- 新的单位(rem, vm,vh)
- rgba
- 圆角(border-radius)
- 阴影(box-shadow)|对文字加特效(text-shadow)
- 线性渐变(gradient)
- transform(rotate旋转, scale缩放,translate定位,skew倾斜)

## ⚠️CSS样式选择器

!import > 行内样式(style="") > ID选择器(#id) > 类选择器(.class) =伪类选择器> 标签选择器 > 其他

## 📎过渡/关键帧动画

- 过渡动画需要有状态的变化


- 关键帧动画不需要状态的变化

- 关键帧动画的控制更加精细

## 👑网页三种传统布局

- 普通流（标准流/文档流）：块级元素和行内元素按照规定的排序。


- 浮动

- 定位

## ⚠️行内/块级元素

**行内元素：**

a，strong，b，em，span等为最常见的行内元素，特点为：

- 一行内可以有多个行内元素

- 宽度和高度直接设置是无效的，默认宽度是本身内容的宽度

- 行内元素只能容纳文本或其他行内元素（a里面不能再放a元素，a中可以包含块级元素）

- margin水平有效/padding水平垂直均有效

**块元素：**

h1～h6，p，div，ul，ol，li等为最常见的块元素，特点为：

- 独占一行

- 高度，宽度，外距和内距都可以控制

- 宽度默认为父级的100%

- 是一个容器及盒子，内部可以是块元素或者行内元素（文字类的元素内不能使用块级元素）

- margin/padding水平垂直均有效

**行内块元素：**

display:inline-block可以转换为行内块元素

- 元素排列在一行

- 宽度默认由内容决定

- 元素间默认有间距

- 支持宽高、外边距、内边距的所有样式的设置

- margin/padding水平垂直均有效

## 📎浮动特性

- 浮动元素会脱离标准流（脱离标准流的控制（浮）移动到指定位置（动），（俗称脱标），不再保留原先的位置，浮动只会压住后面的标准流 ）


- 浮动的元素会一行内显示并且元素顶部对齐


- 浮动的元素会具有行内块元素的特性

## ⚠️清除浮动

解决因为子级元素浮动，所引起的父元素内部高度为0的问题。

```css
/* 方法一（额外标签法）：在浮动元素的最后再添加一个额外元素（必须是块级元素），使用clear: both */ 
.clear { clear: both; }
/* 缺点：添加不必要的元素 */ 
```

```css
/* 方法二（父级元素添加overflow）：给父元素添加代码 */
.father {
	/* 清除浮动，其中的值为hidden、auto、scroll都可以 */
	overflow: hidden;
}
/* 缺点：如果继续添加内容，溢出的内容会被修剪，导致无法显示 */ 
```

```css
/* 方法三（使用after伪元素清除浮动）： 给父元素添加代码（是隔墙法的升级版，由css生成了额外元素）*/
.clearfix:after{
	content: "";
	display: block;
	height: 0;
	clear: both;
	visibility: hidden;
}
.clearfix{
  /* 兼容IE6和IE7 */
  *zoom: 1;
}
```

```css
/* 方法四（使用before和after双伪元素清除浮动）：给父元素添加代码 */
.clearfix:before,
.clearfix:after {
	content: "";
	display: table;
}
.clearfix:after {
	clear: both;
}
.clearfix{
	*zoom: 1;
}
```

## ⚠️定位position

CSS中的position属性是用来设置定位的

定位元素（positioned element）是其计算后位置属性为 `relative`, `absolute`, `fixed `或 `sticky` 的一个元素

- **静态定位**：按照标准流特性摆放位置，没有边偏移

- **相对定位**：相对定位是元素在移动位置的时候，是相对于他原来的位置来说的。不脱标，继续保留原来的位置

- **绝对定位**：绝对定位是元素在移动位置的时候，是相对于它的祖先元素（没有祖先元素或者祖先元素没有定位，则以ICB（inital container block, 初始包含块）为准定位）来说的。脱标，不占位置。

- **固定定位**：固定定位用于不管浏览器怎么滚动，元素仍在固定位置(相对于视口)。脱标，不占位置。

- **粘性定位**：粘性定位是相对定位和固定定位的混合。元素先是相对定位，但是到达一定位置以后变成了固定定位。

```css
div {
    position: sticky;
  	/* 平时为相对定位，当距离上方0px时变成固定定位 */
    top: 0px;
} 
```

**注意：**绝对定位在不使用有定位的父元素时，也无法表现出固定定位的效果，他是相对于ICB的，而固定定位是相对于屏幕视口。

**脱标：**定位的脱离标准流和浮动的有差别，浮动的后面元素上去的时候，里面的内容不会上去，而定位，后面元素加上里面的内容都会顶上去

**边偏移：**通过top（定义元素相对于其父元素上边线的距离）、buttom（定义元素相对于其父元素下边线的距离）、left（定义元素相对于其父元素左边线的距离）、right（定义元素相对于其父元素右边线的距离）四个属性来设置。（定位元素才可以设定）

**定位叠放次序（z-index）：**有定位的盒子可以使用z-index属性来控制盒子的前后次序（z轴）｜z-index的数值，越大越靠上。

## 👑CSS常用单位

- px：绝对长度单位，按精确像素展示。
- em：相对长度单位，相对于对象内文本的字体尺寸（没有的话，就是父节点字体的大小）
- rem：相对长度单位，相对于根节点html的字体大小来计算
- vh和vw：相对长度单位，相对于视口的高度和宽度
- vmin和vmax：和vh还有vw类似，宽和高中，大的用vmax表示，小的用vmin表示

## ⚠️flex布局

一种一维布局模型，一个flexbox一次只能处理一个维度（一行或者一列）上的元素布局。

- `flex-direction`:row(默认)|row-reverse|column|column-reverse（定义主轴的方向）

- `flex-wrap`:wrap(单行溢出时，换行显示)

- `align-items`:(交叉轴方向对其)stretch（拉伸对齐）|flex-start（start处对齐）|flex-end（end处对齐）|center（居中对齐）（定义交叉轴对齐方式）

- `justify-content`:（主轴排列）stretch|flex-start(从起始线排列)|flex-end(从终止线开始排列)|center(从中间排列)|space-around(使每个元素的左右空间相等)｜(剩余空间拿来做元素间隔)

**Flex元素属性：**

- **flex-basis**:指定flex元素主轴方向上的初始大小,当一个元素同时被设定flex-basis和width时候，flex-basis优先级更高
- **flex-grow**:指定flex元素主轴方向上进行延伸占据可用空间,设定flex-grow的flex元素按比例划分可用空间
- **flex-shrink**:指定的flex元素在容器空间不够的时候进行收缩,数值越大，收缩越厉害

**简写形式：**

flex:flex-grow｜flex-shrink｜flex-basis（默认值：0 1 auto）

flex:auto表示flex:1 1 auto

flex:none表示flex:0 0 auto

flex:1表示flex：1 1 0 

## 📎网格布局

网格布局能实现的flex布局都能实现

```css
const Main = styled.main`
  grid-area: main;
`;
const Container = styled.div`
  //使用grid来布局
  display: grid;
  //上中下三块的高分别是多少1fr是自适应
  grid-template-rows: 6rem calc(100vh - 6rem) 0rem;
  //左中右三块的宽度
  grid-template-columns: 0rem 1fr 0rem;
  //如何排列,这里填写的是grid-area的名称
  grid-template-areas:
    "header header header"
    "main main main";
  //盒子间距
  grid-gap: 0px;
`;
```

## ⚠️垂直居中

**通用**

- flex布局的align-items(交叉轴)和justify-content(主轴)来居中

**行内元素**

- height和line-height相同


- 设置行内元素display:table-cell;vertical-align：middle

**块级元素**

- 绝对定位配合top:50%和负的margin-top(高度的一半)


- 绝对定位配合top:50%;transform: translate(0,-50%)


- calc(50% - 50px);

## ⚠️水平居中

**通用**

- flex布局的align-items(交叉轴)和justify-content(主轴)来居中

**行内元素**

- text-align：center

**块级元素**

- 绝对定位配合left:50%和负的margin-left（宽度的一半）

- 绝对定位配合left:50%;transform:translate(-50%,0)

- calc(50% - 50px);

## ⭐️rgba和opacity透明

```css
background-color: rgba(255,255,255,0.1);
```

```css
opacity: 0.1;
```

使用rgba设置的透明仅作用于元素的颜色或其背景色，而使用opacity设置的透明会作用于元素内的所有内容(包括子节点)

## ⚠️盒子模型

盒子模型是由content（内容）、padding（内边距）、margin（外边距）和border（边框）组成的，在盒子模型中还有宽度（width）和高度（height）两大辅助性属性

- 标准盒子模型（box-sizing：content-box）width = content


- IE盒子模型（box-sizing：border-box）width = content + pading + border


❓相邻块元素垂直合并问题

上下两个块级元素上面有一个margin-bottom下面有一个margin-top，则两个块级元素间的外边距取最大的那个值，而不是相加后的值。

**解决方法：**只设置一个外边距

❓父元素塌陷问题

对于两个嵌套关系（父子关系）的块元素，父元素有上外边距同时子元素也有上外边距，此时父元素会塌陷较大的外边距值。

**解决方法：**父元素添加overflow：hidden

## ⚠️BFC

**BFC-块级格式化上下文：**

- 每一个BFC区域只包括其子元素，不包括其子元素的子元素


- 每一个BFC区域都是独立隔绝的，互不影响


**触发BFC的条件**：

- body根元素
- position不是static或者relative
- display值为table-caption｜table-cell｜inline-block｜flex｜grid
- float不是none
- overflow：auto/hidden

**用于解决：**

垂直塌陷｜包含塌陷｜子元素浮动其高度参与容器高度计算｜防止标准流被浮动元素覆盖

## 👑隐藏元素

- display:none（任何对元素的直接用户操作不会生效）
- opacity:0（影响用户交互，仍能够进行事件操作）
- visibility:hidden（影响用户交互，仍能够进行事件操作）
- position:absolute;并设置left极大负值也可以实现元素的隐藏

## 📎CSS加载阻塞问题

- CSS的渲染不会阻塞DOM树的解析构建


- CSS的渲染会阻塞DOM树的渲染


- CSS的渲染会阻塞后面的JS语句的执行

## ⭐️重绘和回流

**重绘**（repaint）：当渲染树中的元素外观（如：颜色）发生改变，不影响布局，则产生重绘

**回流**（reflow）：当渲染树中的元素的布局（尺寸、位置、隐藏/状态）发生改变，产生重绘回流

如果是JS获取Layout属性值的话也会引起回流

**从重绘和回流角度优化：**

1.需要对元素进行复杂的操作时，可以先隐藏（display："none"），操作完成后再显示

2.需要创建多个DOM节点的时，使用DocumentFragment创建完成后一次性加入document中

3.缓存要经常使用的Layout的属性值

4.避免使用css表达式

5.尽量使用css属性简写，如border代替border-width，border-style，border-color

## ⭐️判断元素是否在视口外

```javascript
Element.getBoundingClientRect()
```

返回元素的大小及其相对于视口的位置

## 布局

### ⭐️左侧固定右侧自定义布局

- 使用flex布局，右侧添加flex-grow：1使其进行自动延伸

  ```css
  father{
    display: flex;
  }
  .left{}
  .right{
  	flex-grow: 1;
  }
  ```

- 使用浮动+margin-left

  ```css
  .father{
    width: 100%;
    background-color: orange;
  }
  .left{
    width: 200px;
    float: left;
  }
  .right{
  	margin-left: 200px;
  }
  ```

- 使用定位+margin-left

  ```css
  .father{
  	width: 100%;
  	position: relative;
  }
  .left{
  	width: 200px;
  	position: absolute;
  	top: 0;
  	left: 0;
  }
  .right{
  	margin-left: 200px;
  }
  ```

### ⭐️三栏布局

- 使用grid


- 使用flex，中间元素flex 1


- 圣杯布局

### ⭐️流体布局

网页缩小和放大时网页布局也会随着浏览器的大小而改变

优点：页面布局的宽度都是百分数，页面的放大缩小都不会出现滚动条

缺点，窗口宽度较小时，行会变得很窄，不方便阅读其中的内容

```js
.left {
  float: left;
  width: 100px;
  height: 200px;
  background: red;
}
.right {
  float: right;
  width: 200px;
  height: 200px;
  background: blue;
}
.main {
  margin-left: 120px;
  margin-right: 220px;
  height: 200px;
  background: green;
}
```

### ⭐️圣杯布局

三列布局；中间主体内容前置，且高度自适应；两边内容定宽

优点：重要的内容（中间主体内容）放在文档流的前面可以优先渲染

原理：利用相对定位、浮动、负边距布局，将左右两块移动到父盒子提前设置好的padding里面

```css
.container {
  padding-left: 200px;
  padding-right: 100px;
  background-color: pink;
}
.main {
  height: 200px;
  width: 100%;
  float: left;
  background-color: orange;
}
.left {
  width: 200px;
  height: 200px;
  float: left;
  margin-left: -100%;
  position: relative;
  left: -200px;
  background-color: yellow;
}
  .right{
  width: 100px;
  height: 200px;
  float: left;
  background-color: red;
  margin-left: -100px;
  position: relative;
  left: 100px;
}
```

### ⭐️双飞翼布局

对圣杯布局的改进

优点：消除相对定位布局

原理：主体元素上设置左右边距，预留两翼位置，左右两栏使用浮动和负边距归位，消除相对定位

```css
#father {
  width: 100%;
  float: left;
}
#div1 {
  margin-left:200px;
  margin-right:200px;
  height: 200px;
  background-color: orange;
}
#div2 {
  width: 200px;
  height: 200px;
  float: left;
  margin-left: -100%;
  background-color: yellow;
}
#div3 {
  width: 200px;
  height: 200px;
  float: left;
  margin-left: -200px;
  background-color: red;
}
/* 注意father只嵌套div1 */
```




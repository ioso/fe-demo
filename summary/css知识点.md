## CSS部分-相关知识点

### 1、实现水平垂直居中的几种方法
```
// 1、使用 flex布局
<style>
  .parent {
    width: 100%;
    height: 200px;
    background: blue;
    display: flex;
    align-items: center;
    justify-content: center;
  }
  .child {
    width: 50px;
    height: 50px;
    background: #fff;
  }
</style>
<div class="parent">
  <div class="child"></div>
</div>

//2、用绝对定位相对定位实现
<style>
  .parent {
    width: 100%;
    height: 200px;
    background: blue;
    position: relative;
  }
  .child {
    width: 100px;
    height: 100px;
    position: absolute;
    top: 100px;
    left: 50%;
    margin-top: -50px;
    margin-left: -50px;
    background-color: #fff;
  }
</style>
<div class="parent">
  <div class="child"></div>
</div>
```

### 2、css html回流，重绘

#### wiki
```
https://blog.csdn.net/qq_42098849/article/details/105432195
```
  > 重绘：当页面元素样式改变不影响元素在文档流中的位置时（如background-color，border-color，visibility），浏览器只会将新样式赋予元素并进行重新绘制操作

  > 回流：当渲染树render tree中的一部分或全部因为元素的规模尺寸、布局、隐藏等改变时，浏览器重新渲染部分DOM或全部DOM的过程

  > 回流必将引起重绘，而重绘不一定会引起回流。

#### 什么时候会触发回流或重绘？
  * 添加或删除可见的DOM元素

  * 元素的位置发生变化

  * 元素的尺寸发生变化（包括外边距、内边框、边框大小、高度和宽度等）

  * 内容发生变化，比如文本变化或图片被另一个不同尺寸的图片所替代。

  * 页面渲染初始化

  * 浏览器的窗口resize尺寸变化（因为回流是根据视口的大小来计算元素的位置和大小的）

  * 最复杂的一种：获取某些属性，引发回流

    - (1) offset(Top/Left/Width/Height)
    - (2) scroll(Top/Left/Width/Height)
    - (3) cilent(Top/Left/Width/Height)
    - (4) width,height
    - (5) 调用了getComputedStyle()或者IE的currentStyle

#### 如何减少重绘和回流（重排）

##### CSS中避免回流、重绘

- 尽可能在DOM树的最末端改变class
- 避免设置多层内联样式
- 动画效果应用到position属性为absolute或fixed的元素上
- 避免使用table布局
- 使用css3硬件加速，可以让transform、opacity、filters等动画效果不会引起回流重绘

##### JS操作避免回流、重绘

- 避免使用JS一个样式修改完接着改下一个样式，最好一次性更改CSS样式，或者将样式列表定义为class的名称
- 避免频繁操作DOM，使用文档片段创建一个子树，然后再拷贝到文档中
- 先隐藏元素，进行修改后再显示该元素，因为display:none上的DOM操作不会引发回流和重绘
- 避免循环读取offsetLeft等属性，在循环之前把它们存起来
- 对于复杂动画效果,使用绝对定位让其脱离文档流，否则会引起父元素及后续元素大量的回流
* class修改样式
* 克隆要操作的节点，操作后再与原始节点交换，类似于虚拟dom
* 避免频繁直接访问计算后的样式
* 绝对布局
* 不要嵌套太深


### 3、 css盒模型有哪些及区别content-box border-box padding-box

  IE盒子模型box-sizing:border-box;（怪异模式）
  W3C标准盒子模型 box-sizing:content-box;（标准模式）默认模式

  content-box:这是默认样式指定CSS标准。测量width和height属性只包括的内容，但不是border, margin, 或者 padding。

  padding-box:width和height属性包括padding的大小，不包括border和margin
  border-box:width和height属性包括padding和border，但不是margin。这是盒模型的文档时，Internet Explorer使用Quirks模式。

  content-box不包含padding，border-box包含padding。所以如果你设置的大小是一样的，content-box看起来，会比border-box大

### 4、页面导入样式时，使用link和@import有什么区别

  @import是依赖css的，存在一定的兼容问题，并且根据浏览器渲染机制来说，他在dom树渲染完成后才会渲染，并且不能被js动态修改。

  相比之下link兼容性较好，且dom元素的样式可以被js动态修改，又因为link的权重高于@import，所以 @import适用于引入公共基础样式或第三方样式，link适用于自己写的且需要动态修改的样式。

### 5、px、em、rem的区别？

  1、px像素。绝对单位，像素px是相对于显示器屏幕分辨率而言的，是一个虚拟单位。是计算机系统的数字化图像长度单位，如果px要换算成物理长度，需要指定精度DPI。

  2、em是相对长度单位，相对于当前对象内文本的字体尺寸。如当前对行内文本的字体尺寸未被人为设置，则相对浏览器的默认字体尺寸。它会继承父级元素的字体大小，因此并不是一个固定的值。
  rem是CSS3新增的一个相对单位(root em,根em),使用rem为元素设定字体大小事，仍然是相对大小但相对的只是HTML根元素。

  3、区别：IE无法调用那些使用px作为单位的字体大小，而em和rem可以缩放，rem相对的只是HTML根元素。这个单位可谓集相对大小和绝对大小的优点于一身，通过它既可以做到只修改根元素就成比例地调整所有字体大小，又可以避免字体大小逐层复合的连锁反应。目前，除了IE8及更早版本外，所有浏览器已支持rem。

### 6、bfc

  BFC(Block formatting context)直译为"块级格式化上下文"。它是一个独立的渲染区域，只有Block-level box参与， 它规定了内部的Block-level Box如何布局，

　并且与这个区域外部毫不相干。

　#### BFC布局规则：
  
  Box是CSS布局的对象和基本单位，页面是由若干个Box组成的。

  元素的类型和display属性，决定了这个Box的类型。不同类型的Box会参与不同的Formatting Context。

  Formatting Context是页面的一块渲染区域，并且有一套渲染规则，决定了其子元素将如何定位，以及和其他元素的关系和相互作用。

  Formatting Context有BFC（Block）、IFC（Inline）、FFC（Flex）和GFC（Grid），FFC和GFC是CSS3中新增的。

  BFC布局规则

  * BFC内，盒子依次垂直排列
  * BFC内，两个盒子的垂直距离由margin属性决定。属于同一个BFC的两个相邻Box的margin会发生重叠
  * BFC内，每个盒子的左外边缘接触内部盒子的左边缘（对于从右到左的格式，右边缘接触）。即使在存在浮动的情况下也是如此，除非创建新的BFC
  * BFC的区域不会与float box重叠
  * BFC就是页面上的一个隔离的独立容器，容器里面的子元素不会影响到外面的元素，反之也如此
  * 计算BFC的高度时，浮动元素也参与计算

  如何创建BFC

  * 根元素
  * 浮动元素（float属性不为none）
  * position为absolute或者fixed
  * overflow不为visible的块元素
  * display为inline-block,table-cell,table-caption

  BFC的应用

  * 防止margin重叠（同一个BFC内的两个相邻box的margin会发生重叠，触发生成两个BFC，即不会重叠）
  * 清除内部浮动（创建一个新的BFC，因为根据BFC的规则，计算BFC的高度时，浮动元素也参与计算）
  * 自适应多栏布局（BFC区域不会与float box重叠。因为可以触发生成一个新的BFC）


### 7、圣杯布局

### 8、CSS3新特性？

  1. 过渡transition
  2. 动画animation
  3. 形状转换transform
  4. 阴影box-shadow
  5. 滤镜filter
  6. 颜色rgba
  7. 栅格布局 gird
  8. 弹性布局 flex

### 9、请阐述Float定位的工作原理

### 11、请阐述z-index属性，并说明如何形成层叠上下文（stacking context）。

### 12、请阐述块格式化上下文（Block Formatting Context）及其工作原理。

### 13、有哪些清除浮动的技术，都适用哪些情况？

### 14、请解释什么是雪碧图（css sprites），以及如何实现？

### 15、如何解决不同浏览器的样式兼容性问题？

### 16、如何为功能受限的浏览器提供页面？ 使用什么样的技术和流程？

### 17、有什么不同的方式可以隐藏内容（使其仅适用于屏幕阅读器）？

### 18、你使用过栅格系统吗？偏爱哪一个？

### 19、你是否使用过媒体查询或移动优先的布局？

### 20、你熟悉制作 SVG 吗？

### 21、除了screen，你还能说出一个 @media 属性的例子吗？

### 22、编写高效的 CSS 应该注意什么？

### 23、使用 CSS 预处理的优缺点分别是什么？

### 24、对于你使用过的 CSS 预处理，说说喜欢和不喜欢的地方？

### 25、如何实现一个使用非标准字体的网页设计？

### 26、解释浏览器如何确定哪些元素与 CSS 选择器匹配。

### 27、描述伪元素及其用途。

### 28、说说你对盒模型的理解，以及如何告知浏览器使用不同的盒模型渲染布局
* { box-sizing: border-box; }会产生怎样的效果？

### 29、display的属性值都有哪些？

inline和inline-block有什么区别？

relative、fixed、absolute和static四种定位有什么区别？

### 30、你使用过哪些现有的 CSS 框架？你是如何改进它们的？

### 31、你了解 CSS Flexbox 和 Grid 吗？

### 32、请解释在编写网站时，响应式与移动优先的区别。

### 33、响应式设计与自适应设计有何不同？

### 34、你有没有使用过视网膜分辨率的图形？当中使用什么技术？

### 35、什么情况下，用translate()而不用绝对定位？什么时候，情况相反


### 36、选择器的优先级是如何计算的？

- 参考资料

```
https://www.smashingmagazine.com/2007/07/css-specificity-things-you-should-know/

https://www.sitepoint.com/web-foundations/specificity/
```
### 37、重置（resetting）CSS 和 标准化（normalizing）CSS 的区别是什么？你会选择哪种方式，为什么？

重置（Resetting）： 重置意味着除去所有的浏览器默认样式。对于页面所有的元素，像margin、padding、font-size这些样式全部置成一样。你将必须重新定义各种元素的样式。

标准化（Normalizing）： 标准化没有去掉所有的默认样式，而是保留了有用的一部分，同时还纠正了一些常见错误

- 参考资料
```
https://stackoverflow.com/questions/6887336/what-is-the-difference-between-normalize-css-and-reset-css
```

### 38、万能居中

```css
margin:0 auto;  // 水平
text-align: center; // 水平
line-height: 10px;  // 垂直
表格，center, middle;   // 水平垂直
display: table-cell;   // 模拟表格
绝对定位，50%减自身宽高
绝对定位，上下左右全0，margin:auto;
绝对定位+相对定位，不需要知道宽高
```

### 39、栅格化的原理

antd的row和col，将一行等分为24份，col是几就占几份，底层按百分比实现，结合媒体查询，可以实现响应式。


### 40、 三角形

```css
.box {
    width: 0px;
    height: 0px;
    border: 50px solid transparent;
    border-bottom: 50px solid red;
}
```

### 41、 高度不一定，宽100%，内一p高不确定，如何实现垂直居中

* vertical-align: middle
* 绝对定位50%，translateY（-50%）
* 绝对定位，上下左右全0，margin:auto

### 42、 两种方式实现自适应

* rem, em
* 百分比
* 媒体查询
* bs，antd的栅格布局

### 43、 关于em

```html
 <p style="font-size: 20px">
      123
      <p style="font-size: 2em;width: 2em">456</p>
 </p>
// 此时子元素的font-size为40px, 宽度为80px(还要乘以子元素font-size的系数)
```

### 44、 关于vh.vw

* vw：viewpoint width，视窗宽度，1vw等于视窗宽度的1%
* vh：viewpoint height，视窗高度，1vh等于视窗高度的1%
* vmin：vw和vh中较小的那个
* vmax：vw和vh中较大的那个

### 45、 flex布局

* flex-direction    控制主副轴
* flex-wrap         控制换行（默认不换行）
* flex-flow         上面两个的结合
* justify-content   主轴对齐方式
* align-items       交叉轴对齐方式

### 46、设置一段文字的大小为6px

* 谷歌最小12px，其他浏览器可以更小
* transform：scale实现

### 47、css菊花图-四个小圆点一直旋转

```css
// 父标签
animation: antRotate 1.2s infinite linear;
// 子标签
animation: antSpin 1s infinite linear;
@keyframe antSpin {
    to {
        opacity: 1
    }
}
@keyframe antRotate {
    to {
        transform: rotate(405)
    }
}
// 逐个延迟0.4s
animation-delay: 0.4s;
```

### 48、overflow原理

overflow: hidden能清除块内子元素的浮动影响，因为该属性进行超出隐藏时需要计算盒子内所有元素的高度，所以会隐式清除浮动

创建BFC的条件

* float的值不为none
* overflow的值不为visible
* position的值为fixed、absolute
* display的值为table-cell、table-caption、inline-block、flex、inline-flex

### 49、实现自适应的正方形

* 使用vw。vh
* width百分比，height:0, padding-top(bottom): 50%

### 50、省略号

```css
{
    overflow: hidden;
    white-space: nowrap;
    text-overflow: ellipsis;
}
<!-- 宽度不固定，多行以及移动端显示 -->
{
    overflow: hidden;
    text-overflow: ellipsis;
    display: -webkit-box;
    -webkit-line-clamp: 3;
    -webkit-box-orient: vertical;
}
```

## 51、清除浮动css

```css
.clearfix:after {
    content: '';
    display: block;
    height: 0;
    clear: both;
    visibility: hidden;
}
.clearfix {
    zoom: 1;
}
```

### 51、垂直水平居中

```css
{
    position: absolute;
    top: 50%;
    left: 50%;
    margin-top: -10px;
    margin-left: -10px;
    width: 20px;
    height: 20px;
}
{
    position: absolute;
    left: 50%;
    top: 50%;
    transform: translate(-50%, -50%);
}
{
    display: flex;
    align-items: center;
    justify-content: center;
}
```

### 52、标准模式和怪异模式

document.compatMode属性可以判断是否是标准模式，当document.compatMode为“CSS1Compat”是标准模式，“CackCompat”是怪异模式。
怪异模式是为了兼容旧版本的浏览器，因为IE低版本document.documentElement.clientWidth获取不到
怪异模式盒模型：box-sizing: border-box;
标准模式盒模型：box-sizing: content-box;

### 53、CSS3实现环形进度条

两个对半矩形遮罩，使用rotate以及overflow: hidden进行旋转

### 54、移动端适配

<meta name="viewpoint" content="width=device-width, initial-scale=1.0">
rem,em,百分比
框架的栅格布局
media query媒体查询
flexible，自动判断dpr进行整个布局窗口的缩放


### 参考资料

```
https://www.imooc.com/article/290108
```
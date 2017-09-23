### WEB安全颜色
是指在256色计算机系统上总能避免抖动的颜色。

可以表示为RGB值为20%和51（十六进制值为33）的倍数，0%和0也是安全值。例如：rgb(40%，100%，0%)、rgb(0, 204, 102)。

采用十六进制表示时，00, 33, 66, 99, CC, FF 都是安全的。

### 选择器优先级

important > 内联 > ID > 类 > 标签 > 相邻选择器 > 子选择器 > 后代选择器> 通配符选择器 > 属性选择器 > 伪对象

### 长度单位

绝对长度单位

- 英寸in
- 厘米cm
- 毫米mm
- 点pt
- 派卡pc
- px

相对长度单位

- em

  1em定义为一种给定字体的 font-size 的值。如果一个元素的 font-size 的值为14px，那么相对该元素，1em就等于14px。
  在设置字体大小时，em的值会相对于父元素的字体大小改变。

- ex

  是指所用字体中小写 x 的高度。实际中大多数用户代理的做法是：取 em 的值的一半作为 ex 的值。

- rem

  根元素（html）的 font-size

css2单位：角度值、时间值、频率值




### 用em来设置字体大小
为了避免Internet Explorer 中无法调整文本的问题，许多开发者使用 em 单位代替像素。

em的尺寸单位由W3C建议。

1em和当前字体大小相等。在浏览器中默认的文字大小是16px。

因此，1em的默认大小是16px。可以通过下面这个公式将像素转换为em：px/16=em

### 链接
- a:link - 正常，未访问过的链接
- a:visited - 用户已访问过的链接
- a:hover - 当用户鼠标放在链接上时
- a:active - 链接被点击的那一刻
 
a:hover 必须跟在 a:link 和 a:visited后面

a:active 必须跟在 a:hover后面

### float(浮动)

- 行内框与浮动元素重叠时，其边框、背景、内容都在该浮动元素“之上”显示
- 块框与浮动元素重叠时，其边框、背景在该浮动元素“之下”显示，内容在该浮动元素“之上”显示

#### 清除浮动
- clear属性，应用于块级元素
- 父级div定义 overflow: auto
- :after


### positioning(定位)
- static

默认值，即没有定位，元素出现在正常的流中。

- relative

相对定位元素的定位是相对其正常位置。

- fixed

元素的位置相对于浏览器窗口是固定位置。

Fixed定位使元素的位置与文档流无关，因此不占据空间。

Fixed定位的元素和其他元素重叠。

- absolute

绝对定位的元素的位置相对于最近的已定位父元素，如果元素没有已定位的父元素，那么它的位置相对于

absolute 定位使元素的位置与文档流无关，因此不占据空间。

absolute 定位的元素和其他元素重叠。

### 布局 - 水平居中 & 垂直居中

> 在块级非替换元素在普通流内布局前提下，宽是包含块，是可以确定的。而高即使给了确定值，规范(css2)没给出既定布局算法让它能水平居中。

> 绝对定位导致与文档流无关时，无论垂直还是水平方向，margin有节余同时对应方向设置了auto,相应方向计算出的margin是分半的。


水平居中：对齐一个块级元素(如 div), 可以使用 margin: auto;

垂直居中：
- absolute 定位来实现的几种方法
```javascript
1. Absolute Centering
{
    position: absolute;
    top: 0; left: 0; bottom: 0; right: 0;
    margin: auto;
}

2. Negative Margins
{
    width: 300px;
    heigth: 200px;
    padding: 20px;
    position: absolute;
    top: 50%; left: 50%;
    margin-top: -120px;/* (height + padding)/2 */
    margin-left: -170px;/* (width + padding)/2 */
}

3. Transforms
{
    position: absolute;
    top: 50%; left: 50%;
    margin: auto;
    transform: translate(-50%,-50%);
}
```
- Flexbox

  {display: flex; justify-content: center; align-items: center;}
  
- Table-Cell

```
<div class="Center-Container is-Table">
  <div class="Table-Cell">
    <div class="Center-Block">
    <!-- CONTENT -->
    </div>
  </div>
</div>

.Center-Container.is-Table { display: table; }
.is-Table .Table-Cell {
  display: table-cell;
  vertical-align: middle;
}
.is-Table .Center-Block {
  width: 50%;
  margin: 0 auto;
}
```
- Inline-Block

```
<div class="Center-Container is-Inline">
  <div class="Center-Block">
    <!-- CONTENT -->
  </div>
</div>

.Center-Container.is-Inline { 
  text-align: center;
  overflow: auto;
}

.Center-Container.is-Inline:after,
.is-Inline .Center-Block {
  display: inline-block;
  vertical-align: middle;
}

.Center-Container.is-Inline:after {
  content: '';
  height: 100%;
  margin-left: -0.25em; /* To offset spacing. May vary by font */
}

.is-Inline .Center-Block {
  max-width: 99%; /* Prevents issues with long content causes the content block to be pushed to the top */
  /* max-width: calc(100% - 0.25em) /* Only for IE9+ */ 
}
```




[水平垂直居中的几种方法的比较](https://codepen.io/shshaw/full/gEiDt)

Technique | Browser Support | Responsive | Overflow | resize:both | Variable Height | Major Caveats
---|---|---|---|---|---|---
Absolute Centering | Modern & IE8+ | Yes | Scroll, can overflow container | Yes | Yes* | Variable Height not perfect cross-browser
Negative Margins | All | No | Scroll | Resizes but doesn't stay centered | No | Not responsive, margins must be calculated manually
Transforms | Modern & IE9+ | Yes | Scroll, can overflow container | Yes | Yes | Blurry rendering
Table-Cell | Modern & IE8+ | Yes | Expands container | No | Yes | Extra markup
Inline-Block | Modern, IE8+ & IE7* | Yes | Expands container | No | Yes | Requires container, hacky styles
Flexbox | Modern & IE10+ | Yes | Scroll, can overflow container | Yes | Yes | Requires container, vendor prefixes


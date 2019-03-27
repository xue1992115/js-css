### 文档流，什么是正常文档流，什么是脱离文档流？
### float，原理？设置float之后带来的影响？解决办法？对布局的影响？
### 块级格式化上下文
### position
### flex布局
### tabel-cell

## 正常文档流
正常流中，元素会基于文档模式，从左向右或者从上到下顺序排列的。

## 脱离正常文档流
采用css组织页面的结构布局，就是脱离了正常的文档流。

## float
浮动最初设计的目的是实现文字环绕。默认属性为none，可选属性是left,right.设置之后元素的line-boxes会遭到破坏，文字会重新形成新的line-boxes。
浮动带来的影响:
- 父级元素高度塌陷
- 浮动的元素具有包裹性，去空格化
- 对元素自身的影响是元素块级化，block
解决办法：
- clear方法
1. 这种是应用于父元素的
```ruby
.clearfix:after {
    content: '';
    display: table;
    clear: both;
}
```
- BFC方法 块级格式化上下文
BFC就是不管外边的元素怎么，都不会影响里边元素的布局。
1. 这种方法是让父级元素BFC
2. float(left, right), position(absolute, fixed脱离正常文档流的), overflow(hidden, scroll), display(tabel-cell, inline-block), 设置父元素的width,height;

## float与流体布局
- float实现三列布局（左中右）
- 单侧固定（左侧固定，右侧自适应）左侧浮动+width, 右侧margin-left/padding-left
- 同理右侧固定，左侧自适应(存在的问题是两个dom的上下顺序要互换)
1. 另一种解决办法是左侧(width: 100%, float: left, margin-left: 100px;),
右侧（float: left; width: 100px; margin-left: -200px;）
2. 这里自适应的原理是： 块级元素不设置宽度，自动填充。另一种是设置宽度100%;
- 智能自适应尺寸 float+ table-cell


## 块级格式化上下文
前面清除浮动使用的就是块级格式化上下文，除了上边的方法外，还有一种方法display: flow-root，这种方法不会像其他方法一样带来其他影响，只是创建一个新的块级格式化容器。该方法存在的问题是：对浏览器版本有要求。

## table-cell的用法
table-cell的原理就是让元素标签像td标签一样展示。可以垂直居中，水平居中，对于宽度高度敏感，对于padding敏感，但是对于margin不敏感。
应用的场景：等高布局，两栏自适应，垂直居中。

## position
- static,默认值是正常的文档流，对其他元素没有影响的。
- relative，正常文档流，相对于之前的位置移动，并且保持之前的位置不变，对其他元素没有影响。
- absolute，脱离文档流，相对于body，或者是static的祖先元素，之前的位置不保留，对其他元素有影响。
- fixed，脱离文档流，相对于视口，之前的位置会被移除
- sticky，元素在页面滚动的时候如同在正常文档流中，只有到相对于视口某个位置的时候就会固定。

## 弹性布局 display：flex
- container设置的属性包括：
1. display: flex
2. 方向，flex-driection: row|column|row-reverse|column-reverse
3. 主轴方向的位置：justify-content：flex-start | flex-end | center | space-between | space-around;
4. 次轴方向的位置：align-items： flex-start | flex-end | center | baseline | stretch;
- item设置的属性包括
1. order，数字越小越靠前
2. flex-grow，flex-shrink，flex-basis（初始大小，px %），flex-grow，flex-shrink允许我们在空间不足或者多余的情况下，扩大或者收缩大小。这两个属性可以数任意的正数，然后按照比例进行放大或者压缩。 也可以使用flex简写。




## 参照文章：
一篇全面的css布局指南：https://juejin.im/post/5b3b56a1e51d4519646204bb
table-cell的应用：https://www.zhangxinxu.com/wordpress/2010/10/%E6%88%91%E6%89%80%E7%9F%A5%E9%81%93%E7%9A%84%E5%87%A0%E7%A7%8Ddisplaytable-cell%E7%9A%84%E5%BA%94%E7%94%A8/
阮一峰flex布局：http://www.ruanyifeng.com/blog/2015/07/flex-grammar.html
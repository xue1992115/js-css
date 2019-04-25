# 安装
npm install less -g
# 使用
- lessc input.less output.css
# 变量
- 变量声明 @符号进行声明
```ruby
@nice-blue: #5B83AD;
@light-blue: @nice-blue + #111;
#header {
  color: @light-blue;
}
```
# mixin
```ruby
.bordered {
  border-top: dotted 1px black;
  border-bottom: solid 2px black;
}
#menu a {
  color: #111;
  .bordered;
}
.post a {
  color: red;
  .bordered;
}
```
# 嵌套
- 嵌套使用
```ruby
#header {
  color: black;
  .navigation {
    font-size: 12px;
  }
  .logo {
    width: 300px;
  }
}
```
编译后
```ruby
#header {
  color: black;
}
#header .navigation {
  font-size: 12px;
}
#header .logo {
  width: 300px;
}
```
- 提供&代表父类
```ruby
.clearfix {
  display: block;
  zoom: 1;

  &:after {
    content: " ";
    display: block;
    font-size: 0;
    height: 0;
    clear: both;
    visibility: hidden;
  }
}
```
编译后
```ruby
.clearfix {
  display: block;
  zoom: 1;
}
.clearfix:after {
  content: " ";
  display: block;
  font-size: 0;
  height: 0;
  clear: both;
  visibility: hidden;
}
```
注意：这里的变量类似于常量，因为只能声明一次。
# 媒体查询
```ruby
.screencolor{
  @media screen {
    color: green;
    @media (min-width:768px) {
    color: red;
    }
    }
  @media tv {
    color: black;
  }
}
```
编译后
```ruby
@media screen {
  .screencolor {
    color: green;
  }
}
@media screen and (min-width: 768px) {
  .screencolor {
    color: red;
  }
}
@media tv {
  .screencolor {
    color: black;
  }
}
```
# 运算
任何颜色，数值，变量都可以计算
```ruby
@base: 5%;
@filler: @base * 2;
@other: @base + @filler;

color: #888 / 4;
background-color: @base-color + #111;
height: 100% / 2 + @filler;
```
# 作用域
和编程语言的作用域比较相似，如果在局部知道不到就回去父级查找。
```ruby
@var: red;
#page {
  @var: white;
  #header {
    color: @var; // white
  }
}
```
这边的变量和混合声明不必提前声明，因为变量是延迟加载的，所以不必提前声明。具体详见延迟加载
# 注释
块级注释：显示在css文件中
行级注释：css文件中不显示

# @import导入
```ruby
@import "library"; // library.less
@import "typo.css";
```

# 变量插值
对变量进行管理，包括选择器名称，属性，url以及@import
```ruby
// 变量
@mySelector: banner;

// 用法
.@{mySelector} {
  font-weight: bold;
  line-height: 40px;
  margin: 0 auto;
}
```
编译后
```ruby
.banner {
  font-weight: bold;
  line-height: 40px;
  margin: 0 auto;
}
```
```ruby
// Variables
@images: "../img";

// Usage
body {
  color: #444;
  background: url("@{images}/white-sand.png");
}
```
编译后
```ruby
body {
  color: #444;
  background: url("../img/white-sand.png");
}

```
# 延迟加载
注意，变量时延迟加载的，所以不必提前声明，同时两次声明这个变量时，使用最后一次的声明。
```ruby
.lazy-eval {
  width: @var;
}

@var: @a;
@a: 9%;
```
编译后
```ruby
.lazy-eval {
  width: 9%;
}

```
```ruby
@var: 0;
.class {
  @var: 1;
  .brass {
    @var: 2;
    three: @var;
    @var: 3;
  }
  one: @var;
}
```
编译后
```ruby
.class {
  one: 1;
}
.class .brass {
  three: 3;
}
```
# 默认变量
```ruby
// library
@base-color: green;
@dark-color: darken(@base-color, 10%);

// use of library
@import "library.less";
@base-color: red;
```
很好的说明了延迟加载的，结果还是暗红色。
# 继承 extend
```ruby
nav ul {
  &:extend(.inline);
  background: blue;
}
.inline {
  color: red;
}
```
编译后
```ruby
nav ul {
  background: blue;
}
.inline,
nav ul {
  color: red;
}
```
# 混合
引用混合的方式，括号可以加，也可以不加
```ruby
.a, #b {
  color: red;
}
.mixin-class {
  .a();
}
.mixin-id {
  #b();
}
```
编译后
```ruby
.a, #b {
  color: red;
}
.mixin-class {
  color: red;
}
.mixin-id {
  color: red;
}
```
# 总结：
- 变量声明和使用@
- 嵌套
    - &引用父元素，实现伪类嵌套
- 注释
- 代码重用
    - 继承
    - 混合
    - @import
- 运算
- 作用域
- 函数
- 媒体查询
- 嵌套编译的原理
# less vs sass
- 声明变量的符号不一样
- 继承的思想不一样
- 混合使用的方式不一样
https://go.gliffy.com/go/publish/4784259
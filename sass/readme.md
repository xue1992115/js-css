# 安装
sass是用ruby语言写成的，所以要先安装ruby,MAC环境若已有安装则不必安装。
gem install sass
注意在mac中可能遇到权限问题，需要执行sudo gem install sass, sudo是以其他身份执行命令，预设的身份有root。

# 使用
- sass test.scss test.css
- sass --watch input.scss:output.css 监听一个文件自动编译。

# 编译后的风格
- nested：嵌套缩进的css代码，它是默认值。
- expanded：没有缩进的、扩展的css代码。
- compact：简洁格式的css代码。
- compressed：压缩后的css代码。
使用命令：sass --style compressed test.sass test.css

# 变量
- 变量的声明
1. $符号声明，可声明在块定义之外。
2. 声明同一个变量，后边的会覆盖前面的
- 变量的使用
1. $符号引用
- 中下划线，下划线可以兼容使用 

# 嵌套
- 嵌套使用
```ruby
#content {
  article {
    h1 { color: #333 }
    p { margin-bottom: 1.4em }
  }
    aside { background-color: #EEE }
}
```
编译后
```ruby
#content article h1 { color: #333 }
#content article p { margin-bottom: 1.4em }
#content aside { background-color: #EEE }
```
- 嵌套的副作用
不适用于添加类似伪类样式，:hover,为解决问题sass提供了&, 代表的是最外层的选择器。原因是在嵌套规则中，是通过空格将父选择器和子代选择器链接，而伪类选择器不希望通过这种方式。
- 群组选择器嵌套
```ruby
sass样式
.container {
  h1, h2, h3 {margin-bottom: .8em}
}
nav, aside {
  a {color: blue}
}
```
编译后
```buby
css样式
.container h1, .container h2, .container h3 { margin-bottom: .8em }
nav a, aside a {
  color: blue; }
```
原理是sass会分别将container和h1，container和h2， container和h3分别组合，然后在一起组合。nav和a，aside和a分别组合，然后在一起组合。
- 其他子组合选择器和同层选择器
1. ~
```ruby
article {
  ~ article { border-top: 1px dashed #ccc }
  > section { background: #eee }  // 紧跟article的元素
  dl > { 
    dt { color: #333 }
    dd { color: #555 }
  }
  nav + & { margin-top: 0 } 
}
```
编译后
```ruby
article ~ article {
  border-top: 1px dashed #ccc; }
article > section {
  background: #eee; }
article dl > dt {
  color: #333; }
article dl > dd {
  color: #555; }
nav + article {
  margin-top: 0; }
```
- 嵌套属性
适用于border-style。 border-width......这样的属性
```ruby
nav {
  border: 1px solid #ccc {
  left: 0px;
  right: 0px;
  }
}
```
编译后
```ruby
nav {
  border: 1px solid #ccc;
  border-left: 0px;
  border-right: 0px;
}
```

# 导入sass文件
当文件过大时，可以把文件进行拆分导入的方式。
css提供了@import的规则，缺点是只有执行到@import时才开始下载其他的css文件，速度较慢。
sass也提供了@import的规则，但是会提前导入文件中，这就意味着所有的样式文件被放在同一个css文件中，无需发起额外的请求。
- 使用sass部分文件
对于导入的文件，无需编译时，局部文件命名方式以下划线开头，如themes/_night-sky.scss， 导入时@import "themes/night-sky";

# 静默注释
- // 注释方法
sass提供给的注释方法//，编译之后不会出现在css文件中。
兼容css提供的 /**/，会出现在css文件中。

# 混合器
- @mixin： 主要是实现大段样式的复用
- @include 引用mixin中的样式
```ruby
@mixin rounded-corners {
  -moz-border-radius: 5px;
  -webkit-border-radius: 5px;
  border-radius: 5px;
}
notice {
  background-color: green;
  border: 2px solid #00aa00;
  @include rounded-corners;
}
```
编译后
```ruby
notice {
  background-color: green;
  border: 2px solid #00aa00;
  -moz-border-radius: 5px;
  -webkit-border-radius: 5px;
  border-radius: 5px; }
```
- 优点：可在不同地方重复使用样式
- 缺点：造成样式表过大
- 使用场景：经验法则，能够方便的起名字
- 给混合器传递参数，可以通过$name: value的形式传递参数，无关顺序，保证不遗漏。否则报错，Mixin link-colors is missing argument $hover。
```ruby
@mixin link-colors($normal, $hover, $visited) {
  color: $normal;
  &:hover { color: $hover; }
  &:visited { color: $visited; }
}
a {
    @include link-colors(
      $normal: blue,
      $visited: green,
      $hover: red
  );
}
```
- 指定默认参数
```ruby
@mixin link-colors(
    $normal,
    $hover: $normal,
    $visited: $normal
  )
{
  color: $normal;
  &:hover { color: $hover; }
  &:visited { color: $visited; }
}
a {
    @include link-colors(
      $normal: blue,
  );
}
```
编译后
```ruby
a {
  color: blue; }
  a:hover {
    color: blue; }
  a:visited {
    color: blue; }
```

# 继承属性精简css
```ruby
//通过选择器继承继承样式
.error {
  border: 1px solid red;
  background-color: #fdd;
}
.seriousError {
  @extend .error;
  border-width: 3px;
}
```
- 另外不仅是继承error本身的样式，任何和error组合的样式，也会被seriousError一组合形式继承下来。
```ruby
//.seriousError从.error继承样式
.error a{  //应用到.seriousError a
  color: red;
  font-weight: 100;
}
h1.error { //应用到hl.seriousError
  font-size: 1.2rem;
}
.seriousError {
  @extend .error;
  border-width: 3px;
}
```
编译后
```ruby
.error a, .seriousError a {
  color: red;
  font-weight: 100; }

h1.error, h1.seriousError {
  font-size: 1.2rem; }

.seriousError {
  border-width: 3px; }
```
- 继承的高级用法
可以继承所有的css样式
```ruby
.disabled {
  color: gray;
  @extend a;
}
```
- 继承的实质
@extend背后最基本的想法是，如果.seriousError @extend .error， 那么样式表中的任何一处.error都用.error.seriousError这一选择器组进行替换。
- @extends要点
1. 跟混合器相比，继承生成的css代码相对更少。因为继承仅仅是重复选择器，而不会重复属性，所以使用继承往往比混合器生成的css体积更小。
2. 继承遵从css层叠的规则。根据权重大小，权重一样的在后边的起作用。

# sass.scssc
作用是缓存相关的东西，预处理器编译一次就会生成一次。

# 小结
- 变量定义和使用 $
- 嵌套
    - 通过&引用父元素，实现类似伪类的嵌套
    - 群组选择器的嵌套
    - 其他子类后代选择器嵌套
    - 属性嵌套
- 注释
- 代码重用
    - 继承（包括继承样式的组合样式）
    - 继承的思想
    - @mixin 重用代码块 @include
    - @import导入外部的文件

- 参考文献
sass官方文档：https://www.sass.hk/guide/ 

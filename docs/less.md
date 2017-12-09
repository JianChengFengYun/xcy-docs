### 一.变量 Overview

```css
    @grey:#9a9a9a;
    .navLink-news {
      text-align: left;
      color: @grey;
    }
```

变量也可以用在其他地方，如选择器名称，属性名称，URL和@ import语句。

**1. Selectors 变量命名选择器**

注意：(~"@{name}") 语句可以在 LESS 1.3.1 等之前版本中使用，但 1.4.0 版将不再支持这种用法。

```css
    @my-selector: banner;// Variables
    .@{my-selector}{ // Usage
      font-weight: bold;
      line-height: 40px;
      margin: 0 auto;
    }
```

**2. URLs**

```css
    @images: "../img";
    body {
      color: #444;
      background: url("@{images}/white-sand.png");
    }
```

**3. Import Statements**

 Version: 1.4.0<br>
 
 ```css
Syntax: @import "@{themes}/tidal-wave.less";

@themes: "../css";
@import "@{themes}/2.less";
@import "3.less";

```


**4. Properties**

Version: 1.6.0

```css
@property: color;
.widget {
    @{property}: #0ee;
  background-@{property}: #999;
}
```

**5. Variable Names**

用变量名定义变量

```css
@fnord:  "I am fnord.";
@varr:    "fnord";
.h1::after{
  content: @@varr;
}
```

如果对同一个变量定义两次的话，在当前作用域中最后一次定义的将被使用。
这与CSS的机制类似，最后一次定义的值会成为这个属性的值。

```css
@var: 0;
.class1 {
  @var: 1;
  .class {
    @var: 2;
    three: @var;
    @var: 3;
  }
  one: @var;
}
```

变量是“按需加载”（lazy loaded）的，因此不必强制在使用之前声明。

```css
.lazy-eval {
  width: @var;
}
@var: @a;
@a: 9%;

.lazy-eval-scope {
  width: @var;
  @a: 9%;
}
@var: @a;
@a: 100%;

@base-color: green;
@dark-color: darken(@base-color, 3%);
@base-color: red;
.color{
  color:@dark-color;
}
```




### 二. 混合 :

在 LESS 中我们可以定义一些通用的属性集为一个 class，然后在另一个 class 中去调用这些属性，下面有这样一个 class

```css
.a, #b {
  color: red;
}
```
1. (Notice that when you call the mixin, the parentheses are optional.)
括号是可选的

```css
.mixin-class {
  .a();
}
.mixin-id {
  #b;
}
```
2. (Not Outputting the Mixin)
如果你想创建一个mixin，但你不想编译出来，你可以在类名称后面加上括号

```css
.my-mixin {
  color: black;
}
.my-other-mixin() {
  background: white;
}
.class {
  .my-mixin;
  .my-other-mixin;
}
```

3. (Selectors in Mixins)
混合组件可以包含的不仅仅是属性，也可以包含选择器。

```css
.my-hover-mixin() {
  &:hover {
    border: 1px solid red;
  }
}
button {
  .my-hover-mixin();
}
```

4. (Namespaces)
更复杂的混合属性选择器

命名空间：你可以放自己的 mixins 在id选择器下，这保证它不会与另一个库的冲突。

```css
#outer() {
  .inner {
    color: red;
  }
}

>符号和空格是可选的，以下6种相同

.c1 {
  #outer > .inner;
}
.c2 {
  #outer > .inner();
}
.c3 {
  #outer .inner;
}
.c4 {
  #outer .inner();
}
.c5 {
  #outer.inner;
}
.c6 {
  #outer.inner();
}

#my-library {
  .my-mixin() {
    color: black;
  }
  .inner() {
    color: green;
  }
}

 which can be used like this
 
.class1 {
  #my-library > .my-mixin();
}
.class2 {
  #my-library > .inner();
}
```

**5. (Guarded Namespaces)条件命名空间**

```css
#namespace when (@mode=huge) {
  .mixin() {  }
}
#namespace {
  .mixin() when (@mode=huge) {  }
}

//1
#namespace when (@mode=1) { 

//@mode必须定义在全局
  .mixin() {
    color:#e4393c;
  }
}
@mode:1;
.name1{
  #namespace .mixin;
}

//2  (可用于H5 rem)
#namespace2 {

  .mixin1() when (@fs=18) {
    font-size: 18px;
  }
  .mixin1() when (@fs=20) {
    font-size: 20px;
  }
}

.name2{
  @fs:18;
  #namespace2 .mixin1;
}
.name3{
  @fs:20;
  #namespace2 .mixin1;
}

//3 The default function is assumed to have the same value for all nested namespaces and mixin. Following mixin is never evaluated, one of its guards is guaranteed to be false:

#sp_1 when (default()) {
  #sp_2 when (default()) {
    .mixin() when not(default()) { !* *! }
  }
}

```

6. (The “!important” keyword)

```css
.foo (@bg: #f5f5f5, @color: #900) {
  background: @bg;
  color: @color;
}
.unimportant {
  .foo();
}
.important {
  .foo() !important;
}
```


### 三. Parametric Mixins
   带参数混合

```css
.border-radius(@radius) {
  -webkit-border-radius: @radius;
  -moz-border-radius: @radius;
  border-radius: @radius;
}
#header {
  .border-radius(4px);
}
.button {
  .border-radius(6px);
}
```

Parametric mixins can also have default values for their parameters,
参数混合也可以有自己的参数的默认值:

```css
.border-radius(@radius: 5px) {
  -webkit-border-radius: @radius;
  -moz-border-radius: @radius;
  border-radius: @radius;
}
#header {
  .border-radius;
}
```

多个参数可以使用分号或者逗号分隔，推荐使用分号分隔，因为逗号有两重含义：它既可以表示混合的参数，也可以表示一个参数中一组值的分隔符。

使用分号作为参数分隔符意味着可以将逗号分隔的一组值作为一个变量处理。换句话说，如果编译器在混合的定义或者是调用中找到至少一个分号，就会假设参数是使用分号分隔的，所有的逗号都属于参数中的一组值的分隔符。

2个参数，每个参数都含有通过逗号分隔的一组值的情况：.name(1, 2, 3; something, else)。

3个参数，每个参数只含一个数字的情况：.name(1, 2, 3)。

 使用一个象征性的分号可以创建一个只含一个参数，但参数包含一组值的混合：.name(1, 2, 3;)。

逗号分隔的一组值参数的默认值：.name(@param1: red, blue;)。

使用同样的名字和同样数量的参数定义多个混合是合法的。在被调用时，LESS会应用到所有可以应用的混合上。比如你调用混合时只传了一个参数.mixin(green)，那么所有 只强制要求一个参数的混合 都会被调用：
```css
.mixin(@color) {
  color-1: @color;
}
.mixin(@color; @padding:2) {
  color-2: @color;
  padding-2: @padding;
}
.mixin(@color; @padding; @margin: 2) {
  color-3: @color;
  padding-3: @padding;
  margin: @margin @margin @margin @margin;
}
.some .selector div {
  .mixin(#008000);
}
```

**@arguments 变量**

@arguments包含了所有传递进来的参数。 如果你不想单独处理每一个参数的话就可以像这样写：

```css
.box-shadow (@x: 0, @y: 0, @blur: 1px, @color: #000) {
  box-shadow: @arguments;
  -moz-box-shadow: @arguments;
  -webkit-box-shadow: @arguments;
}
.b1{
  .box-shadow(2px, 5px);
}
```

参数根据它的名称引用，所以参数的位置可以换

```css
.mixin5(@color: black; @margin: 10px; @padding: 20px) {
  color: @color;
  margin: @margin;
  padding: @padding;
}
.class1 {
  .mixin5(@margin: 20px; @color: #33acfe);
}
.class2 {
  .mixin5(#efca44; @padding: 40px);
}
```

**高级参数用法与 @rest 变量**

如果需要在 mixin中不限制参数的数量，可以在变量名后添加 ...，表示这里可以使用 N 个参数。


- .mixin (...) {        // 接受 0-N 个参数
- .mixin () {           // 不接受任何参数
- .mixin (@a: 1) {      // 接受 0-1 个参数
- .mixin (@a: 1, ...) { // 接受 0-N 个参数
- .mixin (@a, ...) {    // 接受 1-N 个参数


此外：

```css

    .mixin (@a, @rest...) {
    // @rest 表示 @a 之后的参数
    // @arguments 表示所有参数
    }
    
    //例如
    .background (...) {
      background: @arguments;
    }
    .box{
      .background(#fff, url(../images/icon_heads.png), no-repeat);
    }
```


### 四. Pattern-matching
模式匹配


1. LESS 提供了通过参数值控制 mixin 行为的功能，让我们先从最简单的例子开始：

如果要根据 @switch 的值控制 .mixin 行为，只需按照下面的方法定义 .mixin：

```css
.mixin2 (dark, @color) {
  color: darken(@color, 10%);
}
.mixin2 (light, @color) {
  color: lighten(@color, 10%);
}
.mixin2 (@_, @color) {
  display: block;
}
.class {
  @switch: light;
  .mixin2(@switch, #888);
}
```

以上传给 .mixin 的颜色将执行 lighten 函数，如果 @switch 的值是 dark，那么则会执行 darken 函数输出颜色。

整个过程如何发生的：

- 第一个 .mixin 没有匹配，因为不满足 dark 条件；
- 第二个 .mixin 可以被匹配，因为它满足了 light 条件；
- 第三个 .mixin 也可以被匹配，因为它接受任何参数。

只有满足匹配要求的混合才会被使用。混合中的变量可以匹配任何值，非变量形式的值只有与传入的值完全相等时才可以匹配成功。


我们也可以根据参数的数量进行匹配，比如：

```css
.mixin3 (@a) {
color: @a;
}
.mixin3 (@a, @b) {
color: fade(@a, @b);
}
//调用 .mixin 时，如果使用了一个参数，输出第一个 .mixin，使用了两个参数，则输出第二个。*/
.pm1{
 .mixin3(#f00);
}
.pm1{
 .mixin3(#0f0,50%);
}
```


### 五. Guards表达式

与上面匹配值或者匹配参数数量的情况不同，Guards 被用来匹配表达式 (expressions)。
为了尽可能地符合 CSS 的语言结构，LESS 选择使用 guard混合（guarded mixins）
（类似于 @media 的工作方式）执行条件判断，而不是加入 if/else 声明。

首先通过下面的例子开始介绍：

```css
.mixin4 (@a) when (lightness(@a) >= 50%) {
  background-color: black;
}
.mixin4 (@a) when (lightness(@a) < 50%) {
  background-color: white;
}
.mixin4 (@a) {
  color: @a;
}
```

要点在于关键词 when，它引入了一个 guard 条件 （这里只用到一个 guard）。

现在如果运行下面的代码：

```css
.class1 { .mixin4(#ddd) }
.class2 { .mixin4(#555) }
```

Guards 支持的运算符包括：> >= = =< <。说明一下，true关键字是唯一被判断为真的值，也就是这两个混合是相等的：

```css
.truth (@a) when (@a) {  }
.truth (@a) when (@a = true) {  }

//其他不为 true 的值都判为假：
.class {
  //.mixin4(40); // 不会匹配上面的 mixin
}
```

多个Guards可以通过逗号表示分隔，如果其中任意一个结果为 true，则匹配成功：
.mixin (@a) when (@a > 10), (@a < -10) {  }

值得注意的是不同的参数之间也可以比较，而参与比较的也可以一个参数都没有：

```css
@medi:mobile;
.mixin (@a) when (@medi = mobile) {  }
.mixin (@a) when (@medi = desktop) {  }
.max (@a, @b) when (@a > @b) { width: @a }
.max (@a, @b) when (@a < @b) { width: @b }
```

如果需要根据值的类型匹配混合，可以使用 is* 函数：

```css
.mixin (@a, @b: 0) when (isnumber(@b)) {  }
.mixin (@a, @b: black) when (iscolor(@b)) {  }
```


几个检查基本类型的函数：

- iscolor
- isnumber
- isstring
- iskeyword
- isurl

如果需要检查一个值（数字）使用了哪个单位，可以使用下面三个函数：
- ispixel
- ispercentage
- isem

最后，你可以使用关键词 and 在 guard 中加入额外的条件:
```css
.mixin (@a) when (isnumber(@a)) and (@a > 0) { ... }

//或者，使用关键词 not 否定条件：
.mixin (@b) when not (@b > 0) { ... }
```

### 六. Merge 合并

Comma 用逗号附加属性值 “+”


    .mixin() {
      box-shadow+: inset 0 0 10px #555;
    }
    .myclass {
      .mixin();
      box-shadow+: 0 0 20px black; //合并后逗号隔开；
    }
    
    //Space 用空格附加属性值“+_”
    .mixino() {
      transform+_: scale(2);
    }
    .myclasso {
      .mixino();
      transform+_: rotate(15deg);
    }


### 七. Parent Selectors "&"
- 用法1：伪类
```css
    a {
      color: blue;
      &:hover {
        color: green;
      }
    }
```
- 用法2：悬停规则

``` css
.button {
  &-ok {
    background-image: url("ok.png");
  }
  &-cancel {
    background-image: url("cancel.png");
  }

  &-custom {
    background-image: url("custom.png");
  }
}
```

&在一个选择器可能会出现超过一次。可以重复引用父选择器 而不用重复其名称。
```css
.link {
  & + & {
    color: red;
  }

  & & {
    color: green;
  }

  && {
    color: blue;
  }

  &, &ish {
    color: cyan;
  }
}
```
需要注意的是 & 代表所有的祖先选择器 （不只是最近的那个），下面的例子：用的时候要注意；

```css
.grand {
  .parent {
    & > & {
      color: red;
    }

    & & {
      color: green;
    }

    && {
      color: blue;
    }

    &, &ish {
      color: cyan;
    }
  }
}
```

### 八. 嵌套规则

LESS 可以让我们以嵌套的方式编写层叠样式。 让我们先看下下面这段 CSS：



```
//LESS 可以让我们以嵌套的方式编写层叠样式。 让我们先看下下面这段 CSS：
#header {
  color: black;
  .navigation {
    font-size: 12px;
  }
  .logo {
    width: 300px;
    &:hover { text-decoration: none }
  }
}
```


嵌套 Media Queries
Media query也可以使用同样的方式进行嵌套。

```css
.screen-color{
  @media screen {
    color: green;
    @media (min-width: 768px) {
      color: red;
      .s1{
        color:white;
      }
    }
  }
  @media tv {
    color: black;
  }
}
```


### 9. Operations 运算

任何数字、颜色或者变量都可以参与运算。来看一组例子：

```css
//转换成相同的单位
@conversion-1: 5cm + 10mm; // result is 6cm
@conversion-2: 2 - 3cm - 5mm; // result is -1.5cm
@conversion-3: 5cm * 10mm; // 乘除（* /）转不了
@incompatible-units: 2 + 5px - 3cm; // result is 4px  转不了

/*LESS 会使用出现的单位，最终输出 6px。
也可以使用括号：*/
@conversion-4: 1px + 5;
@width1: ((@conversion-4 + 5) * 2);

.op1{
  width:@conversion-2;
}
.op2{
  width:@conversion-3;
}
.op3{
  width:@conversion-4;
  width:@width1;
}


//变量的运算
@base: 5%;
@filler: (@base * 2);
@other: (@base + @filler);

//颜色的运算
@color: (#888 / 4);
@background-color: (@color + #111);
@height: (100% / 2 + @filler);

```

### 十、函数

LESS 提供了多种函数用于控制颜色变化、处理字符串、算术运算等等。这些函数在函数手册中有详细介绍。

```css
@base: #f04615;
@width: 0.5;
.class {
  width: percentage(0.5);  // 将 0.5 转换为 50%
  color: saturate(@base, 5%); //将颜色饱和度增加 5%
  background-color: spin(lighten(@base, 25%), 8); //颜色亮度降低 25% 色相值增加 8
}
```

### 十一、导入（Import）

在LESS中，你既可以导入CSS文件，也可以导入LESS文件。但只有导入的LESS文件才会被处理（编译），导入的CSS文件会保持原样。

如果你希望导入一个CSS文件，保留.css后缀即可：

    @import "lib.css";

编译过程中，对导入CSS文件只做一次处理：将导入的语句提到最前，紧跟在@charset之后。


```
//例如输入的文件有导入语句：

h1 { color: green; }
@import "import/official-branding.css?urlParameter=23";

//导入语句将被提到最前：
@import "import/official-branding.css?urlParameter=23";
h1 { color: green; }
```


**被导入的LESS文件会被复制到含导入语句的文件中，然后一起编译。导入和被导入的文件共享所有的混合、命名空间、变量以及其它结构。**

另外，如果导入语句是通过media query指定的，那么导入的语句编译之后会被包裹在@Media声明中。

例如有被导入的文件library.less：

    @imported-color: red;
    h1 { color: green; }

主样式文件导入了上面的library.less：


```css
@import "library.less" screen and (max-width: 400px); // 通过media query指定的import
@import "library.less"; // 无media query的import
.class {
  color: @importedColor; // 使用导入的变量
}
```


将会编译出：


```css
// 对应通过media query指定的import
@media screen and (max-width: 400px) {
  h1 { color: green; }
}
// 对应无media query的import
h1 { color: green; }
.class {
  // 使用导入的变量
  color: #ff0000;
}
```


LESS文件的导入语句并不强制要求在顶部，它可以被入在规则内部、混合中或者其它的结构中。


例如放在规则内部：

    pre {
      @import "library.less";
      color: @importedColor;
    }


在library.less中定义的变量和规则都会被投篮到pre的规则中：


```css
pre {
  color: #ff0000; //定义在library.less中的变量可用
}
pre h1 { 
//定义在library.less中的样式规则被嵌套到pre中
  color: green;
}
```


在v1.3.0 - v1.3.3中，@import允许多次导入同一个文件，可以使用@import-once让同一文件只导入一次。

在v1.4.0中，移除了@import-once，@import的行为就是同一文件只导入一次了。这意味着，如果代码是这样的：

    @import “file.less”; @import “file.less”;
    
那么第二个文件将被忽略。

任何不以.css结尾的文件都被认为是less文件，将被处理。

另外，如果文件名没有后缀，LESS会自动在结尾加上.less。下面两种写法是等价的：

    @import "lib.less";
    @import "lib";

在v1.4.0中，你可以强制某个文件使用特写的方式来处理，比如：

    @import (less) "lib.css";
    
会将lib.css引入，并当作LESS文件处理。如果你指定了某个文件是less但是没有包含后缀名，LESS将不会自动添加后缀。



### 十二. Escaping 避免编译

有时候我们需要使用一些 LESS不认识的专有语法。
要输出这样的值我们可以在字符串前加上一个 ~，例如：

    .class {
      filter: ~"ms:alwaysHasItsOwnSyntax.For.Stuff()";
    }
    
在避免编译的值中间也可以像字符串一样插入变量：

    .class {
      @what: "Stuff";
      filter: ~"ms:alwaysHasItsOwnSyntax.For.@{what}()";
    }

# Mixins

- 添加进已有的样式里

```less
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

```css
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

## Mixins With Parentheses

```less
.my-mixin {
  color: black;
}
// 不会添加多一个className
.my-other-mixin() {
  background: white;
}
.class {
  .my-mixin();
  .my-other-mixin();
}
```

```css
.my-mixin {
  color: black;
}
.class {
  color: black;
  background: white;
}
```

## Selectors in Mixins

```less
.my-hover-mixin() {
  &:hover {
    border: 1px solid red;
  }
}
button {
  .my-hover-mixin();
}
```

```css
button:hover {
  border: 1px solid red;
}
```

## Namespaces

```less
#outer() {
  .inner {
    color: red;
  }
}

.c {
  #outer.inner();
}
```

```css
.c {
  color: red;
}
```

## Guarded Namespaces

- 如果名称空间具有保护，则仅当保护条件返回true时，才使用由其定义的混合。命名空间保护的计算方式与mixin上的保护完全相同，因此以下两个mixin的工作方式相同：

```less
#namespace when (@mode = huge) {
  .mixin() { /* */ }
}

#namespace {
  .mixin() when (@mode = huge) { /* */ }
}
```

## The `!important` keyword

```less
.foo (@bg: #f5f5f5; @color: #900) {
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

```css
.unimportant {
  background: #f5f5f5;
  color: #900;
}
.important {
  background: #f5f5f5 !important;
  color: #900 !important;
}
```

## Parametric Mixins

```less
// .border-radius(@radius) { 这种没有没有默认值
.border-radius(@radius: 5px) {
  -webkit-border-radius: @radius;
     -moz-border-radius: @radius;
          border-radius: @radius;
}

#header {
  .border-radius();
}

```

```css
#header {
  -webkit-border-radius: 5px;
  -moz-border-radius: 5px;
  border-radius: 5px;
}
```

## Overloading mixins

```less
.mixin(@color) {
  color-1: @color;
}
.mixin(@color, @padding: 2) {
  color-2: @color;
  padding-2: @padding;
}
.mixin(@color, @padding, @margin: 2) {
  color-3: @color;
  padding-3: @padding;
  margin: @margin @margin @margin @margin;
}
.some .selector div {
  .mixin(#008000);
}
```

```css
.some .selector div {
  color-1: #008000;
  color-2: #008000;
  padding-2: 2;
}
```

## Named Parameters

```less
.mixin(@color: black; @margin: 10px; @padding: 20px) {
  color: @color;
  margin: @margin;
  padding: @padding;
}
.class1 {
  .mixin(@margin: 20px; @color: #33acfe);
}
.class2 {
  .mixin(#efca44; @padding: 40px);
}
```

```css
.class1 {
  color: #33acfe;
  margin: 20px;
  padding: 20px;
}
.class2 {
  color: #efca44;
  margin: 10px;
  padding: 40px;
}
```

## The `@arguments` Variable

```less
.box-shadow(@x: 0, @y: 0, @blur: 1px, @color: #000) {
  -webkit-box-shadow: @arguments;
     -moz-box-shadow: @arguments;
          box-shadow: @arguments;
}
.big-block {
  .box-shadow(2px, 5px);
}
```

```css
.big-block {
  -webkit-box-shadow: 2px 5px 1px #000;
     -moz-box-shadow: 2px 5px 1px #000;
          box-shadow: 2px 5px 1px #000;
}
```

## Advanced Arguments and the `@rest` Variable

```less
.mixin(@a, @rest...) {
   // @rest is bound to arguments after @a
   // @arguments is bound to all arguments
}
```

## Pattern-matching

```less
.mixin(@s, @color) { ... }

.class {
  .mixin(@switch, #888);
}
```

```less
.mixin(dark, @color) {
  color: darken(@color, 10%);
}
.mixin(light, @color) {
  color: lighten(@color, 10%);
}
.mixin(@_, @color) {
  display: block;
}

@switch: light;

.class {
  .mixin(@switch, #888);
}
```

```css
.class {
  color: #a2a2a2;
  display: block;
}
```

## Using Mixins as Functions

### Property / value accessors

```less
.average(@x, @y) {
  @result: ((@x + @y) / 2);
}

div {
  // call a mixin and look up its "@result" value
  padding: .average(16px, 50px)[@result];
}
```

```css
div {
  padding: 33px;
}
```

### Overriding mixin values

```less
// library.less
#library() {
  .mixin() {
    prop: foo;
  }
}

// customize.less
@import "library";
#library() {
  .mixin() {
    prop: bar;
  }
}

.box {
  my-value: #library.mixin[prop];
}

// Outputs:

.box {
  my-value: bar;
}
```

### Unnamed lookups

```less
.average(@x, @y) {
  @result: ((@x + @y) / 2);
}

div {
  // call a mixin and look up its final value
  padding: .average(16px, 50px)[];
}


//  output :

div {
  padding: 33px;
}
```

### Unlocking mixins & variables into caller scope

```less
.mixin() {
  @width:  100%;
  @height: 200px;
}

.caller {
  .mixin();
  width:  @width;
  height: @height;
}

// Results :

.caller {
  width:  100%;
  height: 200px;
}
```

```less
.unlock(@value) { // outer mixin
  .doSomething() { // nested mixin
    declaration: @value;
  }
}

#namespace {
  .unlock(5); // unlock doSomething mixin
  .doSomething(); //nested mixin was copied here and is usable
}

// Results:

#namespace {
  declaration: 5;
}
```

## Recursive Mixins

```less
.loop(@counter) when (@counter > 0) {
  .loop((@counter - 1));    // next iteration
  width: (10px * @counter); // code for each iteration
}

div {
  .loop(5); // launch the loop
}

// Output:

div {
  width: 10px;
  width: 20px;
  width: 30px;
  width: 40px;
  width: 50px;
}
```

```less
.generate-columns(4);

.generate-columns(@n, @i: 1) when (@i =< @n) {
  .column-@{i} {
    width: (@i * 100% / @n);
  }
  .generate-columns(@n, (@i + 1));
}
// Output:

.column-1 {
  width: 25%;
}
.column-2 {
  width: 50%;
}
.column-3 {
  width: 75%;
}
.column-4 {
  width: 100%;
}
```

## Mixin Guards

```less
.mixin(@a) when (lightness(@a) >= 50%) {
  background-color: black;
}
.mixin(@a) when (lightness(@a) < 50%) {
  background-color: white;
}
.mixin(@a) {
  color: @a;
}

.class1 { .mixin(#ddd) }
.class2 { .mixin(#555) }

// output:

.class1 {
  background-color: black;
  color: #ddd;
}
.class2 {
  background-color: white;
  color: #555;
}

```

### Guard Comparison Operators

```less
.truth(@a) when (@a) { ... }
.truth(@a) when (@a = true) { ... }

@media: mobile;

.mixin(@a) when (@media = mobile) { ... }
.mixin(@a) when (@media = desktop) { ... }

.max(@a; @b) when (@a > @b) { width: @a }
.max(@a; @b) when (@a < @b) { width: @b }
```

### Guard Logical Operators

```less
.mixin(@a) when (isnumber(@a)) and (@a > 0) { ... }

.mixin(@a) when (@a > 10), (@a < -10) { ... }

.mixin(@b) when not (@b > 0) { ... }
```

### Type Checking Functions

- iscolor
- isnumber
- isstring
- iskeyword
- isurl
- ispixel
- ispercentage
- isem
- isunit

```less
.mixin(@a; @b: 0) when (isnumber(@b)) { ... }
.mixin(@a; @b: black) when (iscolor(@b)) { ... }
```

## Aliasing Mixins

```less
#theme.dark.navbar {
  .colors(light) {
    primary: purple;
  }
  .colors(dark) {
    primary: black;
    secondary: grey;
  }
}

.navbar {
  @colors: #theme.dark.navbar.colors(dark);
  background: @colors[primary];
  border: 1px solid @colors[secondary];
}

// output:

.navbar {
  background: black;
  border: 1px solid grey;
}
```

### Variable calls

```less
#library() {
  .colors() {
    background: green;
  }
}
.box {
  @alias: #library.colors();
  @alias();
}
// Outputs:

.box {
  background: green;
}
```

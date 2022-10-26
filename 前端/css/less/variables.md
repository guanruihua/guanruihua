# Variables

- 变量

## Overview

```less
// Variables
@link-color:        #428bca; // sea blue
@link-color-hover:  darken(@link-color, 10%);

// Usage
a,
.link {
  color: @link-color;
}
a:hover {
  color: @link-color-hover;
}
.widget {
  color: #fff;
  background: @link-color;
}
```

```css
a,
.link {
  color: #428bca;
}
.widget {
  color: #fff;
  background: #428bca;
}
```

## Selectors

```less
// Variables
@my-selector: banner;

// Usage
.@{my-selector} {
  font-weight: bold;
  line-height: 40px;
  margin: 0 auto;
}
```

```css

.banner {
  font-weight: bold;
  line-height: 40px;
  margin: 0 auto;
}
```

## URLS

```less
// Variables
@images: "../img";

// Usage
body {
  color: #444;
  background: url("@{images}/white-sand.png");
}
```

## Import Statements

```less
// Variables
@themes: "../../src/themes";

// Usage
@import "@{themes}/tidal-wave.less";
```

## Properties

```less

@property: color;

.widget {
  @{property}: #0ee;
  background-@{property}: #999;
}
```

```css
.widget {
  color: #0ee;
  background-color: #999;
}
```

## Variable Variables

```less
@primary:  green;
@secondary: blue;

.section {
  @color: primary;

  .element {
    color: @@color;
  }
}
```

```css

.section .element {
  color: green;
}
```

## Lazy Evaluation

```less
.lazy-eval {
  width: @var;
  @a: 9%;
}

@var: @a;
@a: 100%;
```

```css
.lazy-eval {
  width: 9%;
}
```

```less
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

```css
.class {
  one: 1;
}
.class .brass {
  three: 3;
}
```

## Properties as Variables

```less
.widget {
  color: #efefef;
  background-color: $color;
}
```

```css
.widget {
  color: #efefef;
  background-color: #efefef;
}
```

```less
.block {
  color: red; 
  .inner {
    background-color: $color; 
  }
  color: blue;  
} 
```

```css
.block {
  color: red; 
  color: blue;  
} 
.block .inner {
  background-color: blue; 
}
```

## Default Variables

```less
// library
@base-color: green;
@dark-color: darken(@base-color, 10%);

// use of library
@import "library.less";
@base-color: red; // 在当前文件, 可以覆盖掉library的
```

## Parent Selectors

### 父选择器`&`

```less
a {
  color: blue;
  &:hover {
    color: green;
  }
}
```

```css
a {
  color: blue;
}

a:hover {
  color: green;
}
```

```less
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

```css
.button-ok {
  background-image: url("ok.png");
}
.button-cancel {
  background-image: url("cancel.png");
}
.button-custom {
  background-image: url("custom.png");
}
```

### Multiple `&`

```less
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

```css
.link + .link {
  color: red;
}
.link .link {
  color: green;
}
.link.link {
  color: blue;
}
.link, .linkish {
  color: cyan;
}
```

### Combinatorial Explosion

```less
p, a, ul, li {
  border-top: 2px dotted #366;
  & + & {
    border-top: 0;
  }
}
```

```css
p,
a,
ul,
li {
  border-top: 2px dotted #366;
}
p + p,
p + a,
p + ul,
p + li,
a + p,
a + a,
a + ul,
a + li,
ul + p,
ul + a,
ul + ul,
ul + li,
li + p,
li + a,
li + ul,
li + li {
  border-top: 0;
}
```

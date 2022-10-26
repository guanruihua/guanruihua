# List Function

## length

```less
@list: "banana", "tomato", "potato", "peach";
n: length(@list);
```

```css
n: 4;
```

## extract

```less
@list: apple, pear, coconut, orange;
value: extract(@list, 3);
```

```css
value: coconut;
```

## range

- 生成列表
- 参数
  - `start`: （可选）开始值，例如1或1px
  - `end`: 结束值，例如5px
  - `step`: （可选）增量

```less
value: range(4);
value: range(10px, 30px, 10);
```

```css
value: 1 2 3 4;
value: 10px 20px 30px;
```

## each

- 将规则集的计算绑定到列表的每个成员
- 参数
  - `list` : 以逗号或空格分隔的值列表。
  - `rule` : 匿名规则集/混合

```less
@selectors: blue, green, red;

each(@selectors, {
  .sel-@{value} {
    a: b;
  }
});
```

```css
.sel-blue {
  a: b;
}
.sel-green {
  a: b;
}
.sel-red {
  a: b;
}
```

```less
@set: {
  one: blue;
  two: green;
  three: red;
}
.set {
  each(@set, {
    @{key}-@{index}: @value;
  });
}
```

```css
.set {
  one-1: blue;
  two-2: green;
  three-3: red;
}
```

```less
.set-2() {
  one: blue;
  two: green;
  three: red;
}
.set-2 {
  each(.set-2(), .(@v, @k, @i) {
    @{k}-@{i}: @v;
  });
}
```

```css
.set-2 {
  one-1: blue;
  two-2: green;
  three-3: red;
}
```

```less
each(range(4), {
  .col-@{value} {
    height: (@value * 50px);
  }
});
```

```css
.col-1 {
  height: 50px;
}
.col-2 {
  height: 100px;
}
.col-3 {
  height: 150px;
}
.col-4 {
  height: 200px;
}
```

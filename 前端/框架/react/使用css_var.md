# React 使用 css var

### less 使用css `var()`

```css
// 变量申明和计算
@pre-primary-color: #f00;

body {
  --pirmary-color: tint(@pre-primary-color, 20%);
}

@primary: var(--pirmary-color);

// 使用
div {
  color: @primary;
}
```

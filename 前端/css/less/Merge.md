# Merge

## Comma

```less
.mixin() {
  box-shadow+: inset 0 0 10px #555;
}
.myclass {
  .mixin();
  box-shadow+: 0 0 20px black;
}
```

```css
.myclass {
  box-shadow: inset 0 0 10px #555, 0 0 20px black;
}
```

## Space

```less
.mixin() {
  transform+_: scale(2);
}
.myclass {
  .mixin();
  transform+_: rotate(15deg);
}
```

```css
.myclass {
  transform: scale(2) rotate(15deg);
}
```

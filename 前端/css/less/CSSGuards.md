# CSS Guard

```less
.my-optional-style() when (@my-option = true) {
  button {
    color: white;
  }
}
.my-optional-style();

//  等同于
button when (@my-option = true) {
  color: white;
}
```

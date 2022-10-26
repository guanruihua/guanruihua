# requestAnimationFrame

```js
(() => {
   let n = 0
   function test() {
    n++
    console.log(`🚀🚀hello ~ requestAnimationFrame ${n}`);
    if (n < 11) requestAnimationFrame(test)
   }
   requestAnimationFrame(test)
  })()
```

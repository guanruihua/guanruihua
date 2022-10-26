# @plugin At-Rules

```less

@plugin "my-plugin";  // automatically appends .js if no extension
.show-me-pi {
  value: pi();
}

// output
.show-me-pi {
  value: 3.141592653589793;
}
```

- 使用模块导出

```js
// my-plugin.js
module.exports = {
    install: function(less, pluginManager, functions) {
        functions.add('pi', function() {
            return Math.PI;
        });
    }
};
```

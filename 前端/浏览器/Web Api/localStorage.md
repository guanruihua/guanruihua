# LocalStorage

> - 长度:
>   - 10M的字节数
>   - 5M，单位就是字符串的长度，而不是字符数。
>
> - localStorage 存储的键和值始终采用 UTF-16 DOMString 格式，每个字符使用两个字节。与对象一样，整数键将自动转换为字符串。
>   - 每个字符使用两个字节，是有前提条件的，就是码点小于`0xFFFF`(65535)， 大于这个码点的是四个字节。
>
- 特点:

1. 存储到浏览器的会话中, localStorage的数据可以长期保存
2. localStorage的键值对是以字符串的形式存储
3. localStorage 目前是支持IE8以上的浏览器
4. 解决了cookie存储空间不足的问题 cookie最大4k localStorage 最大为5M
5. 特定于页面的协议

- 添加&&修改

```js
/*
 * key存在时, 则是修改key对应的value
 */
localStorage.setItem('test','this is a test');
```

- 读取

```
localStorage.getItem('test');
```

- 删除

```
localStorage.removeItem('test');
```

- 删除全部 localStorage

```
localStorage.clear();
```

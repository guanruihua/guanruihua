# IE8

## IE在input内回车关闭自动触发button

<https://www.qttc.net/393-ie-enter-trigger-button-click.html>

> 在IE中有一个现象，当你在input内敲回车时，IE会自动寻找第一个button标签并且触发它。

index.html:

```html
<p><button onclick="console.log('from button click');">button</button></p>
<p><input type="text" value="在这里回车就触发提交按钮" style="width:300px;" /></p>
```

> 使用IE打开，并且在input内回车，就会执行button的onclick事件，如果你有两个button，只会触发第一个，如

```html
<p><button onclick="console.log('from button click1');">button1</button></p>
<p><button onclick="console.log('from button click2');">button2</button></p>
<p><input type="text" value="在这里回车就触发提交按钮" style="width:300px;" /></p>
```

> 回车后只会触发button1的onclick，但可笑的如果你把button标签换成input然后在把type设置为button却不会发生这个奇怪的事，虽然效果是一样。

```html
<p><input type="button" onclick="console.log('from button click1');" value="button1" /></p>
<p><button onclick="console.log('from button click2');">button2</button></p>
<p><input type="text" value="在这里回车就触发提交按钮" style="width:300px;" /></p>
```

> 回车会避开button1，执行button2。
> 如果你不想在input内执行回车就被执行button的onclick事件可以在页面顶部添加一个button，毕竟在input内敲回车是一种经常犯的毛病。如:

```html
<p><button>禁止input回车触发的button</button></p>
<p><button onclick="console.log('from button click1');">button1</button></p>
<p><button onclick="console.log('from button click2');">button2</button></p>
<p><input type="text" value="在这里回车就触发提交按钮" style="width:300px;" /></p>
```

> 这样第一个button就劫持了input的回车事件，防止了后面功能onclick被意外触发的可能性，但页面不美观
> 但不能把button设置display:none给隐藏，或者visibility:hidden占位隐藏，否则不能起到劫持input内回车事件，如

```html
<p><button style="display:none;">禁止input回车触发的button</button></p>
<p><button onclick="console.log('from button click1');">button1</button></p>
<p><button onclick="console.log('from button click2');">button2</button></p>
<p><input type="text" value="在这里回车就触发提交按钮" style="width:300px;" /></p>
```

> 以上代码在input回车仍然会找到button1并执行onclick事件，如果要隐藏它，但又能保持劫持input回车事件的功能，可以把在button外面再套一层父标签并且把父标签设置为宽与高为0即可

### 解决

#### 方法一: 使用`input[type='text']` 替代

```html
<div style="display:block; width:0; height:0; overflow:hidden;">
  <button>禁止input回车触发的button</button>
</div>
<p><button onclick="console.log('from button click1');">button1</button></p>
<p><button onclick="console.log('from button click2');">button2</button></p>
<p><input type="text" value="在这里回车就触发提交按钮" style="width:300px;" /></p>
```

> 这样就相当于把button隐藏了，并且还保住了功能，如果嫌麻烦可以直接把button改成input[type=button]或者使用a标签代替

#### 方法二: 添加 `type='text'`

> 给button标签 添加 `type='text'`属性

#### 方法三: 使用`a`标签替换

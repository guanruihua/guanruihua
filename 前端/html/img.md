# Img

## 判断src是否有效

- 通过onError事件来处理:

- `<img src='url1' onError='imageError(this)'/>` `imageError是自定义的方法`

 ```js
 function imgerror(img){
  img.src="images/default.jpg";
  img.οnerrοr=null;  //控制不要一直跳动
 }
 ```

```js
if(pic==null){
  html='<img src="images/bg_test1.jpg" width="320" height="157" />';
}else{
 html='<img src="'+pic+'" width="320" height="157" οnerrοr="imgerror(this)" />';
}


function imgerror(img){
 img.src="images/default.jpg";
 img.οnerrοr=null;   //控制不要一直跳动
}
```

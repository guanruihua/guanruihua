# URL

## 获取URL?后的参数

```js
/**
   * 获取url后的 id 文章id
   */
  function getUrlData() {
    var url = window.location.search; //url中?之后的部分
    url = url.substring(1); //去掉?
    var dataObj = {};
    if (url.indexOf('&') > -1) {
      url = url.split('&');
      for (var i = 0; i < url.length; i++) {
        var arr = url[i].split('=');
        dataObj[arr[0]] = arr[1];
      }
    } else {
      url = url.split('=');
      dataObj[url[0]] = url[1];
    }
    /**
     * 获取url id的值
     */
    if (dataObj.id) {
      return dataObj.id;
    }else{
        return "没有这个属性"
    }
  }
  console.log(getUrlData());
```

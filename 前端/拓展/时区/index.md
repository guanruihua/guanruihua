# 时区

> 注意`Date()`的默认时区问题, 我这里采用统一指定为格林威治时间, 然后通过偏移量来转换

## 基本使用

### `Date`支持的格式

```js
 new Date('4-21-2019 21:00:00 EST'),
  new Date("2019/04/21 21:00:00 EDT"),
  new Date("2019/04/21 9:00 PM EDT"),
  new Date('4/21/2019 9:00 PM EST'),
  new Date('2019-04-21T21:00:00'),
  new Date("Apr 21, 2019 9:00:00"),
  new Date("April 21, 2019 9:00:00"),
  new Date(95, 3, 21), //两位数年份表示 1900 - 1999 年,
  new Date(),
  new Date(1453094034000),
  new Date('Tue Apr 02 2019 16:39:08'),
  new Date(2019, 3, 2, 16, 37, 22),
  new Date("2019-04-21 21:00:00 EDT"),
  new Date("2019-04-21 21:00:00 EDT").toUTCString(),
```

## momnent

> [文档 | Moment.js 中文网 (momentjs.cn)](http://momentjs.cn/docs/#/parsing/now/)
>
> [Moment Timezone | Home (momentjs.com)](https://momentjs.com/timezone/)

### Install & Use

```shell
// Install 
bower install moment-timezone --save # bower
npm install moment-timezone --save   # npm
yarn add moment-timezone             # Yarn

// Use
<script src="moment.js"></script>
<script src="moment-timezone-with-data.js"></script>
let moment = require('moment-timezone');
```

### Webpack

```js
// webpack.config.js
const MomentTimezoneDataPlugin = require('moment-timezone-data-webpack-plugin');
const currentYear = new Date().getFullYear();

module.exports = {
    plugins: [
        // To include only specific zones, use the matchZones option
        new MomentTimezoneDataPlugin({
            matchZones: /^America/
        }),

        // To keep all zones but limit data to specific years, use the year range options
        new MomentTimezoneDataPlugin({
            startYear: currentYear - 5,
            endYear: currentYear + 5,
        }),
    ],
};
```

### 常用方法

> 常用方法 :
>
> 1. `moment([待转换的时间]).tz([指定时区]).format([时间格式])`
> 2. `moment.tz([待转换的时间],([指定时区]).format([时间格式])`
>
> 注意点:
>
> 1. `待转换的时间`: 注意格式, 不是全部时间格式都支持的
> 2. `指定时区`: 注意和其他语言支持的指定时区格式不完全支持
> 3. `时间格式`:
>    1. `z`:  时区

```js
sessionStorage.setItem("sysTimeZoneOffset", "28800000");// 服务器时区
sessionStorage.setItem("refTimeZoneOffset", "28800000");// 指定的时区

// 将时间 强行指定为格林威治时间, 然后再转换为时间戳
const f_formatToTimeStamp = (time) => {
 if (time.match(/^\d{4}(-|\s)\d{1,2}(-|\s)\d{1,2}(\s)\d{1,2}:\d{1,2}/g)) {
  time += " GMT";
 } else if (time.match(/^\d{4}(-|\s)\d{1,2}(-|\s)\d{1,2}$/g)) {
  time += " 00:00:00 GMT"
 } else if (time.match(/^\d{8}$/g)) {
  let temp = time.split("");
  temp.splice(4, 0, "-");
  temp.splice(7, 0, "-");
  time = temp.join("") + " 00:00:00 GMT";
 } else if (time.match(/^\d{14}$/g)) {
  let temp = time.split("");
  temp.splice(4, 0, "-");
  temp.splice(7, 0, "-");
  temp.splice(10, 0, " ");
  temp.splice(13, 0, ":");
  temp.splice(16, 0, ":");
  time = temp.join("") + " GMT";
 } else if (time.match(/^\d{1,2}-\d{1,2}-\d{4}$/g)) {
  time = time.split("-").reverse().join("-") + " 00:00:00 GMT";
 } else if (time.match(/^\d{10,}$/g)) {
  return String(time * 1000).split("").map((item, index) => index < 13 && item).filter(i => i !== false).join("");
 }
 return Date.parse(String(time))
}

// 通过偏移量来指定转换前后的时区
function f_Timezone(time = "", offset) {
 let timeStamp = f_formatToTimeStamp(String(time)) // 时间戳 string
 offset = offset || Number(sessionStorage.getItem('refTimeZoneOffset')) - Number(sessionStorage.getItem('sysTimeZoneOffset')) || 0;
 return momentz(Number(timeStamp) + Number(offset)).tz("GMT")
}

function f_TimezoneFormat(time = "", offset, format = "YYYY-MM-DD HH:mm:ss") {
 return f_Timezone(time, offset).format(format);
}

function f_gmtTime(time) {
 return Number(f_formatToTimeStamp(String(time))) + Number(new Date().getTimezoneOffset()) * 60 * 1000;
}
```

## [时区缩写](https://www.cnblogs.com/tracy/archive/2010/07/16/1778566.html)

```tex
EST: 美国东部标准时间 ，GMT-5
EDT: 美国东部夏令时时间 ，GMT-4
CST：美国中部标准时间，GMT-6
CDT：美国中部夏令时时间，GMT-5
PST: （美国）太平洋标准时间，GMT-8
PDT:（美国）太平洋夏令时时间，GMT-7
GMT: 格林威治标准时间，即UTC时间
CCT：北京时间（中国沿海时间），GMT+8
```

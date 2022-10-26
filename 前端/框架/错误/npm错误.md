# npm错误

## npm WARN build `npm build` called with no arguments. Did you mean to `npm run-script build`

> 把指令`npm run` 换成`npm run build`

## 执行 npm 命令时报错：Allocation failed - JavaScript heap out of memory

法一：添加参数 --max_old_space_size=4096

npm run start --max_old_space_size=4096

法二：修改cmd文件

在目录node_modules/.bin下打开ng.cmd和ngc.cmd文件，添加 --max_old_space_size=4096

![img](https://img-blog.csdnimg.cn/20201030160512476.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl8zODg4MzMzOA==,size_16,color_FFFFFF,t_70)

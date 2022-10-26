# bat

## 脚本

> 例子, 这是一个git 提交的模板

```shell
@echo off
set /p commit=commit:
title auto commit

rem 调用git命令，如果没有找到git命令，说明环境变量未配置
rem 将git改成git安装目录下的git.exe也可以实现

git add .
git commit -m %commit%
git push

pause
```

## 执行

> 1. `CMD`: 文件名.bat
>
> 2. `powershell`: ./文件名.bat
> 3. 直接点击运行

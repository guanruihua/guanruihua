# vscode 运行`c#`

### 1、下载SDK

首先下载 .NET Core SDK 。下载地址如下:

[.NET Downloads for Windows](https://www.microsoft.com/net/download/windows)

安装SDK：

![img](/__assets__/img/2021-12-28-22-02-54.png)

安装完成后调出控制台 输入 :

```shell
dotnet --version
```

弹出版本信息 说明安装成功
![](/__assets__/img/2021-12-28-22-03-22.png)

### 2、安装C#语言支持

在VS Code里找插件，安装C#的支持。
![](/__assets__/img/2021-12-28-22-05-05.png)

## 二、快速创建C#控制台

输入

```shell
dotnet new console -o D:\study\NetCore
```

讲解一下，new是新建，console是控制台，-o是IO路径，后面的是我准备安放的路径，D盘的study目录下，项目名称是NetCore
![](/__assets__/img/2021-12-28-22-05-23.png)

输出上述文字说明项目创建成功.

把这个文件夹拖进VS Code，或用打开vscode打开项目文件夹D:\study\NetCore。
![](/__assets__/img/2021-12-28-22-12-34.png)

这时右下角弹出通知提示，问你要不要为这个项目创建编译和调试文件，这里要选择“Yes”。

![img](https://www.kmbox.cn/uploads/allimg/200220/1-200220151603332.png)

这样就会自动配置好tasks.json 编译配置文件和launch.json调试配置文件。
![](/__assets__/img/2021-12-28-22-12-55.png)

直接使用终端在项目该目录下执行dotnet run，即可执行，或者按F5运行，然后选择Net Core调式器，在代码左边打个断点，可以发现，左方的Debug有变量的变化显示，上方也有断点进出的快捷键。
![](/__assets__/img/2021-12-28-22-13-06.png)

### 格式化代码

比如我的代码很乱，如下图

![img](https://img2018.cnblogs.com/blog/793293/201905/793293-20190515224448776-1346059103.png)

安装C# FixFormat插件
![](/__assets__/img/2021-12-28-22-12-05.png)

只需要右键，格式化代码，选择C# FixFormat方式格式化即可

切换默认的格式化工具成FixFormat：
![](/__assets__/img/2021-12-28-22-10-58.png)
![](/__assets__/img/2021-12-28-22-10-16.png)

格式化文档：
![](/__assets__/img/2021-12-28-22-08-46.png)

## 三、快速创建 MVC

1. 打开VS Code, 定位到你想要的文件夹 D:\study\NetCore\MVC

![](/__assets__/img/2021-12-28-22-07-35.png)
2. 使用快捷键 Ctrl + `, 这时候右下角会弹出终端窗口
![](/__assets__/img/2021-12-28-22-07-26.png)

3、使用 dotnet new mvc -n myapp

```shell
dotnet new mvc -n myapp
```

![](/__assets__/img/2021-12-28-22-07-11.png)

4.按F5 启动调试

右下角弹出通知提示，问你要不要为这个项目创建编译和调试文件，这里要选择“Yes”。
![](/__assets__/img/2021-12-28-22-06-23.png)
![](/__assets__/img/2021-12-28-22-06-34.png)

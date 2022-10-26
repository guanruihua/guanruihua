---
title: maven
date: 2021-01-22 09:31:02
tags:
- maven
- back-end
---

# maven

> [下载 - Maven – Download Apache Maven](http://maven.apache.org/download.cgi)

## maven目录

#### 1）bin

该目录包含了 mvn 运行的脚本，这些脚本用来配置 Java 命令，准备好 classpath 和相关的 Java 系统属性，然后执行 Java 命令。

其中 mvn 是基于 UNIX 平台的 shell 脚本，mvn.bat 是基于 Windows 平台的 bat 脚本。在命令行输入任何一条 mvn 命令时，实际上就是在调用这些脚本。

该目录还包含了 mvnDebug 和 mvnDebug.bat 两个文件，同样，前者是 UNIX 平台的 shell 脚本，后者是 Windows 平台的 bat 脚本。那么 mvn 和 mvnDebug 有什么区别和关系呢？

打开文件我们就可以看到，两者基本是一样的，只是 mvnDebug 多了一条 MAVEN_DEBUG_OPTS 配置，其作用就是在运行 Maven 时开启 debug，以便调试 Maven 本身。

此外，该目录还包含 m2.conf 文件，这是 classworlds 的配置文件，后面会介绍 classworlds。

#### 2）boot

该目录只包含一个文件，以 maven 3.3.9 为例，该文件为 plexus-classworlds-2.5.2.jar。

plexus-classworlds 是一个类加载器框架，相对于默认的 java 类加载器，它提供了更丰富的语法以方便配置，Maven 使用该框架加载自己的类库。

更多关于 classworlds 的信息请参考 http://classworlds.codehaus.org/。对于一般的 Maven 用户来说，不必关心该文件。

#### 3）conf

> 重点 **setting.xml**

该目录包含了一个非常重要的文件 settings.xml。直接修改该文件，就能在机器上全局地定制 Maven 的行为。

一般情况下，我们更偏向于复制该文件至 ～/.m2/ 目录下（～表示用户目录），然后修改该文件，在用户范围定制 Maven 的行为。后面将会多次提到 **settings.xml**，并逐步分析其中的各个元素。

#### 4）lib

该目录包含了所有 Maven 运行时需要的 Java 类库，Maven 本身是分模块开发的，因此用户能看到诸如 maven-core-3.0.jar、maven-model-3.0.jar 之类的文件。

此外，这里还包含一些 Maven 用到的第三方依赖，如 common-cli-1.2.jar、commons-lang-2.6.jar 等。

对于 Maven 2 来说，该目录只包含一个如 maven-2.2.1-uber.jar 的文件，原本各为独立 JAR 文件的 Maven 模块和第三方类库都被拆解后重新合并到了这个 JAR 文件中。可以说，lib 目录就是真正的 Maven。

关于该文件，还有一点值得一提的是，用户可以在这个目录中找到 Maven 内置的超级 POM，这一点教程后面会详细解释。

#### 5）LICENSE.txt

记录了 Maven 使用的软件许可证Apache License Version 2.0。

#### 6）NOTICE.txt

记录了 Maven 包含的第三方软件。

#### 7）README.txt

包含了 Maven 的简要介绍，包括安装需求及如何安装的简要指令等。



## 常用指令

| 指令                     | 描述          |
| ------------------------ | ------------- |
| `mvn -v`                 | 查看maven版本 |
| `mvn archetype:generate` | 创建一个项目  |

## 编译和测试

> 先切换到工程目录下

| 指令          | 描述                                                         |
| ------------- | ------------------------------------------------------------ |
| `mvn clean`   | 清楚以前编译安装过的历史结果                                 |
| `mvn compile` | 编译源代码                                                   |
| `mvn test`    | 运行测试案例进行测试                                         |
| `mvn install` | 将当前代码打层jar包, 安装maven的本地管理目录下, 其他maven工程指定坐标可以使用 |



> - `mvn site` : 生成站点信息
> - `mvn javadoc:javadoc`: 生成APIDoc文档



## 其他指令

> mvn archetype:create //创建 Maven 项目
> mvn compile //编译源代码
> mvn test-compile //编译测试代码
> mvn test //运行应用程序中的单元测试
> mvn site //生成项目相关信息的网站
> mvn package //依据项目生成 jar 文件
> mvn install //在本地 Repository 中安装 jar
> mvn -Dmaven.test.skip=true //忽略测试文档编译
> mvn clean //清除目标目录中的生成结果
> mvn clean compile //将.java类编译为.class文件
> mvn clean package //进行打包
> mvn clean test //执行单元测试
> mvn clean deploy //部署到版本仓库
> mvn clean install //使其他项目使用这个jar,会安装到maven本地仓库中
> mvn archetype:generate //创建项目架构
> mvn dependency:list //查看已解析依赖
> mvn dependency:tree //看到依赖树
> mvn dependency:analyze //查看依赖的工具
> mvn help:system //从中央仓库下载文件至本地仓库
> mvn help:active-profiles //查看当前激活的profiles
> mvn help:all-profiles //查看所有profiles
> mvn help:effective -pom //查看完整的pom信息



## Archetype插件

#### 1）maven-archetype-quickstart

maven-archetype-quickstart 应该是最常用的 Archetype。在用户输入命令行“mvn archetype:generate”时，如果没有指定使用哪个 Archetype，默认就是使用 quickstart。使用 maven-archetype-quickstart 生成的项目比较简单。

- pom.xml，包含有 JUnit 的依赖声明。
- src/main/java，主代码目录以及一个名为 App 的 [Java](http://c.biancheng.net/java/) 类。
- src/main/test，测试代码目录以及一个名为 AppTest 的 JUnit 测试用例类。


如果需要创建一个全新的 Maven 项目，可以使用该 Archetype 生成项目架构，再在该架构的基础上进行对应地修改，比如添加依赖、添加 resources 目录等，从而省去手动创建 pom 以及目录结构的麻烦。

#### 2）maven-archetype-webapp

maven-archetype-webapp 是一个创建 Maven War 项目的 Archetype。它能创建一个 Web 应用的基本目录结构和必需的 web.xml。使用 maven-archetype-webapp 生成如下内容。

- pom.xml packaging 的值为 war，带有 JUnit 的依赖声明。
- src/main/webapp 目录。
- src/main/webapp/index.jsp 文件。
- src/main/webapp/WEB-INF/web.xml 文件。
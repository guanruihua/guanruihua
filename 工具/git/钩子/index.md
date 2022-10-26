# Git钩子

### 钩子

> 钩子的全部放在`.git/hooks`下面，在新建一个项目仓库的时候，Git已经在这个文件夹下给我们生成了很多个`.sample`后缀的钩子，这些钩子只要把`.sample`去掉就可以运行了，我们可以在这些`sample`上面修改完成我们自己的钩子

### 客户端钩子

> 官网链接[钩子](https://git-scm.com/book/zh/v2/自定义-Git-Git-钩子#_git_hooks)

客户端钩子很好理解，你`commit`之后想做其他事，比如说编译一下程序啥的，这里我就不多讲，主要由下面几个钩子组成

- pre-commit 提交之前
- post-commit 提交之后
- pre-rebase 变基之前
- post-rewrite 替换提交记录之后
- pre-push 推之前

### 服务端钩子

服务端钩子就是你`push`之后的事情服务器要运行的脚步，有用推的步骤只有一个，所以钩子只有四个

- pre-receive 接受之前
- update 更新之前
- post-update 更新之后
- post-receive 接受之后

服务器接收到客户端请求时，`pre-receive`先进行调用，如果返回值为非0就会拒绝推送，所以我们写钩子的时候一定要记住最后要返回0才能正常接收更新，`update`主要处理多分支推送，有的时候你一次更新，推三四个分支到服务器，`pre-receive`只会调用一次，`update`会对每个的分支调用一次，后面两个都很容易理解

一般我们就是要在服务端更新代码之后运行脚步，所以我们要修改的就是`post-update`或者`post-receive`

bash脚步大家都会写，但是大家可能会很陌生什么是Git服务端，接下来我们就来介绍一下Git服务端是什么

### Git 服务端

大家一般使用Git都是使用的客户端，但是Git这个工具的确很强，它不但可以当做客户端，也可以当做服务端，为了让大家更好的理解Git服务端，我们先来拿本地文件做”服务器“

首先我们先新建一个文件夹为`server`，在新建一个文件夹为`local`，假设文件夹都在`/root`文件夹下

我们执行下面的命令生成服务器

```
cd /root/server
git init --bare
复制代码
```

只需要在`init`后面添加一个`--bare`选项告诉Git，Git就会帮我们生成一个空的“服务端”，我们可以查看一下文件，我们发现Git 给我们生成下面几个文件夹，其中就有我们的hooks

```
branches  config  description  HEAD  hooks  info  objects  refs
复制代码
```

但是服务端和客户端生成的位置不一样，客户端是给我们生成一个`.git`文件夹，里面放了这些文件夹，然而服务端直接将这些文件夹放在主目录了

行我们已经生成了服务端的，接下来我们生成客户端的钩子

```
cd /root/local
git init
复制代码
```

很简单，同我们往常操作一样，我们这时候添加一个`README.md` 然后`commit`一下准备开始往服务端推代码了

在 linux 下直接执行下面命令就行

echo “local update” >> README.md
git add README.md
git commit -m “Add ReadME”

接下来我们就要向”服务器“提交代码了，我们先添加本地文件作为远程服务器

```
git remote add origin file:////root/server
复制代码
```

然后直接推代码

```
git push origin master
复制代码
```

这样我们就向我们文件提交了代码，这时候我们回到我们”服务器“

```
cd /root/server
ls
branches  config  description  HEAD  hooks  info  objects  refs
复制代码
```

我们惊奇的发现服务器并没有我们新建的`README.md`文件，原来Git服务端并不像SVN一样只保留一份代码大家共同修改，**Git服务端只是记录文件变化和分支变化**

这里插一句我为什么会去了解Git钩子，由于一开始实现服务器自动更新我的FastProxyScan项目代码，但是我又不想使用`Github`钩子（push后发送http请求），太麻烦了，后来我一想干脆直接推到我的服务器上，但是推到服务器上的代码只是记录了分支和提交信息，不包含源文件，所以我只好在在服务器上部署这个项目，并添加一个服务器钩子，当服务器更新完成后，再用钩子把服务器上的项目代码更新

### 如何写服务器钩子

通过上面对本地文件新建仓库，我们知道Git“服务端”新建很简单，我们一般接触比较多的是Github服务端，但是Git非常强大，他可以支持多种协议来连接“服务端”，比如说我们上面用到的本地文件（`file`协议），假如你用`ssh`连接远程服务器，你也可以使用类似`git remote add origin ssh://username@ip/file/path`添加ssh远程仓库

git 支持的协议有ssh、http、https、file、git等协议，你只要确保你能连接上远程服务器就行，接下来我们谈谈如何写服务器钩子

在使用`git init --bare`新建了一个Git服务端之后，在服务端文件下面有一个`hooks`文件夹，我们要做的就是把脚本放到`hooks`文件夹里面（当然你要确保它有执行权限），如果你更擅长写`Python`，`Ruby`那些脚步也可以，不过要确保前缀后后缀正确。

这里要提到很重要的一点，由于在执行钩子的时候，环境变量`GIT_DIR`被设置为服务端当前目录，如果你像我一样想更新在另外一个文件夹下面的项目代码，你必须使用`uset GIT_DIR`清除变量名，否则只会更新服务端，而不会更新你的项目代码

这里我提供一个模板

文件名为 `post-update`或者`post-receive`

```
#!/bin/sh
cd /project/path/ || exit
unset GIT_DIR
git pull origin master

exec git-update-server-info
复制代码
```

你只需修改项目文件路径和仓库名即可

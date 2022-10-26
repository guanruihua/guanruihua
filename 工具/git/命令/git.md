# git

## 前言

> 工具:  [TortoiseGit](https://gitee.com/enterprises?utm_source=baidu&utm_medium=sem&utm_term=110206&utm_campaign=enterprise&bd_vid=8015310045877046198)
>
> 工作流程: 工作目录 -> 暂存区 -> GIT仓库

![image](https://img-blog.csdnimg.cn/img_convert/b7c2d1149f813df1097f1888eb04e94f.png)

## git常用指令

【git init 】：为当前项目创建一个本地仓库（即需要事先有一个创建好的项目）
【git clone [url]】：克隆一个仓库
【git status 】：查看git的状态
【git add ./】：添加当前目录下的所有到“Stage”中
【git commit】：提交“Stage”中的内容到本地仓库中
【git pull origin master】：拉去远程仓库“origin”相应的分支到本地仓库中的master中，在push之前常常需要先pull一下最新的仓库
【git pull origin master --allow-unrelated-histories】：如果本地与远程是两个独立的仓库，则在pull时需要指定其“允许无关的日志合并”，否则会保存
【git push origin master】：将本地仓库中的内容提交到远程仓库中（origin为远程分支的名字，master为当前分支的名字，应注意的是，该命令是不完全完整的，在实际使用中，如果是第一个提交到远程，应该使用的命令为【git push -u origin master】，如果不是第一次，则直接使用【git push】即可）
【git remote add origin [url]】：添加一个远程仓库连接，并为其命名为"origin"，当然也可以自定义其它的名字，但一般都命名为origin
【git remote rm origin】：删除一个名为"origin"的远程仓库连接
【git remote set-url origin [url]】：修改"origin"的url（结合上面一个命令可以发现，想要修改远程仓库连接有两种方法：一种是先删除再添加，一种是直接修改）

```shell
git status // 查看代码状态
git checkout [ 分支名称 ] // 查看分支的状态
git add . // 添加修改
git commit -m "[描述]" //添加到本地换成
git push 
git push -u origin master
git config user.name "[username]" // 配置用户名
git config user.email "[eamil]" // 配置邮箱
git config user.name // 查看用户名
git config user.email // 查看用户邮箱
```

```shell
git init
git add .
git commit -m 'message'
git remote add origin 远程库地址
git push -u origin main | master
```

```shell
echo "# note" >> README.md
git init
git add README.md
git commit -m "first commit"
git branch -M main
git remote add origin git@github.com:guanruihua/note.git
git push -u origin main
                
```

```shell
git remote add origin git@github.com:guanruihua/note.git
git branch -M main
git push -u origin main
```

## 建立远程仓库连接

```shell
# 和远程仓库建立连接
git remote add origin https://github.com/Jacob-xyb/Projiect  
git remote remove origin https://github.com/Jacob-xyb/Projiect # 删除远程仓库
```

## 提交

### 普通提交

```shell
git commit -m "commit"
git push -u origin main | master
```

```shell
git add .
git commit -m "描述"
git push origin // 提交到已经切换到分支
```

### 强制提交

```shell
报错内容：
To github.com:***/***
! [rejected] master -> master (non-fast-forward)
error: failed to push some refs to ‘git@github.com:***/***’
hint: Updates were rejected because the tip of your current branch is behind
hint: its remote counterpart. Integrate the remote changes (e.g.
hint: ‘git pull …’) before pushing again.
hint: See the ‘Note about fast-forwards’ in ‘git push --help’ for details.
```

```shell
git push origin master -f
```

### 提交到其他分支

> 首先要建立本地的分支，并切换到该分支上（本地建立完分支，默认是在master分支上）

```shell
git branch hello_git_branch // 创建分支
git checkout hello_git_branch // 检查该分支
git push origin hello_git_branch // push到远程仓库上面
```

## git 查看仓库信息

```shell
git log    # 查看日志信息
git reflog    # 查看一行日志
git remote -v  # 查看当前项目的远程git地址
cat .git/HEAD  # 查看当前HEAD
```

查看系统config

```
git config --system --list
```

查看当前用户（global）配置

```
git config --global --list
```

查看当前仓库配置信息

```
git config --local --list
```

## git 更新代码到本地

### 正规流程

```shell
git status（查看本地分支文件信息，确保更新时不产生冲突）
git checkout – [file name] （若文件有修改，可以还原到最初状态; 若文件需要更新到服务器上，应该先merge到服务器，再更新到本地）
git branch（查看当前分支情况）
git checkout remote branch (若分支为本地分支，则需切换到服务器的远程分支)
git pull
```

### 快速流程

```shell
git pull (一句命令搞定)
git branch 看看分支
git checkout aaa 切换分支aaa
git branck aaa 创建aaa分支
git chechout -b aaa 本地创建 aaa分支，同时切换到aaa分支。只有提交的时候才会在服务端上创建一个分支
```

## git 撤销

用暂存区中的文件覆盖工作目录中的文件：
`git checkout 文件名字`

从暂存区中删除文件：
`git rm --cached 文件`

将 git 仓库中指定的更新记录恢复出来，并且覆盖暂存区和工作目录：

```shell
git reset --hard commitID  # 恢复到指定ID版本
git reset --hard HEAD^  # 回退上一个版本
git reset --hard HEAD~1  # 回退上一个版本 // ～n 就是回退n个版本
```

注意：如果将版本指针前置的话，指针后面的日志就会消失。
删除 远程仓库 中的文件
`git rm -r --cached 文件`

修改提交的commit注释
`git commit --amend`

> 参数讲解
> `--mixed`
> 不删除工作空间改动代码，撤销commit，并且撤销git add . 操作
>
> 这个为默认参数，git reset --mixed HEAD^ 和 git reset HEAD^ 效果是一样的。
>
> `--soft`
> 不删除工作空间改动代码，撤销commit，不撤销 git add .
>
> `--hard`
> 删除工作空间改动代码，撤销 commit，撤销 git add .
>
> 注意完成这个操作后，就恢复到了上一次的commit状态。

## 版本回退

```shell
git add . // 提交到暂存去
git commit -m "描述" // 提交到本地分支
git push -u origin master // 提交到远程服务器(第一个版本)

...
git push origin master // 修改项目后提交 ( 第二个版本 )

git add .
git commit -m "描述"
git push origin master // 修改项目后提交 ( 第三个版本 )

git log // 查看历史提交记录
git log --pretty =online // 查看历史提交记录[减少多余信息]
git reset --hard HEAD^ // 回到上一个版本
git reset --hard HEAD~100 // 回退到100个版本前

git reflog // 查看已经回退过的版本记录
```

## git 分支

分支细分
主分支（master）：第一次向 git 仓库中提交更新记录时自动产生的一个分支。
开发分支（develop）：作为开发的分支，基于 master 分支创建。
功能分支（feature）：作为开发具体功能的分支，基于开发分支创建。
查看本地和远程分支

### 查看本地分支（*为当前分支）

git branch == git branch --list == git branch -l

### 查看本地和远程分支

git branch --all == git branch -a

### 查看远程所有分支

git branch --remote == git branch -r

### 切换分支

git checkout 分支名称

### 分支重命名

git branch -m [oldName] [newName]

切换分支之前要保持当前暂存区完全干净，不然会出现错误。

这是由于 git 的暂存区是所有分支共享的，因此，只要未提交(commit)的内容都会在所有分支上呈现。有时，我们还未完成当前分支的功能开发，但是又亟需在另一个分支上修改内容(如：修复一个 BUG)。

一种方式是通过“git commit”(提交)，解决这个问题。但在正常使用中，我们应该在完成某个功能的开发后才提交一个版本，而不是频繁的提交。

这时，我们可以使用“git stash”(储藏)来解决这个问题。

### 创建分支

git branch 分支名称   # 创建分支（基于所在分支创建）
git checkout -b branchName commitId # 根据指定版本号创建分支

#### 合并和删除分支

##### 合并

git merge 来源分支   # 合并分支（将来源分支合并到当前分支）

##### 删除

git branch -d 分支名称  # 删除本地分支（如果删除的分支没有被合并是不能删除的）
git branch -D 分支名称  # 强制删除本地分支
git push origin -d 分支名称 # 删除远程分支[Git > v1.7.0](推荐)
git push origin :分支名称 # 删除远程分支(":"代表删除)

##### 暂时保存更改

在 git 中，可以暂时提取分支上所有的改动并存储，让开发人员得到一个干净的工作副本，临时转向其他工作。

使用场景：分支临时切

git stash  # 临时保存
git stash pop # 恢复保存

##### 分支管理

### 将本地develop分支强制（-f）推送到远程master

git push origin develop:master -f

### 重置的方法

git checkout master  # 切换到旧分支
git reset --hard develop  # 将本地的旧分支master重置成develop
git push origin master --force # 强制推送

### 删除远程分支

`git push origin --delete develop # 删除远程仓库`
`git push origin:develop   # 推送至服务器才行`

## 一台电脑同时使用gitee和github

### 创建ssh key

```shell
# 进入用户目录下的 .ssh 文件夹下，路径会因你使用的操作系统不同而略有差异
# 没有这个文件夹也无所谓，直接运行下一句命令也可以
cd ~/.ssh

# 生成 key，将邮件地址替换为你 Gitee 或者 Github 使用的邮件地址
ssh-keygen -t rsa -C "xxx@xxx.com"

# gitee
Generating public/private rsa key pair.
Enter file in which to save the key (/c/Users/your_user_name/.ssh/id_rsa): id_rsa_gitee
Enter passphrase (empty for no passphrase)://这里直接回车
Enter same passphrase again://这里直接回车
# github
重复上过程,吧id_rsa_gitee 改成 id_rsa_github
```

### 在gitee和github添加public key

```shell
cd ~/.ssh
cat id_rsa_gitee.pub
//这里 gitee 的public key
cat id_rsa_github.pub
//这里 github 的public key
```

然后再分别添加到github 和 gitee 的 SSH

### 创建配置文件

在.ssh文件夹创建config文件, 添加内容 区分 两个 ssh key

```shell
# gitee
Host gitee.com
HostName gitee.com
PreferredAuthentications publickey
IdentityFile ~/.ssh/id_rsa_gitee

# github
Host github.com
HostName github.com
PreferredAuthentications publickey
IdentityFile ~/.ssh/id_rsa_github
```

### 测试连接是否正常

```shell
ssh -T git@github.com

Hi guanruihua! You've successfully authenticated, but GitHub does not provide shell access.


ssh -T git@gitee.com

Hi grh-gitee! You've successfully authenticated, but GITEE.COM does not provide shell access.
```

---
title： nvim
time： 2020-12-31 15:15:34
tags：
- vim
- neovim
- nvim
- vim-plug
---



# nvim

## 缓冲区

> `:ls`
>
> 会显示多个存文件
>
> :buffer  编号 / 后面的路径名

## 窗口控制

| 描述                          | 快捷键                   | 补充                         |
| ----------------------------- | ------------------------ | ---------------------------- |
| 关闭当前窗口                  | `Ctrl  + w  c`           | 关闭最后一改窗口`Ctrl + w q` |
| 上下 \| 左右 分割当前文件     | `Ctrl + w s | v`         |                              |
| 上下 \| 左右 打开一个新的文件 | `:sp | :vsp filename`    |                              |
| 光标移动到 上下左右 屏幕      | `Ctrl + w k | j | h | l` |                              |
| 窗口移动 上下左右 移动窗口    | `Ctrl + w K | J | H | L` |                              |

> 最快的方法：
> 1改变高度：res +n（增加n行的高度)
>
> 2.改变宽度：vertical res +n（增加n列的宽度）
>
> [ n : 可以使用+/-来表示添加或减少对应的列数 ]

## Neovim 和 vim-plug 安装

> vim的插件管理器
>
> 环境是 windows10
>
> 配置文件路径: `C:\Users\ruihuag\AppData\Local\nvim`

### 安装包准备

> [nvim](https://github.com/neovim/neovim/releases/tag/nightly): 直接下载nvim-win64.zip
>
> [vim-plug](https://github.com/junegunn/vim-plug): 要是无法下载`plug.vim`, 可以选择下载整个vim-plug项目, 里面有个文件就是`plug.vim`

### 开始安装

> - neovim\bin\nvim-qt点击打开就可以开始使用
>
> - 把`neovim\bin`添加到环境变量中
>
> - 在C:User\用户名\Appdata\Local下创建nvim文件夹
>
> - 在nvim文件中创建autoload文件夹 , plugged文件夹 和`init.vim`(配置文件)
>
>   - autoload: 放`plug.vim`文件
>
>   - plugged: 存放下载下来的vim插件
>
>   - init.vim : 配置文件填写下面配置
>
>     - ```vim
>       call plug#begin('~/AppData/Local/nvim/plugged')
>       Plug 'junegunn/vim-easy-align'
>       Plug 'scrooloose/nerdtree'
>       call plug#end()
>       ```
>
> - cmd控制台: 输入`nvim`打开,在命令模式输入`:PlugInstall`安装插件

## vim-plug 使用

> 常用插件： [https://vimawesome.com](https://vimawesome.com/)

## 组件安装和卸载

> 安装
>
> - 添加`Plug '插件名'`
> - 命令模式`:PlugInstall`
>
> 卸载
>
> - 删除添加的`Plug '插件名'`
> - 命令行模式`:PlugClean`, 选择y就可以彻底删除插件

## NERDTree 文件树

| 描述     | 操作                                                         |
| -------- | ------------------------------------------------------------ |
| 打开目录 | `:NERDTree` | `<F8>`                                         |
| 目录操作 | `m` : 创建文件 \| 删除文件 \| 复制文件 \| 移动文件 等<br>P       跳到根结点<br/>p       跳到父结点<br/>K       跳到当前目录下同级的第一个结点<br/>J       跳到当前目录下同级的最一个结点 |
| 窗口移动 | `ctrl + w` ( 两次 ) [ 向右切 ]  <br/>`ctrl + w + h|j|k|l`[ 移动对应方向 ] <br/>`ctrl + h|j|k|l`[ 移动对应方向 ] |
| 标签页   | `[ 回车 ]` :  新标签页打开<br>   T: 新标签打开, 光标不跳转  <br> `gt` : 向右切<br> `gT`: 向左切 <br>`alt+[number]`: 跳转到对应的标签页 |
| 分屏打开 | `o`: 打开文件<br> `i`: 水平分屏打开<br/> `s`: 垂直分屏打开<br/>[ 前加`g`: 打开, 但光标不跳转 ] |

> ```shell
> !       执行当前文件
> O       递归打开选中 结点下的所有目录
> x       合拢选中结点的父目录
> X       递归 合拢选中结点下的所有目录
> e       Edit the current dif
> 
> C       将选中目录或选中文件的父目录设为根结点
> u       将当前根结点的父目录设为根目录，并变成合拢原根结点
> U       将当前根结点的父目录设为根目录，但保持展开原根结点
> r       递归刷新选中目录
> R       递归刷新根结点
> cd      将 CWD 设为选中目录
> 
> :tabnew [++opt选项] ［＋cmd］ 文件      建立对指定文件新的tab
> :tabclose   关闭当前的 tab
> :tabo   关闭所有其他的 tab
> :tabs   查看所有打开的 tab
> :tabp   前一个 tab
> :tabn   后一个 tab
> ```
>

## sandwich 环绕字符编辑

> `t`: html标签 [ `tp`: `<p>` ]
>
> `b`:  指代成对的符号 [ 修改的时候 ]
>
> `选择区域` : `w`, `3w`, `$`, `W`,`0`,`f"`[  查找 ]
>
> `sa` : 新增
>
> `sr`: 修改
>
> `sd`: 删除

> - 新增环绕字符: `sa`
>   - `sa[选择区域][符号 | 标签]`: 给多个词加该符号
>   - `sa[选择区域][符号 | 标签]`: 给多个词加该符号[ 不包括符号 ]
>   - 可视化模式下, 选择区域+`sa[符号 | 标签]`
> - 修改环绕字符: `sr`
>   - `srb[符号|]`: 将成对符号修改成该符号
>   - `srbTp`: 修改成`<p>`
>   - `srtth1`: 修改成`<h1>`
>   - `srT)`: 将标签修改成`()`
> - 删除环绕字符: `sd`
>   - `sdb` : 删除引号或括号
>   - `sdt`: 删除标签

## vim-colorschemes 颜色主题

> colorschemes的github主页为：<https://github.com/flazz/vim-colorschemes>
> 在文档中找到`Using`，选择一种主题颜色

命令模式输入其中一个可以切换主题

```
colorscheme wombat
```

```
colorscheme molokai
```

## 多光标 vim-visual-multi

> 基本用法：
>
> - 选择带的单词（如在崇高的文本/VS 代码中）Ctrl-N`Ctrl-d`
>   - 输入多个`Ctrl N` 选择多个单词， 可以通过[]来移动光标选着, 通过Q取消一个光标
> - 使用Ctrl-Down/Ctrl-Up
> - 一次选择一个字符Shift-Arrows
> - 按 /获取下一个/上一次匹配nN
> - 按/选择下一个/上一个光标[ ]
> - 按跳过当前并获取下一次匹配q
> - 按以删除当前光标/选择Q
>
> 两种主要模式：
>
> - 在*光标模式下*，命令的工作方式与在正常模式下一样
> - 在*扩展模式下*，命令的工作方式与在可视模式下一样
> - 按下可切换 [光标] 和 [扩展] 模式Tab
>
> 大多数 vim 命令都正常工作（运动、替换字符、更改大小写等）。此外，您还可以：r~
>
> - 在游标上运行宏/ex/正常命令
> - 对齐光标
> - 转置选择
> - 使用正则表达式或从可视模式添加模式

## 联想

> `ins-completion`

### 在当前文件获取整行

快捷键：`<C-X><C-L>`

### 文件名

快捷键：`<C-X><C-F>`

### 当前文件中的关键字

快捷键：`<C-X><C-P>`或`<C-X><C-N>`

### `complete`选项所指定的范围中的关键字

快捷键：`<C-N>`或`<C-P>`

### `dictionary`中的关键字

快捷键：`<C-X><C-K>`

假设你有一个`javascript.dict`文件，其内容如下：

```
Promise
Proxy
let
class
extends
```

设置`:set dictionary+=/path/to/es6.dict`之后就可以用`<C-X><C-K>`来补全这些关键字了。

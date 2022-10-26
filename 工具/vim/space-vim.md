---
title: space-vim
date: 2020-12-18 10:02:21
tags: 
- vim
- nvim
---

# space-vim

> 官网 : <https://spacevim.org/cn/use-vim-as-ide/>

## windows配置

> Windows 下最快捷的安装方法是下载安装脚本 [install.cmd](https://spacevim.org/cn/install.cmd) 并运行。
>
> SpaceVim 的默认配置文件为 `~/.SpaceVim.d/init.toml`。下面为一简单的配置示例。 如果需要查阅更多 SpaceVim 配置相关的信息，请阅读 SpaceVim 用户文档。

```toml
# 这是一个基础的 SpaceVim 配置示例

# 所有的 SpaceVim 选项都列在 [options] 之下
[options]
    # 设置 SpaceVim 主题及背景，默认的主题是 gruvbox，如果你需要使用更
    # 多的主题，你可以载入 colorscheme 模块
    colorscheme = "gruvbox"
    # 背景可以取值 "dark" 或 "light"
    colorscheme_bg = "dark"
    # 启用/禁用终端真色，在目前大多数终端下都是支持真色的，当然也有
    # 一小部分终端不支持真色，如果你的 SpaceVim 颜色看上去比较怪异
    # 可以禁用终端真色，将下面的值设为 false
    enable_guicolors = true
    # 设置状态栏上分割符号形状，如果字体安装失败，可以将值设为 "nil" 以
    # 禁用分割符号，默认为箭头 "arrow"
    statusline_separator = "nil"
    statusline_iseparator = "bar"
    # 设置顶部标签列表序号类型，有以下五种类型，分别是 0 - 4
    # 0: 1 ➛ ➊
    # 1: 1 ➛ ➀
    # 2: 1 ➛ ⓵
    # 3: 1 ➛ ¹
    # 4: 1 ➛ 1
    buffer_index_type = 4
    # 显示/隐藏顶部标签栏上的文件类型图标，这一图标需要安装 nerd fonts，
    # 如果未能成功安装这一字体，可以隐藏图标
    enable_tabline_filetype_icon = true
    # 是否在状态栏上显示当前模式，默认情况下，不显示 Normal/Insert 等
    # 字样，只以颜色区分当前模式
    enable_statusline_mode = false

# SpaceVim 模块设置，主要包括启用/禁用模块

# 启用 autocomplete 模块，启用模块时，可以列出一些模块选项，并赋值，
# 关于模块的选项，请阅读各个模块的文档
[[layers]]
    name = "autocomplete"
    auto-completion-return-key-behavior = "complete"
    auto-completion-tab-key-behavior = "cycle"

# 禁用 shell 模块，禁用模块时，需要加入 enable = false
[[layers]]
    name = "shell"
    enable = false

# 添加自定义插件
[[custom_plugins]]
    repo = "lilydjwg/colorizer"
    merged = false
```

## 常用快捷键

> SPC : 空格
>
> Leader : \
>
> `:term` : 打开终端
>
> `spc f S` : 保存所有文件

| 快捷键           | 描述                                 | 快捷键               | 描述                                         |
| ---------------- | ------------------------------------ | -------------------- | -------------------------------------------- |
| `SPC + [number]` | 跳转到各个对应窗口编号的窗口         | `Leader + [number ]` | 通过该快捷键对文件进行标记, 并跳转到对应文件 |
| `SPC + b f`      | 代码格式化                           | `SPC + Tap`         | 切换到文件目录                               |
| `g r`           | 跳转到前一个Tab, 用于两个tab来回切换 |

## 窗口常用操作

> 窗口切换:
>
> - 小窗口多文件切换 : `\ [ number ]`
> - 打开
>   - 大窗口切换打开: `SPC + [ number ]`
>   - 分屏打开 : `sg`
>   - 垂直分屏打开 : `sv`
> - 关闭一个窗口 `spc w d`
> - 关闭一个文件 `spc b d`
>
> 文件树操作:
>
> - 切换文件树: `spc f t`
>
> - 打开文件/目录: 移动光标到指定的文件 + 回车
> - 打开目录:
>   - 关闭 `h`
>   - 进入`L`

## 文件树中打开

> 如果只有一个可编辑窗口，则在该窗口中打开选择的文件，否则则需要指定窗口来打开文件

| 快捷键          | 功能描述         |
| :-------------- | :--------------- |
| `l` / `<Enter>` | 打开文件         |
| `sg`            | 分屏打开文件     |
| `sv`            | 垂直分屏打开文件 |

## 基本操作

| 快捷键            | 功能描述                       |
| :---------------- | :----------------------------- |
| `<Up>`            | 向上移动光标，不跳过折行       |
| `<Down>`          | 向下移动光标，不跳过折行       |
| `H`               | 移动光标至屏幕顶部             |
| `L`               | 移动光标至屏幕底部             |
| `<`               | 向左移动文本                   |
| `>`               | 向右移动文本                   |
| `}`               | 向前移动一个段落               |
| `{`               | 向后移动一个段落               |
| `Ctrl-f`          | 向下翻页 (`Ctrl-f` / `Ctrl-d`) |
| `Ctrl-b`          | 向上翻页 (`C-b` / `C-u`)       |
| `Ctrl-e`          | 向下滚屏 (`3 Ctrl-e/j`)        |
| `Ctrl-y`          | 向上滚屏 (`3Ctrl-y/k`)         |
| `Ctrl-Shift-Up`   | 向上移动当前行                 |
| `Ctrl-Shift-Down` | 向下移动当前行                 |

## 他人笔记

# 自动更新

  [options]
automatic_update = true

# 启动主题

  [options]
colorscheme = "molokai"

  [[layers]]
name = "colorscheme"

# 更新插件

  :SPUpdate

# 查看日志

  :SPDebugInfo!

# 界面元素切换

## SPC t

  SPC t 8 高亮所有超过 80 列的字符
SPC t f 高亮临界列
SPC t hh 高亮当前行
SPC t hi 高亮代码对其线
SPC t hc 高亮光标所在列
SPC t hs 启动语法高亮
SPC t n 显示隐藏行号
SPC t b 切换背景色
SPC t c 切换 conceal 模式
SPC t p 切换 paste 模式
SPC t t Tab 管理器

  SPC t mb 电池
SPC t mM 文件类型
SPC t mt 时间
SPC t md 日期
SPC t mT 状态栏
SPC t mv 版本控制

  SPC t 8 高亮指定列后的所有字符
SPC t f 高亮指定列字符
SPC t s 语法检查
SPC t S 拼写检查

## SPC T

  SPC T ~ 显示/隐藏 Buffer 结尾/行首的~
SPC T f 显示隐藏边框
SPC T m 显示隐藏菜单
SPC T t 显示隐藏工具栏

# 切换 window

  SPC [1-9]

# 切换 tag

  \ [1-9]
g r 跳前一个 tag

# 搜索计数

```
1[layers]
2    name = "incsearch"
```

# 状态栏分隔符

```
1[options]
2statusline_separator = 'arrow'
```

# 标签管理器

  o 展开/关闭标签目录
r 重命名标签
n 新建标签
N 新建匿名标签
x 删除标签
enter 跳转到标签

# 文件树

  F3
SPC f t
SPC f T

  N 新建文件
yy 复制文件路径
yY 复制文件
p 粘贴
. 显示隐藏
sv 分屏
sg 分屏
p 预览文件
i 文件修改历史
v 快速查看

> < 放大缩小
> g x 使用相关程序执行
> ' 标记
> V 清除所有标记
> Ctrl+r 刷新

# 光标

  jkjh
H 屏幕顶部
L 屏幕底部

> 右移动文本
> < 左移动文本
> } 后移动段落
> { 前移动段落
> ctrl + f/d 向下翻页
> ctrl + b/u 向上翻页
> ctrl + e/j 向下滚屏
> ctrl + y/k 向上滚屏
> ctrl + c 复制绝对路径
> ctrl + x 切换窗口文件
> ctrl + shift + up 向上移动当前行
> ctrl + shift + down 向下移动当前行

# SPC x a

  SPC x a 各种对其
SPC x j 各种对其

# SPC x

  SPC x c 统计单词
SPC x d w 删除空白
SPC x g t 翻译
SPC x u 小写
SPC x U 大写
SPC x tc 字符前提
SPC x tC 字符后提

# 文本插入

  SPC i ll list
SPC i lp paragh
SPC i ls sentence
SPC i p1 password
SPC i p2 password2
SPC i p3 password3
SPC i pp easy password
SPC i pn numerical password
SPC i U UUID
SPC n +/- 数字加减

# 注释

  SPC ; 进入注释操作模式
SPC c h 隐藏/显示注释
SPC c l 注释/反注释当前行
SPC c L 注释行
SPC c u 反注释行
SPC c p 注释/反注释段落
SPC c P 注释段落
SPC c s 使用完美格式注释
SPC c t 注释/反注释到行
SPC c T 注释到行
SPC c y 注释/反注释同时复制(TODO)
SPC c Y 复制到未命名寄存器后注释
SPC c $ 从光标位置开始注释当前行

# 语法树

  F2

# WINDOW

  SPC w = window
SPC w . 启用窗口临时快捷键
SPC w 在同一标签内进行窗口切换
SPC w = 对齐分离的窗口
SPC w c 进入阅读模式，浏览当前窗口 (需要 tools 模块)
SPC w C 选择某一个窗口，并且进入阅读模式 (需要 tools 模块)
SPC w d 删除一个窗口
SPC w D 选择一个窗口并关闭
SPC w F 新建一个新的标签页
SPC w h 移至左边窗口
SPC w H 将窗口向左移动
SPC w j 移至下方窗口
SPC w J 将窗口向下移动
SPC w k 移至上方窗口
SPC w K 将窗口向上移动
SPC w l 移至右方窗口
SPC w L 将窗口向右移动
SPC w m 最大化/最小化窗口（最大化相当于关闭其它窗口）
SPC w M 选择窗口进行替换
SPC w o 按序切换标签页
SPC w p m 使用弹窗打开消息
SPC w p p 关闭当前弹窗窗口
SPC w r 顺序切换窗口
SPC w R 逆序切换窗口
SPC w s/- 水平分割窗口
SPC w S 水平分割窗口，并切换至新窗口
SPC w u 恢复窗口布局
SPC w U 撤销恢复窗口布局
SPC w v// 垂直分离窗口
SPC w V 垂直分离窗口，并切换至新窗口
SPC w w 切换至前一窗口
SPC w W 选择一个窗口

# 缓冲区

  SPC 切换至前一缓冲区，常用于两个缓冲区来回切换
SPC b . 启用缓冲区临时快捷键
SPC b b 通过模糊搜索工具进行缓冲区切换，需要启用一个模糊搜索工具模块
SPC b d 删除当前缓冲区，但保留编辑窗口
SPC b D 选择一个窗口，并删除其缓冲区
SPC b c 删除其它已保存的缓冲区
SPC b C-d 删除其它所有缓冲区
SPC b e 清除当前缓冲区内容，需要手动确认
SPC b h 打开欢迎界面， 等同于快捷键 SPC a s
SPC b n 切换至下一个缓冲区，排除特殊插件的缓冲区
SPC b m 打开消息缓冲区
SPC b p 切换至前一个缓冲区，排除特殊插件的缓冲区
SPC b P 使用系统剪切板内容替换当前缓冲区
SPC b R 从磁盘重新读取当前缓冲区所对应的文件
SPC b w 切换只读权限
SPC b Y 将整个缓冲区复制到系统剪切板
SPC b N h 在左侧新建一个窗口，并在其中新建空白 buffer
SPC b N j 在下方新建一个窗口，并在其中新建空白 buffer
SPC b N k 在上方新建一个窗口，并在其中新建空白 buffer
SPC b N l 在右侧新建一个窗口，并在其中新建空白 buffer
SPC b N n 在当前窗口新建一个空白 buffer

# 文件操作

  SPC f / 使用 find 命令查找文件，支持参数提示
SPC f b 跳至文件书签
SPC f C d 修改文件编码 unix -> dos
SPC f C u 修改文件编码 dos -> unix
SPC f D 删除文件以及 buffer，需要手动确认
SPC f f 打开文件
SPC f F 打开光标下的文件
SPC f o 代开文件树，并定位到当前文件
SPC f R rename the current file(TODO)
SPC f s / Ctrl-s 保存文件 (:w)
SPC f W 使用管理员模式保存
SPC f S 保存所有文件
SPC f r 打开文件历史
SPC f t 切换侧栏文件树
SPC f T 打开文件树侧栏
SPC f d Windows 下显示/隐藏磁盘管理器
SPC f y 复制并显示当前文件的绝对路径

# SPACE VIM

  SPC f v v 复制并显示当前 SpaceVim 的版本
SPC f v d 打开 SpaceVim 的用户配置文件

# 错误处理

  SPC t s 切换语法检查器
SPC e c 清除所有错误
SPC e h describe a syntax checker
SPC e l 切换显示错误/警告列表
SPC e n 跳至下一错误
SPC e p 跳至上一个错误
SPC e v 验证语法检查器设置
SPC e . 错误暂态

# quickfix

   q l 打开 quickfix 列表窗口
q c 清除 quickfix 列表
q n 跳到 quickfix 列表中下一个位置
q p 跳到 quickfix 列表中上一个位置

# 跳动到定义代码块

```
1g d
```

# 关闭当前 buffer

```
1Space b d
```

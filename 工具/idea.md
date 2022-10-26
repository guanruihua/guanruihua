---
title: idea
date: 2021-01-13 14:44:09
tags:
- idea
- tools
---

# idea

## 快捷键

| 快捷键           | 输出                                            |
| ---------------- | ----------------------------------------------- |
| psvm             | main  S函数                                     |
| fori             | for循环                                         |
| sout             | `System.out.println('')`                        |
| **Ctrl+P**       | **显示方法参数**                                |
| **Ctrl+W**       | **选择代码块，连续按会增加选择外层的代码块**    |
| **Ctrl+Shift+W** | **与“Ctrl+W”相反，减少选择代码块**              |
| **Ctrl+Alt+L**   | **格式化代码**                                  |
| **Ctrl+Alt+I**   | **自动优化代码缩进**                            |
| **Ctrl+[ \| ]**  | **光标移动到代码块的起始 \| 结束 位置**         |
| `Ctrl+alt+T`     | 选中的代码会被包含进要执行try…catch或其他语句中 |
| `ctrl+shift+u`   | 大小写转换                                      |

## idea-vim

### 安裝ideaVim插件

> setting/plugins : 在idea中直接搜索插件 `ideaVim` 并安装

#### 配置vim

在 `/home/`目录下创建 `.ideavimrc`文件，如在 windows 系统中，则在 `C:\Users\xxxx\`下建

在 `.ideavimrc` 中:

```shell
" Vim 的默认寄存器和系统剪贴板共享
set clipboard+=unnamed

" leader: ,  快捷键前缀
let mapleader = ","

" 基础键位映射
" esc 使用jk
"inoremap jk <esc>

" 映射idea常用快捷键
" 重命名
nnoremap <leader>r :action RenameElement<CR>
" 格式化当前文件
nnoremap <leader>f :action ReformatCode<CR><esc>
" quick java doc
nnoremap <leader>q :action QuickJavaDoc<CR>

" 映射到idea快捷键
" 弹出输入框，可以跳到指定类
nnoremap <Space>gc :action GotoClass<CR>
" 弹出输入框，跳转到指定操作
nnoremap <Space>ga :action GotoAction<CR>

" 跳转到实现
nnoremap <Space>gi :action GotoImplementation<CR>
" 跳转到指定的文件
nnoremap <Space>gf :action GotoFile<CR>
" 跳转到方法的声明
nnoremap <Space>gs :action GotoSuperMethod<CR>
" 跳转到测试
" nnoremap <Space>gt :action GotoTest<CR>
" 跳转到变量的声明
nnoremap <Space>gS :action GotoSymbol<CR>

" 查找使用
nnoremap <Space>fu :action FindUsages<CR>
" 显示使用
nnoremap <Space>su :action ShowUsages<CR>

" 前进，相当似于eclipse中的alt+方向右键
" nnoremap gf :action Forward<CR>
" 后退，相当于eclipse中的alt+方向左键
" nnoremap gb :action Back<CR>

" 插件模拟
" surround
set surround
" multiple-cursors
set multiple-cursors
" easyMotion 模拟，额外依赖插件：AceJump,IdeaVim-EasyMotion
set easymotion
" 注释插件模拟
set commentary
```

| 快捷键   | 中文                                                                            |
| -------- | ------------------------------------------------------------------------------- |
| ci"      | change inside “,找到最近”“的位置，并删除”"中内容                                |
| ci(、ci[ | change inside (， change inside [                                               |
| cit      | cat in tag，可以直接编辑匹配的xml标签中的内容！                                 |
| yi"      | yank inside " ,复制""中的所有内容                                               |
| ya"      | yank around " 复制整个字符串，包括双引号。                                      |
| ysiw’    | 为光标下的单词包围上单引号,同理 ysiw"                                           |
| cs’"     | 将单引号变成双引号,change surrounding ’ replace “”                              |
| cs"(     | 将双引号变成圆括号’                                                             |
| gUw      | 大写                                                                            |
| guw      | 小写                                                                            |
| gc       | 打开或关闭注释. 输入 gcc 打开或关闭某一行代码注释， gc2j 打开或关闭两行代码注释 |
| gC       | 块代码注释.输入 gCi) 注释 括号()中的代码                                        |
| gd       | Go to definition, 跳转到定义                                                    |
| gb       | 找出与光标下相同的下一个单词, 并添加一个光标 ，接下来就可以同时修改             |
| af       | VISUAL 模式命令, 依据语法分析, 将选择区域向外扩展                               |
| gh       | 等同于将鼠标移至光标所在单词, 方便查看定义以及报错                              |
| gt       | 下一个文件                                                                      |
| gT       | 上一个文件                                                                      |

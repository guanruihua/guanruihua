---
title: vim
date: 2021-3-10 20:55:21
tags:
- vim
- configure
---

# vim 配置



## init.vim

```
call plug#begin('~/AppData/Local/nvim/plugged')
Plug 'scrooloose/nerdtree' "文件树
Plug 'flazz/vim-colorschemes' " 主题
Plug 'jiangmiao/auto-pairs' "成对插入符号
Plug 'pangloss/vim-javascript' " 代码高亮与显示缩进
Plug 'tpope/vim-commentary' " 备注
Plug 'mxw/vim-jsx' "jsx语法包
Plug 'ap/vim-css-color' "css颜色显示
"Plug 'valloric/youcompleteme' "代码提示
Plug 'mattn/emmet-vim' "HTML补全
Plug 'posva/vim-vue' "vue代码高亮
Plug 'scrooloose/syntastic' "vue语法检查
Plug 'mxw/vim-jsx' "jsx代码高亮
Plug 'neoclide/coc.nvim', {'branch': 'release'} " 代码提示
Plug 'majutsushi/tagbar' "预览代码结构
Plug 'machakann/vim-sandwich' "环绕字符编辑(sandwich)

"Plug 'marijnh/tern_for_vim' "代码提示插件2, 和上一个一起使用
"Plug 'moll/vim-node' "node的相关插件
"Plug 'liuchengxu/vista.vim' " 
"Plug 'tpope/vim-surround' " 添加、更改和删除标签
call plug#end()

"*********************************************************************************
" 代码提示 coc.nvim 
let g:coc_global_extensions = ['coc-json','coc-css','coc-html']

"*********************************************************************************
"分割窗口
nnoremap <C-J> <C-W><C-J>
nnoremap <C-K> <C-W><C-K>
nnoremap <C-L> <C-W><C-L>
nnoremap <C-H> <C-W><C-H>


"定义快捷键
map <F8> :NERDTreeToggle<CR> "文件树折叠
nmap <F9> :TagbarToggle<CR> "预览代码结构
nmap <C-Z> <CR> " 去掉这个快捷键, 会导致我的机器卡死
nmap lj   ^  "行首
nmap lk   $  "行末

"文件保存与退出  
nmap fw     :w!<CR>  "强制保存
nmap fq     :q!<CR>  "强制保存与退出
nmap fwq    :wq!<CR> "强制保存与退出

"代码折叠
set foldmethod=indent  "indent 依缩进折叠 ；manual 手动折叠 ； marker 依标志折叠; syntax 依语法折叠; expr 依表达式折叠; diff 折叠未被改动的行"
set foldlevel=3
nnoremap <space> za
let g:SimpylFold_docstring_preview=1
let NERDTreeMapOpenInTab='<ENTER>' 

" 显示制图符
set listchars=tab:>-

"文件树
"自动打开
autocmd vimenter * NERDTree
autocmd VimEnter * wincmd p
"vim设置
"*****************************************************************************
" VUE & React 的配置
au BufNewFile,BufRead *.html,*.js,*.vue set tabstop=2
au BufNewFile,BufRead *.html,*.js,*.vue set softtabstop=2
au BufNewFile,BufRead *.html,*.js,*.vue set shiftwidth=2
au BufNewFile,BufRead *.html,*.js,*.vue set expandtab
au BufNewFile,BufRead *.html,*.js,*.vue set autoindent
au BufNewFile,BufRead *.html,*.js,*.vue set fileformat=unix
let g:syntastic_javascript_checkers = ['eslint'] "配置eslint检查器
let g:jsx_ext_required = 0 "允许在普通JS文件中使用JSX
"*****************************************************************************
colorscheme molokai "主题

set foldenable
set fdm=manual "开启代码折叠

" 添加js提示文件(补全)
set dictionary+=C:/Users/ruihuag/AppData/Local/nvim/javascript.dict

let g:ycm_min_num_of_chars_for_completion = 3 
let g:ycm_autoclose_preview_window_after_completion=1
let g:ycm_complete_in_comments = 1
let g:ycm_key_list_select_completion = ['<c-n>', '<Down>']
let g:ycm_key_list_previous_completion = ['<c-p>', '<Up>']
" 比较喜欢用tab来选择补全...
"function! MyTabFunction ()
"    let line = getline('.')
"    let substr = strpart(line, -1, col('.')+1)
"    let substr = matchstr(substr, "[^ \t]*$")
"    if strlen(substr) == 0
"        return "\<tab>"
"    endif
"    return pumvisible() ? "\<c-n>" : "\<c-x>\<c-o>"
"endfunction
"inoremap <tab> <c-r>=MyTabFunction()<cr>


"****************************************************************************
" tap页切换
" alt + [number] : 切换tap

:nn <M-1> 1gt
:nn <M-2> 2gt
:nn <M-3> 3gt
:nn <M-4> 4gt
:nn <M-5> 5gt
:nn <M-6> 6gt
:nn <M-7> 7gt
:nn <M-8> 8gt
:nn <M-9> 9gt
:nn <M-0> :tablast<CR>

"****************************************************************************

" jsx语法包
let g:jsx_ext_required = 1
let g:jsx_pragma_required = 1

let NERDTreeShowHidden=1 "显示隐藏文件

set tabstop=2
set softtabstop=4
set shiftwidth=4
set expandtab
set autoindent
set fileencodings=utf-8,ucs-bom,gb18030,gbk,gb2312,cp936
set termencoding=utf-8
set encoding=utf-8
set nu
set relativenumber							"开启相对行数
set clipboard=unnamed            "系统剪贴板
let mapleader=';'                "定义leader
set backspace=indent,eol,start   "backspace不能用时设置
set noswapfile 
set clipboard=unnamed            "设置系统复制粘贴板
set hlsearch                     "搜索关键字高亮
set incsearch                    "输入搜索命令立即显示
set encoding=utf-8
set wrap                         "主动换行
set autoread 
set autowriteall
" 让配置变更立即生效  
autocmd BufWritePost $MYVIMRC source $MYVIMRC  

"定义高亮
highlight BadWhitespace guifg=gray guibg=red ctermfg=gray ctermbg=red
set nobackup                      "关闭.swp文件
set nowritebackup
"au InsertLeave *.go, *.sh, *.py write   "退出插入模式指定类型的文件自动保存
"au InsertLeave *.js, *.json write
"****************************************************************************
```



## javascript.dict

```
# a
# b
# c
const
console
class

# d 
# e
export
extends

# f
function

# g
# h 
# i 
info

# j 
# k 
# l 
log
let

# m
module

# n 
node
# o 
# p 
Promise
Proxy

# q
# r 
React
require()

# s 
# t
# u 
# v 
var
Vue

# w 
# x
# y 
# z 
```


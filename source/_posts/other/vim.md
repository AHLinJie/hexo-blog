---
layout: post
title: Vim配置及插件
date: 2015-08-09
category: Edit
tag: vim
---

**摘要:**
我喜欢vim了,现在!
不是我朝三暮四哈,确实是vim很好用,现在又有很多的插件可以使用,还有结合github的包
管理工具,简直是太人性化了已经.因为之前插件的问题一直没有使用的好,一直没有用.
我都觉得是个遗憾了, 不过学习总是不迟的,从现在开始我就尽量使用vim来写代码和文档了
加油!



工具及插件：
https://github.com/tpope/vim-pathogen  # vim 插件管理工具
https://github.com/fatih/vim-go  # go语言高亮插件
https://github.com/rking/ag.vim  # 搜索插件

https://github.com/majutsushi/tagbar.git  # tagbar插件
sudo apt-get install exuberant-ctags  # 系统要安装ctags 注意版本
go get -u github.com/jstemmer/gotags # 让vim支持golang的tagbar

还有gocode代补全插件 这里暂时不使用

https://github.com/vim-scripts/The-NERD-tree.git # 目录插件





快捷键：

1. ctrl+w  后 hjkl选则窗口
2. ctrl+w  后 c 
3. ctrl+f   向前翻页
4. ctrl+b  向后翻页
5. z 重画页面
6. r  在NERDTree页面按r键刷新目录显示新创建的文件


6. ctrl+v   后 jk选择行  I  后 输入注释内容  后 esc     #  多行注释
7. ctrl+v  后 I  左右移动要注释的字符串  jk上下选中取消注释的行   后   d   删除注释    # 取消多行注释 
8. d$  光标到行尾剪切  d^ 光标到行首剪切
9. 10, 20 s/A/B  # 多行替换,10行到20行 替换A为B
10. ctrl + r  # 反撤销

配置文件：
if has("gui_running")
  set lines=999 columns=999
endif

"可以在buffer的任何地方使用鼠标
set mouse=a
set selection=exclusive
set selectmode=mouse,key

"设置编码
set encoding=utf-8
set fencs=utf-8,ucs-bom,shift-jis,gb18030,gbk,gb2312,cp936
set fileencodings=utf-8,ucs-bom,chinese
 
"语言设置
set langmenu=zh_CN.UTF-8
 
"设置语法高亮
syntax enable
syntax on

"高亮当前行
set cursorline
hi CursorLine  cterm=NONE   ctermbg=darkred ctermfg=white
hi CursorColumn cterm=NONE ctermbg=darkred ctermfg=white

"80列提示
au BufWinEnter * let w:m2=matchadd('Underlined', '\%>' . 80 . 'v.\+', -1)


"启用状态栏信息
set laststatus=2
"设置命令行的高度为2，默认为1
set cmdheight=2

" 把空格键映射成:
nmap <space> :
"设置配色方案
colorscheme torte

"改变输入状态光标的颜色为白色
hi Cursor guibg=white
set cursorline
hi CursorLine guifg=red guibg=blue
set cursorcolumn
set wildmenu

"高亮显示匹配的括号
set showmatch
set number
set showcmd
set lazyredraw
 
"去掉vi一致性
set nocompatible
 
"设置缩进
set tabstop=4
set softtabstop=4
set shiftwidth=4
set expandtab
set autoindent
set cindent
if &term=="xterm"
    set t_Co=8
    set t_Sb=^[[4%dm
    set t_Sf=^[[3%dm
endif


"不要备份文件
set nobackup
set nowb
"不要生成swp文件（多人修改产生）
set noswapfile

"python 语法校验
let g:pyflakes_use_quickfix = 0
filetype plugin indent on



" searching
set incsearch
set hlsearch

"folding 
set foldenable
set foldlevelstart=10
set foldnestmax=10
set foldmethod=indent

"movement
nnoremap B ^
nnoremap E $
nnoremap gV `[v`]


"vim-pathogen
execute pathogen#infect()
syntax on
filetype plugin indent on								 

set runtimepath^=~/

nmap <F8> :TagbarToggle<CR>





"nerd tree config
autocmd vimenter * NERDTree
autocmd StdinReadPre * let s:std_in=1
autocmd VimEnter * if argc() == 0 && !exists("s:std_in") | NERDTree | endif
map <F7> :NERDTreeToggle<CR>
autocmd bufenter * if (winnr("$") == 1 && exists("b:NERDTreeType") && b:NERDTreeType == "primary") | q | endif
let g:NERDTreeDirArrows = 1
let g:NERDTreeDirArrowExpandable = '▸'
let g:NERDTreeDirArrowCollapsible = '▾'
let NERDTreeIgnore = ['\.pyc$']

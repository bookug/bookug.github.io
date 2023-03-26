---
layout: post
title: Vim Configuration
date: 2020-05-28
categories: blog
tags: [Technical Blog]
description: Technique is the wing that help you fly.
---

## Build  and use docker-vim

Let's bring Vim anywhere!

https://www.jianshu.com/p/93972f0df163

An example can be pulled from `bookug/zvim`.

Or you can build one from Dockerfile below, please modify it as you wish.

```
FROM scratch
ADD centos-8-x86_64.tar.xz /

LABEL org.label-schema.schema-version="1.0" \
    org.label-schema.name="CentOS Base Image" \
    org.label-schema.vendor="CentOS" \
    org.label-schema.license="GPLv2" \
    org.label-schema.build-date="20200611"

CMD ["/bin/bash"]

ADD ./install.sh /usr/local
ADD ./vim /root/.vim
ADD ./Centos-8.repo /root/Centos-8.repo

#运行install.sh脚本进行实际的安装工作
RUN /usr/local/install.sh
```

The script `install.sh` is given below:

```
mv /etc/yum.repos.d/CentOS-Base.repo /etc/yum.repos.d/CentOS-Base.repo.bak
mv /root/Centos-8.repo /etc/yum.repos.d/CentOS-Base.repo
yum clean all
yum list
yum makecache

# 安装所有依赖的组件
yum install -y vim git curl ctags cscope python3-devel make cmake gcc gcc-c++ clang-devel
echo "-->download vundle to manage vim plugins..."

git clone https://github.com/VundleVim/Vundle.vim.git ~/.vim/bundle/Vundle.vim
# 安装vim插件
vim -c PluginInstall -c q -c q

# 安装插件运行需要依赖的一些组件
pip3 install autopep8
cd /root/.vim/bundle/YouCompleteMe/ && python3 install.py --clang-complete

```

---

## Install vim82 on Linux

pre-install x11-devel xtst-devel xt-devel sm-devel xpm-devel

```
https://www.cnblogs.com/JoiT/p/build_from_source_for_linux_vim.html
https://stackoverflow.com/questions/11416069/compile-vim-with-clipboard-and-xterm
https://www.cnblogs.com/shinemic/p/8409827.html
```

support system clipboard

```
https://www.cnblogs.com/jpfss/p/9040561.html
```

YouCompleteMe needs GLIBCXX 3.4.20 and CXXABI 1.3.9

```
https://blog.csdn.net/lyn_00/article/details/77984257
```

In order to solve this, we need to update GCC.                                                                     

```
https://bigsearcher.com/mirrors/gcc/releases/gcc-6.1.0/
https://blog.csdn.net/EI__Nino/article/details/100086157
```

We may need to yum install gmp-devel, libmpc-devel, mpfr-devel first or using below to compile install:

```
https://blog.csdn.net/lyn_00/article/details/77984257
```

maybe consider other variants like spacevim, coc.nvim

```
https://www.jianshu.com/p/249850f2cc64
```



## Configuration Steps

1. build `~/.vim/` folder, and add new configuration file `~/.vim/vimrc`.
2. Clone `Vundle` from GitHub to `~/.vim/bundle/`.
3. Add plugins in `vimrc`, and use `:PluginInstall` to install them.
4. More configurations of plugins can be added to `vimrc`.
5. place color schemes in `~/.vim/colors/`, syntax templates in `~/.vim/syntax/`, etc.


When writing essays, we can also open spell check (need dictionary) and grammar check (need LanguageTool).
There is many tutorials on the Internet.

snippets are very useful, self-defined snippets can be placed in `~/.vim/UltiSnips/`.

```
https://zhuanlan.zhihu.com/p/61036165
https://vimzijun.net/2016/10/30/ultisnip/
```

A template of `vimrc` is listed below.
Plugins are in `~/.vim/bundle/`, dictionary is in `~/.vim/dict/`, spell files (words added by user) are in `~/.vim/spell`, undo history is placed in `~/.vim/undo`.
Plugin `vim-grammarous` needs to download LanguageTool for grammar check, please find it on GitHub for details.

```
if has("win32")
    let $VIMFILES = $VIM.'/vimfiles'
else
    let $VIMFILES = $HOME.'/.vim'
endif

"http://blog.chinaunix.net/uid-26495963-id-3062657.html
"http://vim.spf13.com/

"https://vi.stackexchange.com/questions/281/how-can-i-find-out-what-leader-is-set-to-and-is-it-possible-to-remap-leader
"set <Leader>, \ by default
"
"gdefault
"https://codeday.me/bug/20181203/424482.html
":s/xxx/yyy/gg to substitute occurrences in a line, /g for one


" NOTICE: if we want to input the leader key itself, there is a latency.
" \ is frequently used in LaTex, ; is rarely used
let mapleader='\'
"let mapleader=';'



"""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""
" configure the vundle "
"""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""
set nocompatible  "去掉讨厌的有关vi一致性模式，避免以前版本的一些bug和局限
filetype off      " required

" set the runtime path to include Vundle and initialize
set rtp+=~/.vim/bundle/Vundle.vim
call vundle#begin()
" alternatively, pass a path where Vundle should install plugins
"call vundle#begin('~/some/path/here')

" let Vundle manage Vundle, required
Plugin 'VundleVim/Vundle.vim'

" TODO: doxygen vim
" https://blog.csdn.net/zistxym/article/details/7432693

"NOTICE: Below only works for fcitx input methods
"Plugin 'edkolev/tmuxline.vim'
"Plugin 'lilydjwg/fcitx.vim'
" The following are examples of different formats supported.
" Keep Plugin commands between vundle#begin/end.
" plugin on GitHub repo
" https://github.com/Chiel92/vim-autoformat
" code format: http://aiezu.com/article/linux_vim_plugin_autoformat_install.html
" https://www.cnblogs.com/lepeCoder/p/8032178.html
Plugin 'Chiel92/vim-autoformat'
Plugin 'rhysd/vim-grammarous'
"https://vimzijun.net/2017/02/12/color-scheme/
"Plugin 'jnurmine/Zenburn'
"Plugin 'whatyouhide/vim-gotham'
"Plugin 'chriskempson/base16-vim'
"Plugin 'whatyouhide/vim-gotham'
"Usage of vimmake: https://github.com/skywind3000/vimmake
"http://www.udpwork.com/item/15680.html
"Plugin 'skywind3000/vimmake'
"Plugin 'plasticboy/vim-markdown'
"Plugin 'davidhalter/jedi-vim'
"Plugin 'Lokaltog/vim-powerline'
Plugin 'vim-airline/vim-airline'
Plugin 'vim-airline/vim-airline-themes'
"Plugin 'powerline/powerline'
Plugin 'Xuyuanp/nerdtree-git-plugin'
"NOTICE: indentLine seems to conflict with tex snippets.
"Plugin 'Yggdroot/indentLine'
"Plugin 'vim-scripts/vimwiki'
"Plugin 'vim-scripts/DrawIt'
Plugin 'vim-scripts/cscope.vim'
" :help csupport
"Plugin 'vim-scripts/c.vim'
"Plugin 'vim-scripts/LanguageTool'
"http://studygolang.com/articles/1785
"Plugin 'fatih/vim-go'
"Plugin 'vim-scripts/slimv.vim'
"http://blog.sina.com.cn/s/blog_64e527a30100xasw.html
"Plugin 'ludovicchabant/vim-gutentags'
" http://www.wklken.me/posts/2015/06/07/vim-plugin-ctrlp.html
"Plugin 'kien/ctrlp.vim'
"this is for git symbol in powerline
Plugin 'tpope/vim-fugitive'
"Fold in Vim
"https://www.jianshu.com/p/16e0b822b682
"Plugin 'tmhedberg/VimFold4C'
"Plugin 'tmhedberg/SimpylFold'
"Plugin 'vim-scripts/indentpython.vim'
Plugin 'ervandew/supertab'
Plugin 'scrooloose/syntastic'
Plugin 'jiangmiao/auto-pairs'
Plugin 'scrooloose/nerdcommenter'
Plugin 'scrooloose/nerdtree'
Plugin 'mbbill/undotree'
Plugin 'lervag/vimtex'
"Plugin 'vim-latex/vim-latex'
" plugin from http://vim-scripts.org/vim/scripts.html
"Plugin 'L9'
"Plugin 'ctags.vim'
"https://github.com/vim-scripts/bash-support.vim
"Plugin 'vim-scripts/bash-support.vim'
Plugin 'rking/ag.vim'
" need to install the_silver_searcher first
"Plugin 'Chun-Yang/vim-action-ag'
" vimshell and vimproc on GitHub
Plugin 'honza/vim-snippets'
Plugin 'sirver/UltiSnips'
"Plugin 'garbas/vim-snipmate'
" https://github.com/sirver/UltiSnips
" https://github.com/terryma/vim-multiple-cursors
Plugin 'terryma/vim-multiple-cursors'
"this requires the version of vim higher than 7.4, and newer system (glibc
"3.4.20)
Plugin 'Valloric/YouCompleteMe'
"Plugin 'closetag'
"Plugin 'python'
"Plugin 'LoadHeaderFile.vim'
"Plugin 'a.vim'
"Plugin 'Triggers'
"Plugin 'perl'
"Plugin 'calendar'
"Plugin 'taglist.vim'
Plugin 'majutsushi/tagbar'
"Plugin 'methods'
"Plugin 'vim'
"Plugin 'Shell'
"Plugin 'email'
"Plugin 'glib'
"Plugin 'vimdoc'
"Plugin 'minibufexpl'
"Plugin 'gdbmgr'
"Plugin 'vim-abbrev-matcher'
" Git plugin not hosted on GitHub
""Plugin 'git://git.wincent.com/command-t.git'
" git repos on your local machine (i.e. when working on your own plugin)
" Plugin 'file:///home/gmarik/path/to/plugin'
" The sparkup vim script is in a subdirectory of this repo called vim.
" Pass the path to set the runtimepath properly.
""Plugin 'rstacruz/sparkup', {'rtp': 'vim/'}
" Avoid a name conflict with L9
""Plugin 'user/L9', {'name': 'newL9'}
"Plugin 'vim-scripts/vim-auto-save'

" All of your Plugins must be added before the following line
call vundle#end()            " required
filetype plugin indent on    " required
" To ignore plugin indent changes, instead use:
"filetype plugin on
"
" Brief help
" :PluginList       - lists configured plugins
" :PluginInstall    - installs plugins; append `!` to update or just :PluginUpdate
" :PluginSearch foo - searches for foo; append `!` to refresh local cache
" :PluginClean      - confirms removal of unused plugins; append `!` to auto-approve removal
"
" see :h vundle for more details or wiki for FAQ
" Put your non-Plugin stuff after this line











"""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""
" User configuration
"""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""
if has('gui_running')
    set background=light
else
    set background=dark
endif
colorscheme ron
"colorscheme molokai
"colorscheme solarized
"colorscheme jellybeans
"colorscheme vividchalk
"colorscheme wombat256
"colorscheme github
"colorscheme railscasts
"colorscheme twilight
"colorscheme candy
colorscheme bookug "my own color scheme
"vim own colors:  /usr/share/vim/vim74/colors/
"colorscheme desert
"colorscheme morning
"colorscheme evening
"a color scheme online editor: http://bytefluent.com/
"http://ethanschoonover.com/solarized
highlight NonText guibg=#060606
highlight Folded guibg=#0A0A0A guifg=#9090D0
"https://github.com/tomasr/molokai
""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""
" 显示相关
""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""
set go=             " 不要图形按钮
autocmd InsertLeave * se nocul  " 用浅色高亮当前行
autocmd InsertEnter * se cul    " 用浅色高亮当前行
set showcmd         " 输入的命令显示出来，看的清楚些
set novisualbell    " 不要闪烁(不明白)
" 显示中文帮助
if version >= 603
    set helplang=cn
    set encoding=utf-8
endif





"""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""
"""""新文件标题
""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""
"新建.c,.h,.sh,.java文件，自动插入文件头
"autocmd BufNewFile *.cpp, *.[ch], *.cu, *.java, *.sh, *.p[ly], *.php, *.lisp, *.ml, *.html exec ":call SetTitle()"
""定义函数SetTitle，自动插入文件头
"func! SetTitle()
"   if &filetype == 'sh' || &filetype == 'pl' || &filetype == 'py' || &filetype == 'php'
"       call setline(1, "\#########################################################################")
"       call append(line("."), "\# File Name: ".expand("%"))
"       call append(line(".")+1, "\# Author: bookug ")
"       call append(line(".")+2, "\# Mail: bookug@qq.com ")
"       call append(line(".")+3, "\# Last Modified: ".strftime("%c"))
"       call append(line(".")+4, "\#########################################################################")
"       ""call append(line(".")+5, "\#!/bin/bash")
"       call append(line(".")+5, "")
"   else if &filetype == 'lisp'
"       call setline(1, ";;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;")
"       call append(line("."), "; File Name: ".expand("%"))
"       call append(line(".")+1, "; Author: bookug ")
"       call append(line(".")+2, "; Mail: bookug@qq.com ")
"       call append(line(".")+3, "; Last Modified: ".strftime("%c"))
"       call append(line(".")+4, ";;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;")
"       call append(line(".")+5, "")
"   else if &filetype == 'ml'
"       call setline(1, "(*************************************************************************")
"       call append(line("."), "    > File Name: ".expand("%"))
"       call append(line(".")+1, "  > Author: bookug ")
"       call append(line(".")+2, "  > Mail: bookug@qq.com ")
"       call append(line(".")+3, "  > Last Modified: ".strftime("%c"))
"       call append(line(".")+4, " ************************************************************************)")
"       call append(line(".")+5, "")
"   else if &filetype == 'html'
"       call setline(1, "<!-----------------------------------------------------------------------")
"       call append(line("."), "    > File Nmae:")
"   else
"       call setline(1, "/*************************************************************************")
"       call append(line("."), "    > File Name: ".expand("%"))
"       call append(line(".")+1, "  > Author: bookug ")
"       call append(line(".")+2, "  > Mail: bookug@qq.com ")
"       call append(line(".")+3, "  > Last Modified: ".strftime("%c"))
"       call append(line(".")+4, " ************************************************************************/")
"       call append(line(".")+5, "")
"   endif
""  if &filetype == 'cpp'
""      call append(line(".")+6, "#include <iostream>")
""      call append(line(".")+7, "using namespace std;")
""      call append(line(".")+8, "")
""  endif
""  if &filetype == 'c'
""      call append(line(".")+6, "#include <stdio.h>")
""      call append(line(".")+7, "")
""  endif
"   if &filetype == 'java'
"       call append(line(".")+6,"public class ".expand("%"))
"       call append(line(".")+7,"")
"   endif
"新建文件后，自动定位到文件末尾
"   autocmd BufNewFile * normal G
"endfunc
""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""
"键盘命令
""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""
"nmap <leader>w :w!<cr>
"nmap <leader>f :find<cr>
""WARN:when running GUI, F1 is used for system help by default
"map <F12> gg=G
"remove empty lines
"nnoremap <F2> :g/^\s*$/d<CR>
nnoremap <leader>rel :g/^\s*$/d<CR>
"比较文件
"nnoremap <C-F2> :vert diffsplit
"新建标签
"map <M-F2> :tabnew<CR>
"列出当前目录文件
"map <F3> :tabnew .<CR>
"打开树状文件目录
"map <C-F3> \be
"C，C++ 按F5编译运行
"map <F5> :call CompileRunGcc()<CR>
"func! CompileRunGcc()
"   exec "w"
"   if &filetype == 'c'
"       exec "!g++ % -o %<"
"       exec "! ./%<"
"   elseif &filetype == 'cpp'
"       exec "!g++ % -o %<"
"       exec "! ./%<"
"   elseif &filetype == 'java'
"       exec "!javac %"
"       exec "!java %<"
"   elseif &filetype == 'sh'
"       :!./%
"   elseif &filetype == 'py'
"       exec "!python %"
"       exec "!python %<"
"   endif
"endfunc
"C,C++的调试
"map <F8> :call Rungdb()<CR>
"func! Rungdb()
"   exec "w"
"   exec "!g++ % -g -o %<"
"   exec "!gdb ./%<"
"endfunc
""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""
""实用设置
"""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""
" 设置当文件被改动时自动载入
set autoread

" quickfix模式
"autocmd FileType c,cpp map <buffer> <leader><space> :w<cr>:make<cr>
"set cscopequickfix=s-,c-,d-,i-,t-,e-
"nmap <C-n>:cnext<CR>
"nmap <C-p>:cprev<CR>

"代码补全
set completeopt=longest,menu
"set completeopt=preview,menu
"允许插件
filetype plugin on
"共享剪贴板
set clipboard+=unnamed
"从不备份
set nobackup
"make 运行
:set makeprg=g++\ -Wall\ \ %
"自动保存
"set autowrite
set ruler                   " 打开状态栏标尺
set cursorline              " 突出显示当前行
set cursorcolumn            " 突出显示当前列
set magic                   " 设置魔术
set guioptions-=T           " 隐藏工具栏
set guioptions-=m           " 隐藏菜单栏
"set foldenable      " 允许折叠
"set foldmethod=manual   " 手动折叠
""set foldcolumn=0
""set foldmethod=indent
""set foldlevel=3
""set foldenable              " 开始折叠
syntax enable
set syntax=on
set noeb
set confirm
set autoindent
set cindent

"TAB: https://blog.csdn.net/shell_picker/article/details/6033023
" this will replace tab with four spaces
" do not worry, in makefile it will still be tabs
set tabstop=4
set softtabstop=4
set shiftwidth=4
set noexpandtab
set smarttab

" 管理行号
"set number
" 启用相对行号
"set relativenumber
" tricks: 10j 4k >2j用于对当前行及以下2行缩进
" 绝对行号仍然可以使用，如843G或:843G<CR>
" Vim 的一个非常好的特性是，你可以设置折行功能。很多用户包括我会把 j/k
" 键重映射为gj/gk，使得按下它们时，光标按虚拟行而不是按物理行移动。
" 然而，这种重映射影响了前文提到的计数功能。为了弥补这一不足，基于这篇stackoverflow.com的博文，我们重新进行如下映射：
noremap <silent> <expr> j (v:count == 0 ? 'gj' : 'j')
noremap <silent> <expr> k (v:count == 0 ? 'gk' : 'k')
" 当遇到没有行号的行时，gj/gk
" 命令会使光标按虚拟行移动，而当遇到有行号的行时，光标则按物理行移动。

set ignorecase "ignore the case when searching
set history=1000
set nobackup
set noswapfile
set gdefault
set enc=utf-8
set fencs=utf-8,ucs-bom,shift-jis,gb18030,gbk,gb2312,cp936
set langmenu=zh_CN.UTF-8
set helplang=cn
" 带有如下符号的单词不要被换行分割
set iskeyword+=_,$,@,%,#,-
" 字符间插入的像素行数目
set linespace=0
" 增强模式中的命令行自动完成操作
set wildmenu
" 使回格键（backspace）正常处理indent, eol, start等
set backspace=2
" 允许backspace和光标键跨越行边界
"set whichwrap+=<,>,h,l
" 可以在buffer的任何地方使用鼠标（类似office中在工作区双击鼠标定位）
"set mouse=a
"set mouse=v
set selection=exclusive
set selectmode=mouse,key
" 通过使用: commands命令，告诉我们文件的哪一行被改变过
set report=0
" 在被分割的窗口间显示空白，便于阅读
set fillchars=vert:\ ,stl:\ ,stlnc:\
" 高亮显示匹配的括号
set showmatch
" 匹配括号高亮的时间（单位是十分之一秒）
set matchtime=1
" 光标移动到buffer的顶部和底部时保持3行距离
set scrolloff=3
" 为C程序提供自动缩进
set smartindent
" 高亮显示普通txt文件（需要txt.vim脚本）
au BufRead,BufNewFile *  setfiletype txt
"自动补全
"":inoremap < <><ESC>i
"":inoremap > <c-r>=ClosePair('>')<CR>
":inoremap ( ()<ESC>i
":inoremap ) <c-r>=ClosePair(')')<CR>
":inoremap { {<CR>}<ESC>O
":inoremap } <c-r>=ClosePair('}')<CR>
":inoremap [ []<ESC>i
":inoremap ] <c-r>=ClosePair(']')<CR>
":inoremap " ""<ESC>i
":inoremap ' ''<ESC>i
func! ClosePair(char)
    if getline('.')[col('.') - 1] == a:char
        return "\<Right>"
    else
        return a:char
    endif
endfunc
filetype plugin indent on
"打开文件类型检测, 加了这句才可以用智能补全
set completeopt=longest,menu
"""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""
"NERDtree设定
let NERDChristmasTree=1
let NERDTreeAutoCenter=1
"let NERDTreeBookmarksFile=$VIM.'\Data\NerdBookmarks.txt'
let NERDTreeMouseMode=3
let NERDTreeShowBookmarks=1
let NERDTreeShowFiles=1
let NERDTreeShowHidden=0
"let NERDTreeShowLineNumbers=1
let NERDTreeWinPos='left'
let NERDTreeWinSize=15
"let NERDTreeStatusline=1
"nnoremap f :NERDTreeToggle
"map <F7> :NERDTree<CR>
nmap <F7> :NERDTreeToggle<CR>
"let NERDTreeChDirMode=2  "choose root then set as the current dir
"let NERDTreeQuitOnOpen=1 "close the tree if file is open
"let NERDTreeMinimalUI=1 "not show the help panel
"let NERDTreeDirArrows=1 "dir arrow    1 show the arrow  0 general +-|

""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""
"taglist and ctags settings
"let Tlist_Show_One_File=1
"let Tlist_Exit_OnlyWindow=1
"let Tlist_Use_Right_Window=1
"let Tlist_Auto_Open=0
"let Tlist_WinWidth=15
"map <F6> :Tlist<CR>
"""let Tlist_Ctags_Cmd="usr/bin/ctags"  "TODO
"let Tlist_Inc_Winwidth=0

"tagbar is better for OO
let g:tagbar_width=15
let g:tagbar_autofocus=1
nmap <F6> :TagbarToggle<CR>
let g:tagbar_ctags_bin='/usr/bin/ctags'  " 设置ctags所在路径
"autocmd BufReadPost *.cpp,*.c,*.h,*.hpp,*.cc,*.cxx,*.cu,*.cuh call tagbar#autoopen()　" 在某些情况下自动打开tagbar"

"set paste      "format will be saved when pasting

map <F4> :call TitleDet()<cr>'s
func! AddTitle()
    call append(0,"/*=============================================================================")
    call append(1,"# Filename: ".expand("%:t"))
    call append(2,"# Author: bookug ")
    call append(3,"# Mail: bookug@qq.com")
    call append(4,"# Last Modified: ".strftime("%Y-%m-%d %H:%M"))
    call append(5,"# Description: ")
    call append(6,"=============================================================================*/")
    call append(7,"")
    echohl WarningMsg | echo "Successful in adding the copyright." | echohl None
endfunc
"更新最近修改时间和文件名
"WARN(this conflicts with auto-inserting)
func! UpdateTitle()
    normal m'
    execute '/# *Last Modified:/s@:.*$@\=strftime(":\t%Y-%m-%d %H:%M")@'
    normal ''
    normal mk
    execute '/# *Filename:/s@:.*$@\=":\t\t".expand("%:t")@'
    execute "noh"
    normal 'k
    echohl WarningMsg | echo "Successful in updating the copy right." | echohl None
endfunc
"判断前10行代码里面，是否有Last modified这个单词，
"如果没有的话，代表没有添加过作者信息，需要新添加；
"如果有的话，那么只需要更新即可
func! TitleDet()
    let n=1
    "默认为添加
    while n < 10
        let line = getline(n)
        if line =~ '^\#\s*\S*Last\sModified:\S*.*$'
            call UpdateTitle()
            return
        endif
        let n = n + 1
    endwhile
    call AddTitle()
endfunc

"au BufNewFile,BufRead *.py
            "\ set tabstop=4
            "\ set softtabstop=4
            "\ set shiftwidth=4
            "\ set textwidth=79
            "\ set expandtab
            "\ set autoindent
            "\ set fileformat=unix

au BufNewFile,BufRead *.js, *.html, *.css
            \ set tabstop=2
            \ set softtabstop=2
            \ set shiftwidth=2

"au BufRead,BufNewFile *.py,*.pyw,*.c,*.h match BadWhitespace /\s\+$/


"let g:SimpylFold_docstring_preview=1
let python_highlight_all=1
let NERDTreeIgnore=['\.pyc$', '\.o$', '\*.out$', '\~$'] "ignore files in NERDTree






"for cscope
"http://my.oschina.net/u/572632/blog/267471
"Ctrl+] jumps to the definnition of the function/variable where your cursor hovers; Ctrl+T returns
nnoremap <leader>fa :call cscope#findInteractive(expand('<cword>'))<CR>
nnoremap <leader>l :call ToggleLocationList()<CR>
"nnoremap <leader>fa :call cscope#findInteractive(expand('<cword>'))<CR>
"nnoremap <leader>l :call ToggleLocationList()<CR>
" s: Find this C symbol
nnoremap  <leader>fs :call cscope#find('s', expand('<cword>'))<CR>
" g: Find this definition
nnoremap  <leader>fg :call cscope#find('g', expand('<cword>'))<CR>
" d: Find functions called by this function
nnoremap  <leader>fd :call cscope#find('d', expand('<cword>'))<CR>
" c: Find functions calling this function
nnoremap  <leader>fc :call cscope#find('c', expand('<cword>'))<CR>
" t: Find this text string
nnoremap  <leader>ft :call cscope#find('t', expand('<cword>'))<CR>
" e: Find this egrep pattern
nnoremap  <leader>fe :call cscope#find('e', expand('<cword>'))<CR>
" f: Find this file
nnoremap  <leader>ff :call cscope#find('f', expand('<cword>'))<CR>
" i: Find files #including this file
nnoremap  <leader>fi :call cscope#find('i', expand('<cword>'))<CR>

"===============================================================================

"to use undo tree
nnoremap <F2> :UndotreeToggle<cr>
let g:undotree_WindowLayout = 1
"if has('persistent_undo')
set undofile         " Save undo's after file closes
set undodir=~/.vim/undo/
set undolevels=1000  " How many undos
set undoreload=10000 " number of lines to save for undo
"endif

"use undotree_debug.log to see debug info, if not, move it to
"undotree_debug.log.bak

"" Put plugins and dictionaries in this dir (also on Windows)
"let vimDir = '$HOME/.vim'
"let &runtimepath.=','.vimDir

"" Keep undo history across sessions by storing it in a file
"if has('persistent_undo')
"let myUndoDir = expand(vimDir . '/undodir')
"" Create dirs
"call system('mkdir ' . vimDir)
"call system('mkdir ' . myUndoDir)
"let &undodir = myUndoDir
"set undofile
"endif
"
"silent call system('mkdir -p ' . &undodir)

"===============================================================================



" configure syntastic syntax checking to check on open as well as save
" Quickfix settings
let g:syntastic_check_on_open=1
let g:syntastic_html_tidy_ignore_errors=[" proprietary attribute \"ng-"]
let g:syntastic_always_populate_loc_list = 1
let g:syntastic_auto_loc_list = 1
let g:syntastic_check_on_wq = 0
set statusline+=%#warningmsg#
set statusline+=%{SyntasticStatuslineFlag()}
set statusline+=%*

" ctrlp
"set wildignore+=*/tmp/*,*.so,*.swp,*.zip,*.png,*.jpg,*.jpeg,*.gif " MacOSX/Linux
"let g:ctrlp_custom_ignore = '\v[\/]\.(git|hg|svn)$'
"if executable('ag')
"    " Use Ag over Grep
"    set grepprg=ag\ --nogroup\ --nocolor
"    " Use ag in CtrlP for listing files.
"    let g:ctrlp_user_command = 'ag %s -l --nocolor -g ""'
"    " Ag is fast enough that CtrlP doesn't need to cache
"    let g:ctrlp_use_caching = 0
"endif
"let g:ctrlp_map = '<c-p>'
"let g:ctrlp_cmd = 'CtrlP'
"let g:ctrlp_open_multiple_files = 'v'         " <C-Z><C-O>时垂直分屏打开多个文件
""let g:ctrlp_custom_ignore = {
""  \ 'dir':  '\v[\/]\.(git)$',
""  \ 'file': '\v\.(log|jpg|png|jpeg)$',
""  \ }
"let g:ctrlp_working_path_mode= 'ra'
"let g:ctrlp_match_window_bottom= 1
"let g:ctrlp_max_height= 10
"let g:ctrlp_match_window_reversed=0
"let g:ctrlp_mruf_max=500
"let g:ctrlp_follow_symlinks=1


"vim-powerline
"let g:Powerline_symbols='fancy'
"set laststatus=2 " Always display the status line
"set statusline+=%{fugitive#statusline()} "  Git Hotness
""set statusline=%F%m%r%h%w\ [FORMAT=%{&ff}]\ [TYPE=%Y]\ [POS=%l,%v][%p%%]\ %{strftime(\"%d/%m/%y\ -\ %H:%M\")}   "状态行显示的内容
""set laststatus=1    " 启动显示状态行(1),总是显示状态行(2)
"set noshowmode
"set t_Co=256
"set cmdheight=1
"set viminfo+=!

" vim airline
let g:airline#extensions#tabline#enabled = 1
let g:airline#extensions#tabline#left_sep = ' '
let g:airline#extensions#tabline#left_alt_sep = '|'
let g:airline#extensions#tabline#formatter = 'default'

" nerdtree-git-plugin
" How to show ignored status?
"let g:NERDTreeShowIgnoredStatus = 1    "(a heavy feature may cost much more time)
let g:NERDTreeIndicatorMapCustom = {
    \ "Modified"  : "✹",
    \ "Staged"    : "✚",
    \ "Untracked" : "✭",
    \ "Renamed"   : "➜",
    \ "Unmerged"  : "═",
    \ "Deleted"   : "✖",
    \ "Dirty"     : "✗",
    \ "Clean"     : "✔︎",
    \ 'Ignored'   : '☒',
    \ "Unknown"   : "?"
    \ }


" indentLine, see GitHub for more details.
"let g:indentLine_setColors = 0


"to use nerdcommenter
"n\cc, \ca, \cu, \cA
"if \ is the mapleader


" window size when opening vim
"set lines=35 columns=118

"https://wiki.archlinux.org/index.php/Vim_(%E7%AE%80%E4%BD%93%E4%B8%AD%E6%96%87)
set linebreak
"autocmd FileType python set breakindentopt=shift:4

"TODO:set sections spliter

if exists('$TMUX')
    set term=screen-256color
endif

"if exists('$ITERM_PROFILE')
"   if exists('$TMUX')
"       let &amp;t_SI = "<Esc>[3 q"
"       let &amp;t_EI = "<Esc>[0 q"
"   else
"       let &amp;t_SI = "<Esc>]50;CursorShape=1x7"
"       let &amp;t_EI = "<Esc>]50;CursorShape=0x7"
"   endif
"endif

" for tmux to automatically set paste and nopaste mode at the time pasting (as
" happens in VIM UI)
"function! WrapForTmux(s)
"  if !exists('$TMUX')
"    return a:s
"  endif
"  let tmux_start = "<Esc>Ptmux;"
"  let tmux_end = "<Esc>"
"  return tmux_start . substitute(a:s, "<Esc>", "<Esc><Esc>", 'g') . tmux_end
"endfunction
"
"let &amp;t_SI .= WrapForTmux("<Esc>[?2004h")
"let &amp;t_EI .= WrapForTmux("<Esc>[?2004l")
"
"function! XTermPasteBegin()
"  set pastetoggle=<Esc>[201~
"  set paste
"  return ""
"endfunction
"
"inoremap <special> <expr> <Esc>[200~ XTermPasteBegin()

"""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""
" cscope setting
"""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""
if has("cscope")
    set csprg=/usr/bin/cscope
    set csto=1
    set cst
    set nocsverb
    " add any database in current directory
    if filereadable("cscope.out")
        cs add cscope.out
    endif
    set csverb
endif
"
"nmap <C-@>s :cs find s <C-R>=expand("<cword>")<CR><CR>
"nmap <C-@>g :cs find g <C-R>=expand("<cword>")<CR><CR>
"nmap <C-@>c :cs find c <C-R>=expand("<cword>")<CR><CR>
"nmap <C-@>t :cs find t <C-R>=expand("<cword>")<CR><CR>
"nmap <C-@>e :cs find e <C-R>=expand("<cword>")<CR><CR>
"nmap <C-@>f :cs find f <C-R>=expand("<cfile>")<CR><CR>
"nmap <C-@>i :cs find i ^<C-R>=expand("<cfile>")<CR>$<CR>
"nmap <C-@>d :cs find d <C-R>=expand("<cword>")<CR><CR>

"fuzzying switching between vim with Chinaese
"http://tieba.baidu.com/p/4178297213
"http://www.2cto.com/os/201402/276425.html

"""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""

"below are about motions

"noremap <Up> <Nop>
"noremap <Down> <Nop>
"noremap <Left> <Nop>
"noremap <Right> <Nop>

"nnoremap k gk
"nnoremap gk k
"nnoremap j gj
"nnoremap gj j

"can use C-o j/k/l/h
"DEBUG:C-h to backspace?
"inoremap <C-h> <Left>
"inoremap <C-j> <Down>
"inoremap <C-k> <Up>
"inoremap <C-l> <Right>

" C-f C-b
" C-d C-u
" C-e C-y
" zz zt zb

" http://mp.weixin.qq.com/s?__biz=MzAxODI5ODMwOA==&mid=2666539328&idx=1&sn=2dac4c8964915c1c13803f32da5ed871&scene=1&srcid=07090SwRlNoqDbNLO55pu8JS&from=groupmessage&isappinstalled=0#wechat_redirect
" operations and movement
" Vim 命令中有类似动词的 d（删除）或 y（复制），类似名词的 b（括号）或
" s（句子），以及类似形容词的 i（内部的）或
" a（全部的）。你可以把它们自由组合，因此，dib 代表删除括号内的所有文本，yap
" 代表复制当前的段落。在你学会这门简单语言之后，你不用再思考删除一个段落需要做什么，只需简单地输入dap。
" tcomment-vim——执行代码注释操作：gc。例如，为一个段落注释，可以用 gcap（go
" comment  a paragraph）；为当前行及以下 5 行注释，可以用
" gc5j（感谢有相对行号！）。
"
" vim-sort-motion——执行排序操作：gs。例如，在 Python 中，对光标所在处的
" imports 进行排序，可以用 gsip（go sort  inside paragraph）。
"
" ReplaceWithRegister——执行替换操作：gr。执行替换文本操作不会覆盖寄存器的内容。例如，要用默认的寄存器替换当前单词，可以用
" griw（go replace inner word ）。gr
" 操作还可以与点号命令配合（.，重复上一次动作），这使它更加高效。
"
" targets.vim——添加许多有用的文本对象。例如，daa
" 用于删除一个函数调用的一个参数（deletes an argument of a function
" call），ci$ 用于改变美元符号之间的文本（在 LaTeX
" 中非常有用）。该插件的另一个特点是，即使光标未落在所欲编辑的文本对象之处，用户也可以通过该插件使用该文本对象。例如，用
" di”（delete inside quotes）可以删除距离光标最近的双引号内的内容。
"
" vim-indent-object——对当前的缩进执行操作。例如，将当前代码块向左移动，可以用
" <ii（left-shift inside indent）。这对 Python
" 来说很有用，因为它用空格标识代码块，而不是大括号。
"
" vim-surround——对周围环境操作。例如，cs”‘ 表示用单引号替换周围的双引号，而
" dsb 则表示删除周围的括号。
"
" 当然，你也可以把在以上插件中提及的操作与行动结合起来使用。例如，在一个
" Python 列表中每一项都在单独成行，为了对列表排序，可以使用 gsii（go sort
" inside indent）。
" 有些简单的文本对象甚至可以不借助插件来定义。例如，对一整个文件进行操作，可以把下列这行代码加入.vimrc：
onoremap af :<C-u>normal! ggVG<CR>
" 于是，要复制整个文件的内容（更准确地讲，整个缓冲器），可以使用 yaf（yank a file）。
"
" Quickly select the text that was just pasted. This allows you to, e.g.,
" indent it after pasting.
noremap gV `[v`]
" Stay in visual mode when indenting. You will never have to run gv after
" performing an indentation.
vnoremap < <gv
vnoremap > >gv
" Make Y yank everything from the cursor to the end of the line. This makes Y
" act more like C or D because by default, Y yanks the current line (i.e. the
" same as yy).
noremap Y y$
" Make Ctrl-e jump to the end of the current line in the insert mode. This is
" handy when you are in the middle of a line and would like to go to its end
" without switching to the normal mode.
"inoremap <C-e> <C-o>$
inoremap <leader>e <C-o>$
inoremap <leader>a <C-o>^
"jump to the middle
"inoremap <leader>m <C-o>70|
"nnoremap <leader>m 70|
" Allows you to easily replace the current word and all its occurrences.
"nnoremap <Leader>rc :%s/<<C-r><C-w>>/
"vnoremap <Leader>rc y:%s/<C-r>"/
" Allows you to easily change the current word and all occurrences to something
" else. The difference between this and the previous mapping is that the mapping
" below pre-fills the current word for you to change.
"nnoremap <Leader>cc :%s/<<C-r><C-w>>/<C-r><C-w>
"vnoremap <Leader>cc y:%s/<C-r>"/<C-r>"
" Replace tabs with four spaces. Make sure that there is a tab character between
" the first pair of slashes when you copy this mapping into your .vimrc!
nnoremap <Leader>rts :%s/   /    /g<CR>
" Remove ANSI color escape codes for the edited file. This is handy when
" piping colored text into Vim.
"nnoremap <Leader>rac :%s/<C-v><Esc>[(d{1,2}(;d{1,2}){0,2})?[m|K]//g<CR>

" set all numbers begin with 0 to decimal numbers
" otherwise 007 + 1 will be 010, instead of 008
set nrformats=

"to avoid auto-indent when pasting
set pastetoggle=<F10>


"use abbreviation
":ab asap as soon as possible
":ab recieve receive
"use :una asap to remove an abbreviation
"to block abbreviation temporarily, press Ctrl-V and space after a word
"
"correction
ab recieve receive
ab rtn return
ab teh the
ab isntead instead
ab vcetor vector
ab startegy strategy
ab mmeory memory
ab taht that
"abbreviation
ab icld #include <>
ab uns using namespace std;
ab thsi this

"vim auto save configuration
"let g:auto_save = 1  " enable AutoSave on Vim startup
"let g:auto_save_no_updatetime = 1  " do not change the 'updatetime' option
""let g:auto_save_in_insert_mode = 0  " do not save while in insert mode
"let g:auto_save_silent = 1  " do not display the auto-save notification
"let g:auto_save_postsave_hook = 'TagsGenerate'  " this will run :TagsGenerate after each save
"let autosave=100

"vim for cuda
au BufNewFile,BufRead *.cu set ft=cuda
au BufNewFile,BufRead *.cuh set ft=cuda
"To suuport cuda in taglist TagBar
"let s:known_types.cpp = type_cpp
"let s:known_types.cuda = type_cpp
"ctags --langmap=c++:+.cu *
"https://stackoverflow.com/questions/10277883/ctagstaglist-for-cu-cuda-files

let g:go_version_warning = 0

" NOTICE: to input tab, use Ctrl-V Tab
" replace tab with 4 spaces to avoid different appearance in different editors
set ts=4
set expandtab

"ag.vim settings
"let g:ag_prg="<custom-ag-path-goes-here> --vimgrep"
"let g:ag_working_path_mode="r"

" settings for csupport of c.vim
" REFER: https://www.cnblogs.com/eddy-he/archive/2012/09/14/vim_csupport.html
"let g:C_MapLeader=';'
"let g:C_Ctrl_j_mode='i'

"python settings
"jedi: https://github.com/davidhalter/jedi-vim
"ipdb: http://www.cnblogs.com/pylemon/archive/2012/03/08/2384899.html

"lisp settings
"BETTER: use emacs or drracket
"http://blog.csdn.net/u011500307/article/details/34452011

"How to use ALT in VIM?
"http://www.zhixing123.cn/ubuntu/55731.html
"https://zhuanlan.zhihu.com/p/20902166
"if !has('gui_running')
"map "in Insert mode, type Ctrl+v Alt+n here" <A-n>
"endif
"noremap <Esc>x :echo "ALT-X pressed"<cr>
"settings of multiple cursors
let g:multi_cursor_use_default_mapping=0
" Default mapping
"let g:multi_cursor_start_word_key      = '<C-n>'
"let g:multi_cursor_select_all_word_key = '<A-n>'
"let g:multi_cursor_start_key           = 'g<C-n>'
"let g:multi_cursor_select_all_key      = 'g<A-n>'
"Below is required
let g:multi_cursor_next_key            = '<C-m>'
let g:multi_cursor_prev_key            = '<C-p>'
let g:multi_cursor_skip_key            = '<C-x>'
let g:multi_cursor_quit_key            = '<Esc>'




"For Vim Ctrl-S
" save in normal mode
nmap <C-S> :update<CR>
" save in visual mode
vmap <C-S> <Esc>:update<CR>
" save in insert mode, return to inert mode after saving
imap <C-S> <C-O>:update<CR>
" exit vim
noremap <C-c> <Esc>:wqa<CR>
noremap <C-z> <Esc>:q!<CR>

"English dictionary
"https://yanbin.blog/configure-vim-dictionary/#more-8332
"
"vim scripts
"https://stackoverflow.com/questions/12105290/how-to-write-simple-script-in-vim
"https://vim.fandom.com/wiki/Write_your_own_Vim_function
setlocal dictionary+=~/.vim/dict/engspchk.dict
"setlocal dictionary+=$VIMRUNTIME/dict/engspchk.dict
"https://blog.csdn.net/kypfos/article/details/78517835
"ctrl-x ctrl-k
"below add dict to default completion list
set complete-=k complete+=k
"external dict
"https://www.linuxidc.com/Linux/2011-01/31182.htm"


"TIPS: to make vim better
"
"1. vim file1 file2 file3...
":n to go to next file, :N to go to prev file,
":bf to first, :bl to last, :e filename to open another(use full path if not
"in current directory)
":ls to see all files opened now  %a is the one, # is the prev
"
"2. to save a file requiring root priority
":w !sudo tee %
"
"3. exchange between shell and vim:
":sh and exit
"Ctrl-Z and fg
"
"4. pair matching: surround
"http://blog.csdn.net/u011500307/article/details/33400853
"
"5. vim diff: http://www.cnblogs.com/MuyouSome/archive/2013/04/28/3049661.html


"To forbid Chinese input in normal mode
"function! OnInsertLeave()
"python << EOT
"import ibus
"ff = ibus.bus.Bus()
"ibus.inputcontext.InputContext(ff,ff.current_input_contxt()).disable()
"EOT
"endfunction
"au InsertLeave * :call OnInsertLeave()

" configure markdown, you-complete-me and syntax
" https://blog.csdn.net/u012948976/article/details/48227713

" jump between windows
map <C-j> <C-w>j
map <C-k> <C-w>k
map <C-h> <C-w>h
map <C-l> <C-w>l

" copy and paste, if + is supported
vmap <leader>c "+y
nmap <leader>c "+yy
nmap <leader>v "+gp
" 映射全选+复制 ctrl+a
"map <C-A> ggVGY
"map! <C-A> <Esc>ggVGY
"map <leader>a <Esc>ggVGY
"map <leader>a <Esc>ggVG"+Y
map <F12> <Esc>ggVG"+Y


" Use clang-format for vim:  2 spaces indent for C++, { not at new line, function parameters alignment, etc.
" clang-format -style=llvm -dump-config > .clang-format
" F3自动格式化代码
"noremap <F3> :Autoformat<CR>
let g:autoformat_verbosemode=0   "no detail information
"保存时自动格式化代码，针对所有支持的文件
"au BufWrite * :Autoformat
au BufWrite *.c,*.cpp,*.h,*.cu,*.cuh :Autoformat
"保存时自动格式化PHP代码
"au BufWrite *.php :Autoformat
"<!-- 指定html格式化工具，并设置缩进为两个空格 -->
"let g:formatdef_my_html = '"html-beautify -s 2"'
"let g:formatters_html = ['my_html']


" configuration of YouCompleteMe
" https://blog.csdn.net/liao20081228/article/details/80347889
" https://www.cnblogs.com/feiyuhuo/p/10274236.html
" NOTICE: on Linux we donot need to install mono, thus not add --cs-completer  and not use --all when compiling YCM.
autocmd InsertLeave * if pumvisible() == 0|pclose|endif "离开插入模式后自动关闭预览窗口
inoremap <expr> <CR> pumvisible() ? "\<C-y>" : "\<CR>" "回车即选中当前项
"上下左右键的行为 会显示其他信息
inoremap <expr> <Down> pumvisible() ? "\<C-n>" : "\<Down>"
inoremap <expr> <Up> pumvisible() ? "\<C-p>" : "\<Up>"
inoremap <expr> <PageDown> pumvisible() ? "\<PageDown>\<C-p>\<C-n>" : "\<PageDown>"
inoremap <expr> <PageUp> pumvisible() ? "\<PageUp>\<C-p>\<C-n>" : "\<PageUp>"
"youcompleteme 默认tab s-tab 和自动补全冲突
"let g:ycm_key_list_select_completion=['<c-n>']
let g:ycm_key_list_select_completion = ['<Down>']
"let g:ycm_key_list_previous_completion=['<c-p>']
let g:ycm_key_list_previous_completion = ['<Up>']
let g:ycm_confirm_extra_conf=0 "关闭加载.ycm_extra_conf.py提示
let g:ycm_collect_identifiers_from_tags_files=1 " 开启 YCM 基于标签引擎
let g:ycm_min_num_of_chars_for_completion=2 " 从第2个键入字符就开始罗列匹配项
let g:ycm_cache_omnifunc=0 " 禁止缓存匹配项,每次都重新生成匹配项
let g:ycm_seed_identifiers_with_syntax=1 " 语法关键字补全
nnoremap <F5> :YcmForceCompileAndDiagnostics<CR> "force recomile with syntastic
"nnoremap <leader>lo :lopen<CR> "open locationlist
"nnoremap <leader>lc :lclose<CR> "close locationlist
inoremap <leader><leader> <C-x><C-o>
"在注释输入中也能补全
let g:ycm_complete_in_comments = 1
"在字符串输入中也能补全
let g:ycm_complete_in_strings = 1
"注释和字符串中的文字也会被收入补全
let g:ycm_collect_identifiers_from_comments_and_strings = 0
" Below can be replaced by gD
"nnoremap <leader>jd :YcmCompleter GoToDefinitionElseDeclaration<CR> " 跳转到定义处


" If use tab for both:  https://blog.csdn.net/qq_20336817/article/details/51115411
" make YCM compatible with UltiSnips (using supertab)
"let g:ycm_key_list_select_completion = ['<C-n>', '<Down>']
"let g:ycm_key_list_previous_completion = ['<C-p>', '<Up>']
"let g:ycm_autoclose_preview_window_after_completion=1
"map <leader>g  :YcmCompleter GoToDefinitionElseDeclaration<CR>
"let g:SuperTabDefaultCompletionType = '<C-n>'

" configuration of snippets
" https://zhuanlan.zhihu.com/p/61036165
" https://vimzijun.net/2016/10/30/ultisnip/
let g:UltiSnipsSnippetDirectories = ['~/.vim/bundle/vim-snippets/UltiSnips', 'UltiSnips']
" Trigger configuration. Do not use <tab> if you use https://github.com/Valloric/YouCompleteMe.
let g:UltiSnipsExpandTrigger="<tab>"
let g:UltiSnipsListSnippets='<C-Tab>'
let g:UltiSnipsJumpForwardTrigger="<tab>"
let g:UltiSnipsJumpBackwardTrigger="<S-tab>" "shift+tab
"let g:UltiSnipsExpandTrigger=";;"
"let g:UltiSnipsExpandTrigger=";;" "maybe we can use two spaces
"let g:UltiSnipsJumpForwardTrigger="];"  "default c-j
"let g:UltiSnipsJumpBackwardTrigger="[;" "default c-k
" If you want :UltiSnipsEdit to split your window.
let g:UltiSnipsEditSplit="vertical"
let g:UltiSnipsUsePythonVersion = 3


" Writing: spell and grammar check
"map <F8> :set spell<CR> " how to unspell via F8
"map <F8> :SpellToggle<CR> "not works
let g:grammarous#show_first_error=1
" spell and language check
"set spell spelllang=en_us "en,de
"""below is for vim-scripts/LanguageTool
"let g:languagetool_jar='~/LanguageTool-3.2/languagetool-commandline.jar'
"autocmd FileType md setlocal spell spelllang=en_us
"setlocal spell spelllang=en_us
"setlocal spell
"set spelllang=nl,en_gb
"set spelllang=en_US
"inoremap <C-l> <c-g>u<Esc>[s1z=`]a<c-g>u
"set spell
"set nospell
"]s
"[s
"z=
"zg
"zw
" for vim latex
" https://jdhao.github.io/2019/03/26/nvim_latex_write_preview/
let g:tex_flavor='pdflatex'
"let g:tex_flavor='latex'
let g:vimtex_view_method='general'
let g:vimtex_view_general='evince'
let g:vimtex_quickfix_mod=0
set conceallevel=1
let g:tex_conceal='abdmg'
"let g:Tex_ShowErrorContext=0
"let g:Tex_GotoError=0
set wrap
set colorcolumn=+1
hi ColorColumn ctermbg=7

" 使用 256 颜色库
let base16colorspace=256
" 使用 base16 中 base16-oceanicnext
"colorscheme base16-oceanicnext
" 使用 gotham 配色
"colorscheme gotham
colorscheme gotham256
colorscheme zenburn

"Different tab settings for different file types
"https://vim.fandom.com/wiki/Indenting_source_code
autocmd FileType c,cpp setlocal shiftwidth=2 tabstop=2
autocmd FileType html setlocal shiftwidth=2 tabstop=2
autocmd FileType python setlocal expandtab shiftwidth=4 softtabstop=4

"https://zhuanlan.zhihu.com/p/25801800
"https://vimjc.com/vim-display-unprintable-character.html
"set list
"set listchars=tab:·
",eol:↩︎,trail:-↩
```


最全的vim配置
https://github.com/langsim/vim-ide

python补全
https://github.com/davidhalter/jedi-vim/

颜色插件
https://github.com/flazz/vim-colorschemes

vim 安装vim-javascript插件--Vundle管理
时间 2015-01-05 16:23:00 博客园精华区
原文 http://www.cnblogs.com/juepei/p/4203922.html
主题 Vim
最近看了一下node.js，但是写的时候，vim对js没有很好的提示。于是就安装插件来处理,准备安装 vim-javascript 。但是安装github上面的插件时，推荐用 Vundle 和 pathogen .

安装插件，用vundle管理，的确是方便很多。具体配置如下（本人操作系统是ubuntu 14.04 lts）：

1.下载vundle，从github下载，本人没有用管理员权限，是用普通用户来安装的。

git clone https://github.com/gmarik/vundle.git ~/.vim/bundle/vundle
不管有没有.vim文件夹，这样就会生成这个文件夹了。

2.配置vimrc，既然不是用的root用户，那就直接在本用户目录下，新建一个.vimrc文件，内容如下：

set nocompatible	" be iMproved
filetype off	" required!
set rtp+=~/.vim/bundle/vundle/
call vundle#rc()
" let Vundle manage Vundle
" required!
Bundle 'gmarik/vundle'
" 可以通过以下四种方式指定插件的来源
" a) 指定Github中vim-scripts仓库中的插件，直接指定插件名称即可，插件明中的空格使用“-”代替。
"Bundle 'L9'
" b) 指定Github中其他用户仓库的插件，使用“用户名/插件名称”的方式指定
"add javascript vim
Bundle "pangloss/vim-javascript"
" c) 指定非Github的Git仓库的插件，需要使用git地址
"Bundle 'git://git.wincent.com/command-t.git'
" d) 指定本地Git仓库中的插件
"Bundle 'file:///Users/gmarik/path/to/plugin'

filetype plugin indent on	 " required!  
里面有Bundle " pangloss/vim-javascript "插件, 这样就可以在接下来进行安装了。

一切完毕后，打开vim。如果打开没有异常，基本是没问题了。这时候，输入：BundleInstall ，自动就会安装vimrc中所写的插件了。如果出错，也就是bundle目录路径的问题。

这样就可以安心的写js了。

vim-jsbeautify 安装
([](http://www.tuicool.com/articles/VBzY7rZ
http://blog.58share.com/?p=247
https://github.com/maksimr/vim-jsbeautify))
=============================================================

在~/ 目录下，编辑   .vimrc  文件
-----------------------------------------------------------------
set nocompatible                          " be iMproved  
        filetype off                              " required!  
        set rtp+=~/.vim/bundle/vundle/
        call vundle#rc()
        Bundle 'gmarik/vundle'
        Bundle "pangloss/vim-javascript"
        Bundle 'maksimr/vim-jsbeautify'
        filetype plugin indent on        " required!  
-----------------------------------------------------------
vim命令行输入 :BundleInstall 安装完成，配置

在.vim目录下有一个文件： .editorconfig
;.editorconfig

root = true

[**.js]
; path to optional external js beautifier, default is vim-jsbeautify/plugin/lib
path=~/.vim/bundle/js-beautify/js/lib/beautify.js
; Javascript interpreter to be invoked by default 'node'
bin=node
indent_style = space
indent_size = 2

[**.css]
path=~/.vim/bundle/js-beautify/js/lib/beautify-css.js
indent_style = space
indent_size = 4

[**.html]
; Using special comments
; And such comments or apply only in global configuration
; So it's best to avoid them
;vim:path=~/.vim/bundle/js-beautify/js/lib/beautify-html.js
;vim:max_char=78:brace_style=expand
indent_style = space
indent_size = 4
最后的 ~/.vimrc
[rabbit@iZ23psatkqsZ ~]$ cat .vimrc
"set nocompatible " Use Vim defaults (much better!)  
set bs=2  " allow backspacing over everything in insert mode  
set ai   " always set autoindenting on  
"set backup  " keep a backup file  
set viminfo='20,\"50 " read/write a .viminfo file, don't store more  
   " than 50 lines of registers  
set history=50  " keep 50 lines of command line history  
set ruler  " show the cursor position all the time  
  
execute pathogen#infect()
syntax on
set nohls
set hlsearch  
set incsearch  
set tabstop=4  
set autoindent  
set cindent  
set confirm  
set nonumber  
set expandtab  
set autoindent   
set smartindent   
filetype indent on   
"filetype plugin indent on
set syn=cpp  
set fileencodings=utf-8,ucs-bom,gb18030,gbk,gb2312,cp936
set termencoding=utf-8
set encoding=utf-8
hi Identifier cterm=NONE
hi Comment cterm=NONE ctermfg=4
hi Statement cterm=NONE
hi Constant cterm=NONE ctermfg=1
hi Type cterm=NONE
hi PreProc cterm=NONE ctermfg=5
hi Special cterm=NONE ctermfg=5
set paste
map <C-n> :NERDTree<CR>
set nocompatible                          " be iMproved  
        filetype off                      " required!  
        set rtp+=~/.vim/bundle/vundle/
        call vundle#rc()
        Bundle 'gmarik/vundle'
            Bundle "pangloss/vim-javascript"
            Bundle 'maksimr/vim-jsbeautify'
        filetype plugin indent on         " required!  
" map <c-f> :call JsBeautify()<cr>
autocmd FileType javascript noremap <buffer>  <c-f> :call JsBeautify()<cr>
autocmd FileType json noremap <buffer> <c-f> :call JsonBeautify()<cr>
autocmd FileType jsx noremap <buffer> <c-f> :call JsxBeautify()<cr>
autocmd FileType html noremap <buffer> <c-f> :call HtmlBeautify()<cr>
autocmd FileType css noremap <buffer> <c-f> :call CSSBeautify()<cr>

" autocmd FileType javascript vnoremap <buffer>  <c-f> :call RangeJsBeautify()<cr>
" autocmd FileType json vnoremap <buffer> <c-f> :call RangeJsonBeautify()<cr>
" autocmd FileType jsx vnoremap <buffer> <c-f> :call RangeJsxBeautify()<cr>
" autocmd FileType html vnoremap <buffer> <c-f> :call RangeHtmlBeautify()<cr>
" autocmd FileType css vnoremap <buffer> <c-f> :call RangeCSSBeautify()<cr>
[rabbit@iZ23psatkqsZ ~]$ 
https://github.com/maksimr/vim-jsbeautify

作者：raawaa
链接：https://www.zhihu.com/question/19714240/answer/42910634
来源：知乎
著作权归作者所有，转载请联系作者获得授权。

用 YouCompleteMe，非常好用，配合 tern 甚至可以自动补全对象属性和函数名。
安装 YouCompleteMe插件
去 Valloric/YouCompleteMe · GitHub 跟着文档安装，过程稍微有点繁琐，需要手动编译一些依赖库，不过文档写的很详细，所以应该不会有什么问题。

安装 tern_for_vim 插件
YouCompleteMe 只原生对 C 系列的静态语言提供补全。对于 javascript，YouCompleteMe 会调用 omni-completion 进行补全。为了使 omni-completion 支持 javascript 的语义分析，需要通过 tern_for_vim 插件来调用 tern 这个强大的 javascript 代码分析器。

首先安装 tern_for_vim，我用 Pathogen 管理 vim 插件，所以直接 cd 到 ~/.vim/bundle 下：
git clone https://github.com/marijnh/tern_for_vim.git
然后 cd 到 tern_for_vim 目录下
npm install
安装依赖，其实就是安装 tern 本体。因为 tern 本身就是个 node_module。
大功告成，体验媲美 Visual Studio 的 Intelisense 。

YouCompleteMe安装
在下面的目录下：

.vim/bundle/YouCompleteMe
执行：

$ git submodule update --init --recursive
$ install.py

[rabbit@iZ23psatkqsZ YouCompleteMe]$ ll
total 224
-rw-r--r-- 1 rabbit ctp    394 Feb 21 08:19 appveyor.yml
drwxr-xr-x 2 rabbit ctp   4096 Feb 21 08:19 autoload
drwxr-xr-x 4 rabbit ctp   4096 Feb 21 08:19 ci
-rw-r--r-- 1 rabbit ctp    350 Feb 21 08:19 codecov.yml
-rw-r--r-- 1 rabbit ctp   2385 Feb 21 08:19 CODE_OF_CONDUCT.md
-rw-r--r-- 1 rabbit ctp   6051 Feb 21 08:19 CONTRIBUTING.md
-rw-r--r-- 1 rabbit ctp  35147 Feb 21 08:19 COPYING.txt
drwxr-xr-x 2 rabbit ctp   4096 Feb 21 08:21 doc
-rwxr-xr-x 1 rabbit ctp   1500 Feb 21 08:19 install.py
-rwxr-xr-x 1 rabbit ctp    328 Feb 21 08:19 install.sh
drwxr-xr-x 2 rabbit ctp   4096 Feb 21 08:19 plugin
-rwxr-xr-x 1 rabbit ctp    153 Feb 21 08:19 print_todos.sh
drwxr-xr-x 3 rabbit ctp   4096 Feb 21 08:19 python
-rw-r--r-- 1 rabbit ctp 125486 Feb 21 08:19 README.md
-rwxr-xr-x 1 rabbit ctp   3181 Feb 21 08:19 run_tests.py
drwxr-xr-x 5 rabbit ctp   4096 Feb 21 08:19 third_party
-rw-r--r-- 1 rabbit ctp    169 Feb 21 08:19 tox.ini
[rabbit@iZ23psatkqsZ YouCompleteMe]$ pwd
执行 install.py。

在.vimrc中加上：
```
" 自动补全配置
set completeopt=longest,menu    "让Vim的补全菜单行为与一般IDE一致(参考VimTip1228)
autocmd InsertLeave * if pumvisible() == 0|pclose|endif "离开插入模式后自动关闭预览窗口
inoremap <expr> <CR>       pumvisible() ? "\<C-y>" : "\<CR>"    "回车即选中当前项
"上下左右键的行为 会显示其他信息
inoremap <expr> <Down>     pumvisible() ? "\<C-n>" : "\<Down>"
inoremap <expr> <Up>       pumvisible() ? "\<C-p>" : "\<Up>"
inoremap <expr> <PageDown> pumvisible() ? "\<PageDown>\<C-p>\<C-n>" : "\<PageDown>"
inoremap <expr> <PageUp>   pumvisible() ? "\<PageUp>\<C-p>\<C-n>" : "\<PageUp>"

"youcompleteme  默认tab  s-tab 和自动补全冲突
"let g:ycm_key_list_select_completion=['<c-n>']
let g:ycm_key_list_select_completion = ['<Down>']
"let g:ycm_key_list_previous_completion=['<c-p>']
let g:ycm_key_list_previous_completion = ['<Up>']
let g:ycm_confirm_extra_conf=0 "关闭加载.ycm_extra_conf.py提示

let g:ycm_collect_identifiers_from_tags_files=1 " 开启 YCM 基于标签引擎
let g:ycm_min_num_of_chars_for_completion=2     " 从第2个键入字符就开始罗列匹配项
let g:ycm_cache_omnifunc=0      " 禁止缓存匹配项,每次都重新生成匹配项
let g:ycm_seed_identifiers_with_syntax=1        " 语法关键字补全
nnoremap <F5> :YcmForceCompileAndDiagnostics<CR>        "force recomile with syntastic
"nnoremap <leader>lo :lopen<CR> "open locationlist
"nnoremap <leader>lc :lclose<CR>        "close locationlist
inoremap <leader><leader> <C-x><C-o>
"在注释输入中也能补全
let g:ycm_complete_in_comments = 1
"在字符串输入中也能补全
let g:ycm_complete_in_strings = 1
"注释和字符串中的文字也会被收入补全
let g:ycm_collect_identifiers_from_comments_and_strings = 0

nnoremap <leader>jd :YcmCompleter GoToDefinitionElseDeclaration<CR> " 跳转到定义处
```

最后的 .vimrc:

[rabbit@iZ23psatkqsZ YouCompleteMe]$ cd 
[rabbit@iZ23psatkqsZ ~]$ cat .vimrc
set nocompatible              " be iMproved, required
filetype off                  " required

" set the runtime path to include Vundle and initialize
set rtp+=~/.vim/bundle/Vundle.vim
call vundle#begin()
" alternatively, pass a path where Vundle should install plugins
"call vundle#begin('~/some/path/here')

" let Vundle manage Vundle, required
Plugin 'VundleVim/Vundle.vim'

" The following are examples of different formats supported.
" Keep Plugin commands between vundle#begin/end.
" plugin on GitHub repo
Plugin 'Valloric/YouCompleteMe'
Plugin 'ascenator/L9', {'name': 'newL9'}

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
"set nocompatible " Use Vim defaults (much better!)  
set bs=2  " allow backspacing over everything in insert mode  
set ai   " always set autoindenting on  
"set backup  " keep a backup file  
set viminfo='20,\"50 " read/write a .viminfo file, don't store more  
   " than 50 lines of registers  
set history=50  " keep 50 lines of command line history  
set ruler  " show the cursor position all the time  
  
execute pathogen#infect()
syntax on
set nohls
set hlsearch  
set incsearch  
set tabstop=4  
set autoindent  
set cindent  
set confirm  
set nonumber  
set expandtab  
set autoindent   
set smartindent   
filetype indent on   
"filetype plugin indent on
set syn=cpp  
set fileencodings=utf-8,ucs-bom,gb18030,gbk,gb2312,cp936
set termencoding=utf-8
set encoding=utf-8
hi Identifier cterm=NONE
hi Comment cterm=NONE ctermfg=4
hi Statement cterm=NONE
hi Constant cterm=NONE ctermfg=1
hi Type cterm=NONE
hi PreProc cterm=NONE ctermfg=5
hi Special cterm=NONE ctermfg=5
set paste
map <C-n> :NERDTree<CR>

" map <c-f> :call JsBeautify()<cr>
autocmd FileType javascript noremap <buffer>  <c-f> :call JsBeautify()<cr>
autocmd FileType json noremap <buffer> <c-f> :call JsonBeautify()<cr>
autocmd FileType jsx noremap <buffer> <c-f> :call JsxBeautify()<cr>
autocmd FileType html noremap <buffer> <c-f> :call HtmlBeautify()<cr>
autocmd FileType css noremap <buffer> <c-f> :call CSSBeautify()<cr>

" autocmd FileType javascript vnoremap <buffer>  <c-f> :call RangeJsBeautify()<cr>
" autocmd FileType json vnoremap <buffer> <c-f> :call RangeJsonBeautify()<cr>
" autocmd FileType jsx vnoremap <buffer> <c-f> :call RangeJsxBeautify()<cr>
" autocmd FileType html vnoremap <buffer> <c-f> :call RangeHtmlBeautify()<cr>
" autocmd FileType css vnoremap <buffer> <c-f> :call RangeCSSBeautify()<cr>
[rabbit@iZ23psatkqsZ ~]$ 
[rabbit@iZ23psatkqsZ ~]$ 
# 安装 vim autoformat:
https://github.com/Chiel92/vim-autoformat#default-formatprograms%E3%80%82

在.vimrc文件中配置：
Plugin 'Chiel92/vim-autoformat'
然后在vim中执行：
hen restart vim and run :PluginInstall. To update the plugin to the latest version, you can run :PluginUpdate.

apt-get install astyle clang-format python-pep8 python3-pep8 python-autopep8
http://blog.csdn.net/demorngel/article/details/69053613

配置astyle:
先下载：
astyle_2.06_linux.tar.gz
[]https://fossies.org/linux/misc/astyle_2.05.1_linux.tar.gz/
然后在 其gcc目录下 执行make
然后 将其bin下的 astyle执行文件 cp ./bin/astyle /usr/bin copy到 /usr/bin下

在.vimrc中配置：
let g:formatdef_harttle = '"astyle --style=linux -s4 -U "'
let g:formatters_cpp = ['harttle']
let g:formatters_c = ['harttle']
let g:formatters_java = ['harttle']
noremap :Autoformat

==============
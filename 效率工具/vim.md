# vim 总结:
<!--
create time: 2018-05-05 20:50:34
Author: <黄东鸿>
-->

<!-- vim-markdown-toc GFM -->

* [光标快速移动](#光标快速移动)
* [插入](#插入)
* [缩进](#缩进)
* [删除](#删除)
* [查找与替换](#查找与替换)
* [多行查找替换](#多行查找替换)
* [选中](#选中)
* [复制粘贴](#复制粘贴)
* [其他](#其他)
* [函数跳转](#函数跳转)
* [多行注释](#多行注释)
* [自定义快捷键](#自定义快捷键)
	* [\<Leader\>和mapleader变量](#leader和mapleader变量)
	* [支持系统剪贴板的复制粘贴](#支持系统剪贴板的复制粘贴)
	* [支持移动文本到上/下一行](#支持移动文本到上下一行)
* [键表](#键表)
* [插件管理](#插件管理)
	* [pathogen](#pathogen)
	* [vundle](#vundle)
	* [vim-plug](#vim-plug)
	* [vim 常用插件列表](#vim-常用插件列表)
* [插件命令](#插件命令)
	* [vim-markdown-toc](#vim-markdown-toc)
* [插件快捷键](#插件快捷键)
	* [nerdtree](#nerdtree)
* [.vimrc 配置](#vimrc-配置)

<!-- vim-markdown-toc -->

vim 辅助记忆神图：

![vim 辅助记忆神图](./vim-key.png)

进入Vim自带的教程里

	$ vimtutor

检查vim支持的功能

	$ vim --version


> 注意：Mac 自带的 Vim 不支持复制内容到剪切板

### 光标快速移动

* `h`, `j`, `k`, `l` : 左，下，上，右
* `w` : 光标移动至下一单词首位
* `b` : 光标移动至当前单词首位，如果光标已经在当前单词首位，就移动到前一单词首位
* `e` : 光标移动至当前单词末位，如果光标已经在当前单词末位，就移动到下一单词末位
* `$` : 光标移动至行末
* `^` : 光标移动至行首
* `gg` : 光标移动至文本首行
* `Shift + G` : 光标移动至文本尾行
* `27 + Shift + G` : 光标移动至文本第27行
* `Ctrl + f` : 向下翻页
* `Ctrl + b` : 向上翻页
* `2 + Ctrl + f` : 向下翻2页

### 插入

* `i` : 进入编辑模式，在当前光标处插入文本
* `o` : 进入编辑模式，在当前光标的下方插入新一行
* `a` : 进入编辑模式，在下一光标处追加文本
* `s` : 进入编辑模式，删除字符并插入

### 缩进

* `<<` : 向左缩进
* `>>` : 向右缩进

### 删除

* `x` : 删除光标处的字符
* `X` : 删除光标处前面的字符
* `D` : 删除至本行行末
* `d$` : 删除至本行行末
* `d^` : 删除至本行行首
* `d0` : 删除至本行行首
* `dl` : 删除光标处的字符
* `dh` : 删除光标前一个字符
* `dd` : 删除光标所在行
* `3dd` : 删除3行
* `dw` : 删除到下一个单词开头
* `de` : 删除到本单词末尾
* `dE` : 删除到本单词末尾包括标点在内
* `db` : 删除到前一个单词
* `dB` : 删除到前一个单词包括标点在内
* `dG` : 删除到文本末尾

### 查找与替换

* `f + 字符` : 在当前行的光标之后查找字符
* `F + 字符` : 在当前行的光标之前查找字符
* `/word` : 全文查找 word
* `/.word` : 向后搜索 word
* `?.word` : 向前搜索 word
* `n` : 查找下一处
* `N` : 查找上一处
* `r + c` : 将光标所在的字符替换成 c
* `:s/foo/bar` : 光标所在行的第一个 foo 替换为 bar
* `:1,50s/foo/bar/` : 在第1行和第50行之间（含）进行搜索和替换
* `:45s/foo/bar/` : 仅仅在第45行进行搜索和替换
* `:%s/foo/bar/g` : 全文查找 foo 并替换为 bar
* `:%s/foo/bar/gc` : 全文查找 foo 并替换为 bar，替换时询问

	`y/n/a/q/l/^E/^Y` : 

	- y表示同意当前替换；
	- n表示不同意当前替换；
	- a表示替换当前和后面的并且不再确认；
	- q表示立即结束替换操作；
	- l表示把当前的替换后结束替换操作；
	- ^E向上滚屏，用来帮助查看前后内容以决定进行操作；
	- ^Y向下滚屏，用来帮助查看前后内容以决定进行操作。

### 多行查找替换

将 foo 替换成 bar

```
Shift + V
// 方向键选中需要查找替换的内容，输入：,vim会自动补全 :'<,'
:'<,'>s/foo/bar/g>
```

### 选中

* `v + i + w` : 选中当前光标所在的单词 （iw: inner word)
* `v + b` : 选中当前光标到当前单词的开头
* `v + w` : 选中当前光标到当前单词的末尾 
* `ggVG` : 全选	

	解释：

		gg 让光标移动到首行
		V  进入Visual（可视）模式
		G  光标移到最后一行

可视模式下（Visual) 选中内容进行以下操作了的含义：

* `d` : 删除选中内容

* `y` : 复制选中内容到0号寄存

### 复制粘贴

* `yy` : 复制光标所在行的内容
* `yw` : 复制光标所在当前单词的内容
* `p` : 粘贴到光标所在行
* `+y` : 复制选中内容到+寄存器，也就是系统剪贴板，供其他程序使用（在Mac下不可用，因为Mac的Vim不支持剪贴板，可使用以下命令替代）
* `:w !pbcopy` : 复制选中内容到+寄存器，也就是系统剪贴版，供其他程序使用
* `+p` : 粘贴系统剪贴板的内容（在Mac下不可用，因为Mac的Vim不支持剪贴板，可使用以下命令替代)
* `:r !pbpaste` : 粘贴系统剪贴板的内容

### 其他

* `u` : 撤销
* `U` : 行内撤销
* `Ctrl + r` : 取消撤销
* `:!ls` : 等同于在 Shell 终端执行 *ls* 命令。
* `:w` : 保存
* `:q` : 退出
* `:noh` : 取消 highlighted search
* `:edit` `:e` : 刷新当前文件

### 函数跳转

* `gd` : 进入函数
* `Ctrl + T` : 返回跳转之前的位置

### 多行注释

1. `Ctrl + V` : 进入 VISUAL BLOCK (可视块)模式
2. `j/k` : 向下或向上选取行
3. `Shift + I` : 大写I，进入编辑模式
4. `//` : 输入注释符号
5. `ESC` : 连续按连词 ESC 退出 

### 取消注释

1. `Ctrl + V` : 进入块选择模式
2. 选中你要删除的行首的注释符号，注意// 要选中两个
3. 选好之后按 `d` 即可删除注释

### 自定义快捷键

Vim 通过 `map` 自定义快捷键，`map` 是一个映射命令，将常用的很长的命令映射到一个新的功能键上。

对于 `map` 而言，可能有这么几种前缀：

* `nore` : 表示非递归。 递归的映射，其实很好理解，也就是如果键a被映射成了b，c又被映射成了a，如果映射是递归的，那么c就被映射成了b。
* `n` : 表示在普通模式下生效
* `v` : 表示在可视模式下生效
* `i` : 表示在插入模式下生效
* `c` : 表示在命令行模式下生效

命令格式：

`:map {lhs} {rhs}`

其含义是，在 `:map` 作用的模式中把键系列 `{lhs}` 映射为 `{rhs}`，`{rhs}` 可进行映射扫描，也就是可递归映射.

#### \<Leader\>和mapleader变量

`mapleader`变量对所有`map`映射命令起效，它的作用是将参数`<Leader>`替换成`mapleader`变量的值，可以用来自定义快捷键的前缀。


* `let mapleader=";"` : 自定义快捷键的前缀为`;`

#### 支持系统剪贴板的复制粘贴

```vim
let mapleader=";"
vmap <Leader>y :w !pbcopy<CR><CR>
nmap <Leader>y :w !pbcopy<CR><CR>
nmap <Leader>p :r !pbpaste<CR><CR>
```

在 `.vimrc` 进行如上配置后，就支持以下的2个命令了：

* `;y` : 复制内容到剪贴板
* `;p` : 粘贴剪贴板的内容

#### 支持移动文本到上/下一行

```vim
nnoremap <C-j> :m .+1<CR>==
inoremap <C-j> <Esc>:m .+1<CR>==gi
vnoremap <C-j> :m '>+1<CR>gv=gv
nnoremap <C-k> :m .-2<CR>==
inoremap <C-k> <Esc>:m .-2<CR>==gi
vnoremap <C-k> :m '<-2<CR>gv=gv
```

在 `.vimrc` 进行如上配置后，就支持快捷键上下移动文本了：

* `Ctrl + j` : 移动文本到下一行
* `Ctrl + k` : 移动文本到上一行

### 键表

	<k0> - <k9> 	: 小键盘 0 到 9 
	<S-...> 	: Shift＋键 
	<C-...> 	: Control＋键 
	<M-...> 	: Alt＋键 或 meta＋键 
	<A-...>	 	: 同 <M-...> 
	<Esc> 		: Escape 键 
	<Up>		: 光标上移键 
	<Space> 	: 插入空格 
	<Tab> 		: 插入Tab 
	<CR> 		: 等于<Enter>

### 多窗口

* `:split` : 上下方向分窗口
* `:vsplit` : 左右分窗口
* `Ctrl + ww` : 切换窗口
* `Ctrl + w[hjkl]` : 上下左右切换窗口
* `Ctrl + w, Ctrl + -` : 放大上下方向的当前窗口
* `Ctrl + w, Shift + |` : 放大左右方向的当前窗口
* `Ctrl + w, =` : 恢复窗口到相同大小
* `:close` : 关闭当前窗口

### 插件管理

#### pathogen

使用 [pathogen](https://github.com/tpope/vim-pathogen) 来管理插件会非常的方便，可以让每一个插件占有一个单独的目录，解决了文件分散的问题。只需要将要安装的所有插件放在 `~/.vim/bundle` 目录下即可，如果要删除某个插件，只需要将 `~/.vim/bundle` 目录下对应的插件目录删除即可，通常使用 *git clone* 的方式安装插件。

#### vundle

[Vundle](https://github.com/VundleVim/Vundle.vim) 可以说是 `pathogen` 的升级版，把 git 操作整合进去，进一步简化了操作，用户需要做的只是去 GitHub 上找到自己想要的插件的名字，安装、更新和卸载由 vundle 来完成。

插件的安装目录是：`~/.vim/bundle`

在 vim 里面运行以下命令来安装插件：

```terminal
:so ~/.vimrc 	// reload vimrc
:PluginInstall
```

其他命令:

* 打开doc : `:h vundle`
* 更新插件 : `:PluginUpdate`
* 清空全部没有在*.vimrc*中配置的插件 : `:PluginClean`
* 清空没有使用的插件 : `:PluginClean!`
* 列出所有插件 : `PluginList`
* 查找插件 : `PluginSearch`

#### vim-plug

[vim-plug](https://github.com/junegunn/vim-plug) 是 `vundle` 升级版，支持并行安装插件，异步加载插件，配合 [NeoVim](https://github.com/neovim/neovim) 可以安装一些比较高级的插件。

#### 手动安装的插件

* [vim-instant-markdown](https://github.com/suan/vim-instant-markdown) : markdown 预览插件(不能使用vundle安装，只能使用npm手动安装)

#### vundle 插件

* [VundleVim/Vundle.vim]() : 插件管理工具
* [plasticboy/vim-markdown](https://github.com/plasticboy/vim-markdown) : markdown 编辑插件
* [altercation/vim-colors-solarized](https://github.com/altercation/vim-colors-solarized) : vim 配色
* [mzlogin/vim-markdown-toc](https://github.com/mzlogin/vim-markdown-toc) : 生成markdown目录
* [jiangmiao/auto-pairs](https://github.com/jiangmiao/auto-pairs) : 括号自动补全
* [Svtter/ACM.vim](https://github.com/Svtter/ACM.vim/wiki) : 支持ACM的vim插件，用于对单文件的一键编译运行
* [scrooloose/nerdcommenter](https://github.com/scrooloose/nerdcommenter) : 代码注释插件

#### vim-plug 插件

* [dbgx/lldb.nvim]()
* [scrooloose/nerdtree](https://github.com/scrooloose/nerdtree) : 树型文件管理系统

### 插件命令

#### vim-markdown-toc

* `:GenTocGFM` : 生成 [GFM](https://help.github.com/categories/writing-on-github/) (Github Flavored Markdown) 风格的目录
* `:GenTocRedcarpet` : 生成 [Redcarpet](https://github.com/vmg/redcarpet) 风格的目录
* `:GenTocGitLab` : 生成 [GitLab](https://docs.gitlab.com/ee/user/markdown.html) 风格的目录
* `:UpdateToc` : 手动更新目录
* `:RemoveToc` : 手动删除目录

#### nerdcommenter

* `<leader>cc` : 当行注释
* `<leader>cb` : 多行注释
* `<leader>cu` : 取消注释

### 插件快捷键

#### nerdtree

[NERDTree 快捷键辑录](http://yang3wei.github.io/blog/2013/01/29/nerdtree-kuai-jie-jian-ji-lu/)

* `Ctrl + n` : 打开/关闭 nerdtree（自定义的map）
* `Ctrl + w + w` : 左右切换窗口

### vimrc 配置含义

[vim技巧：詳解tabstop、softtabstop、expandtab三個選項的區別](https://kknews.cc/code/gp46ae8.html)

* showcmd : 命令模式下，在底部显示，当前键入的指令。
* relativenumber : 可以将行号变成相对于当前行的方式来显示，它可以帮你快速的精确定位距离当前行的偏移数。比如，当前行是第 27 行，如果我想去往前的14行，我就敲 14k；如果我想去往后的8行，我就敲 8j，方便吧。


### .vimrc 配置

要 source Vim 配置需要在 Vim 里执行：

```
:so %
```
%代表当前文件，这样就会重新加载vimrc配置了。

以下是我的 vim 配置：

```vim
"""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""
" => General
"""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""
syntax on				" 语法高亮
filetype plugin on		" 根据不同的文件类型语言加载不同插件（如，C++ 的语法高亮插件与python的不同）

set nocompatible        " 关闭兼容模式 
set number              " 显示行号
set autoindent          " 自动对齐
set smartindent         " 智能对齐
set showmatch           " 括号匹配模式
set ruler               " 显示状态行
set incsearch           " 查询时非常方便，如要查找book单词，当输入到/b时，会自动找到   第一个b开头的单词，当输入到/bo时，会自动找到第一个bo开头的单词，依次类推，进行查找时，使用此设置会快速找到答案，当你找要匹配的单词时，别忘记回车.

set cindent             " C语言格式对齐
set nobackup            " 不要备份文件
set clipboard+=unnamed	" 共享剪贴板

" 1 tab == 4 spaces
set tabstop=4
set shiftwidth=4

" 高亮显示当前行/列
set cursorline			
" set cursorcolumn

" 与剪贴板共享复制粘贴
let mapleader=";"
vmap <Leader>y :w !pbcopy<CR><CR>
nmap <Leader>y :w !pbcopy<CR><CR>
nmap <Leader>p :r !pbpaste<CR><CR>

" 上移或下移一行
nnoremap <C-j> :m .+1<CR>==
inoremap <C-j> <Esc>:m .+1<CR>==gi
vnoremap <C-j> :m '>+1<CR>gv=gv
nnoremap <C-k> :m .-2<CR>==
inoremap <C-k> <Esc>:m .-2<CR>==gi
vnoremap <C-k> :m '<-2<CR>gv=gv


"""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""
" => pathogen 
 """""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""
execute pathogen#infect()
filetype plugin indent on


"""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""
" => Vundle & Plugins
 """""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""
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

Plugin 'godlygeek/tabular'
Plugin 'plasticboy/vim-markdown'
Plugin 'mzlogin/vim-markdown-toc'
Plugin 'altercation/vim-colors-solarized'
Plugin 'Svtter/ACM.vim'
Plugin 'scrooloose/nerdcommenter'

" All of your Plugins must be added before the following line
call vundle#end()            " required
filetype plugin indent on    " required


"""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""
" => Plugin Settings
"""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""
" vim-markdown 
let g:vim_markdown_folding_disabled = 1

" vim-instant-markdown 
" vim-instant-markdown 不能使用vundle安装，只能使用npm手动安装
set shell=bash\ -i

"solarized theme
syntax enable
set background=dark
"set background=light
colorscheme solarized

" lldb.nvim
"set rtp+=/path/to/lldb.nvim

"""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""
" => vim-plug
"""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""
 
if empty(glob('~/.vim/autoload/plug.vim'))
  	silent !curl -fLo ~/.vim/autoload/plug.vim --create-dirs
	  \ https://raw.githubusercontent.com/junegunn/vim-plug/master/plug.vim
	autocmd VimEnter * PlugInstall --sync | source $MYVIMRC
endif

" Specify a directory for plugins
" - For Neovim: ~/.local/share/nvim/plugged
" - Avoid using standard Vim directory names like 'plugin'
call plug#begin('~/.vim/plugged')

Plug 'dbgx/lldb.nvim'
Plug 'scrooloose/nerdtree'

" lldb.nvim
set rtp+=/path/to/lldb.nvim

" nerdtree
map <C-n> :NERDTreeToggle<CR>


" Initialize plugin system
call plug#end()


"""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""
" => ACM配置 
"""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""

" 一键编译
nmap <F5> :call Run()<CR>
func! Run()
	exec "w"
	exec "!clang++ -Wall % -o %<"
	exec "!./%<"
endfunc

"自动生成并打开.in文件， 方便放入输入数据。
nmap<F2> : vs %<.in <CR> 
"直接运行java程序并读入.in中的数据
nmap<F4> : !clear && time java %< < %<.in <CR>
"直接运行c++程序并读入.in中的数据
"nmap<F5> : !clear && time ./%< < %<.in <CR>  

" 打开.out
nmap<F6> : vs %<.out <CR>

"""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""
" => 括号自动补全
"""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""
:inoremap ( ()<ESC>i
:inoremap ) <c-r>=ClosePair(')')<CR>
:inoremap { {<CR>}<ESC>O
:inoremap } <c-r>=ClosePair('}')<CR>
:inoremap [ []<ESC>i
:inoremap ] <c-r>=ClosePair(']')<CR>
:inoremap " ""<ESC>i
:inoremap ' ''<ESC>i

function ClosePair(char)
  if getline('.')[col('.') - 1] == a:char
      return "\<Right>"
  else
      return a:char
  endif
endfunction

"""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""
" => 一键补全ACM刷题头文件
"""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""

map <F12> :call SetAcmTitle()<CR>
func SetAcmTitle()
let l = 0
let l = l + 1 | call setline(l,'/************************************************')
let l = l + 1 | call setline(l,'* Author        : jumpingfrog0')
let l = l + 1 | call setline(l,'* Created Time  : '.strftime('%Y/%m/%d'))
let l = l + 1 | call setline(l,'* File Name     : '.expand('%'))
let l = l + 1 | call setline(l,'*************************************************/')
let l = l + 1 | call setline(l,'')

let l = l + 1 | call setline(l,'#include <iostream>')
let l = l + 1 | call setline(l,'#include <cstdio>')
let l = l + 1 | call setline(l,'#include <cstring>')
let l = l + 1 | call setline(l,'#include <algorithm>')
let l = l + 1 | call setline(l,'#include <string>')
let l = l + 1 | call setline(l,'#include <cmath>')
let l = l + 1 | call setline(l,'#include <cstdlib>')
let l = l + 1 | call setline(l,'#include <vector>')
let l = l + 1 | call setline(l,'#include <queue>')
let l = l + 1 | call setline(l,'#include <set>')
let l = l + 1 | call setline(l,'#include <map>')
let l = l + 1 | call setline(l,'')
let l = l + 1 | call setline(l,'using namespace std;')
let l = l + 1 | call setline(l,'')
let l = l + 1 | call setline(l,'int main()')
let l = l + 1 | call setline(l,'{')
let l = l + 1 | call setline(l,'	//freopen("in.txt","r",stdin);')
let l = l + 1 | call setline(l,'	//freopen("out.txt","w",stdout);')
let l = l + 1 | call setline(l,'    ')
let l = l + 1 | call setline(l,'    return 0;')
let l = l + 1 | call setline(l,'}')
endfunc

```




```
"""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""
" => 括号自动补全
"""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""
 
function! AutoPair(open, close)
        let line = getline('.')
        if col('.') > strlen(line) || line[col('.') - 1] == ' '
                return a:open.a:close."\<ESC>i"
        else
                return a:open
        endif
endf

function! ClosePair(char)
        if getline('.')[col('.') - 1] == a:char
                return "\<Right>"
        else
                return a:char
        endif
endf

function! SamePair(char)
        let line = getline('.')
        if col('.') > strlen(line) || line[col('.') - 1] == ' '
                return a:char.a:char."\<ESC>i"
        elseif line[col('.') - 1] == a:char
                return "\<Right>"
        else
                return a:char
        endif
endf

inoremap ( <c-r>=AutoPair('(', ')')<CR>
inoremap ) <c-r>=ClosePair(')')<CR>
inoremap { <c-r>=AutoPair('{', '}')<CR>
inoremap } <c-r>=ClosePair('}')<CR>
inoremap [ <c-r>=AutoPair('[', ']')<CR>
inoremap ] <c-r>=ClosePair(']')<CR>
inoremap " <c-r>=SamePair('"')<CR>
inoremap ' <c-r>=SamePair("'")<CR>
inoremap ` <c-r>=SamePair('`')<CR>

```
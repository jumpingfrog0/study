# vim 总结


<!-- vim-markdown-toc GFM -->

* [.Vimrc 配置](#vimrc-配置)
* [ACM 现场14行配置](#acm-现场14行配置)
* [光标快速移动](#光标快速移动)
* [插入](#插入)
* [删除](#删除)
* [查找与替换](#查找与替换)
* [多行查找替换](#多行查找替换)
* [选中](#选中)
* [复制粘贴](#复制粘贴)
* [其他](#其他)
* [自定义快捷键](#自定义快捷键)
	* [\<Leader\>和mapleader变量](#leader和mapleader变量)
	* [支持系统剪贴板的复制粘贴](#支持系统剪贴板的复制粘贴)
* [键表](#键表)
* [插件命令](#插件命令)
	* [vim-markdown-toc](#vim-markdown-toc)

<!-- vim-markdown-toc -->

检查vim支持的功能

	vim --version


> 注意：Mac 自带的 Vim 不支持复制内容到剪切板


### .Vimrc 配置

```vim
# $ vim ~/.vimrc
# 输入以下配置：
	
syntax on				" 语法高亮
filetype plugin on		" 根据不同的文件类型语言加载不同插件（如，C++ 的语法高亮插件与python的不同）
filetype plugin on		" 根据不同文件类型加载相关缩进文件

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
  
map <F5> :call Run()<CR>
func! Run()
	exec "w"
	exec "!g++ -Wall % -o %<"
	exec "!./%<"
endfunc

" 一键编译
"自动生成并打开.in文件， 方便放入输入数据。
nmap<F2> : vs %<.in <CR> 
"直接运行java程序并读入.in中的数据
nmap<F4> : !clear && time java %< < %<.in <CR>
"直接运行c++程序并读入.in中的数据
"nmap<F5> : !clear && time ./%< < %<.in <CR>  

" 打开.out
nmap<F6> : vs %<.out <CR>

```

### ACM 现场14行配置

```vim
syntax on  
set cindent  
set nu  
set tabstop=4  
set shiftwidth=4  
set background=dark  
  
map <C-A> ggVG"+y  
map <F5> :call Run()<CR>  
func! Run()  
    exec "w"  
    exec "!g++ -Wall % -o %<"  
    exec "!./%<"  
endfunc  
```


### 光标快速移动

* `h`, `j`, `k`, `l` : 左，下，上，右

* `w` : 光标移动至下一单词首位

* `b` : 光标移动至当前单词首位，如果光标已经在当前单词首位，就移动到前一单词首位

* `e` : 光标移动至当前单词末位，如果光标已经在当前单词末位，就移动到下一单词末位

* `$` : 光标移动至行末

* `^` : 光标移动至行首

* `gg` : 光标移动至文本首行

* `Shift + g` : 光标移动至文本尾行

* `27 + Shift + g` : 光标移动至文本第27行

* `Ctrl + f` : 向下翻页

* `Ctrl + b` : 向上翻页

* `2 + Ctrl + f` : 向下翻2页

  

### 插入

* `i` : 进入编辑模式，在当前光标处插入文本
* `o` : 进入编辑模式，在当前光标的下方插入新一行
* `a` : 进入编辑模式，在下一光标处追加文本
* `s` : 进入编辑模式，删除字符并插入

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

### 查找与替换

* `f + 字符` : 在当前行的光标之后查找字符
* `F + 字符` : 在当前行的光标之前查找字符
* `/word` : 全文查找 word
* `/.word` : 向后搜索 word
* `?.word` : 向前搜索 word
* `n` : 查找下一处
* `N` : 查找上一处
* `r + w` : 将光标之后的字符替换成 c
* `:%s/foo/bar/g`: 全文替换 foo 为 bar

### 多行查找替换

将 foo 替换成 bar

```
Shift + V
// 方向键选中需要查找替换的内容，输入：,vim会自动补全 :'<,'>
:'<,'>s/foo/bar/g
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
* `p` : 粘贴到光标所在行
* `+y` : 复制选中内容到+寄存器，也就是系统剪贴板，供其他程序使用（在Mac下不可用，因为Mac的Vim不支持剪贴板，可使用以下命令替代）
* `:w !pbcopy` : 复制选中内容到+寄存器，也就是系统剪贴版，供其他程序使用
* `+p` : 粘贴系统剪贴板的内容（在Mac下不可用，因为Mac的Vim不支持剪贴板，可使用以下命令替代)
* `:r !pbpaste` : 粘贴系统剪贴板的内容

### 其他

* `u` : 撤销
* `U` : 行内撤销
* `Ctrl + r` : 取消撤销

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

	let mapleader=";"
	vmap <Leader>y :w !pbcopy<CR><CR>
 	nmap <Leader>y :w !pbcopy<CR><CR>
	nmap <Leader>p :r !pbpaste<CR><CR>

在 `.vimrc` 进行如上配置后，就支持以下的2个命令了：

* `;y`  复制内容到剪贴板
* `;p` : 粘贴剪贴板的内容

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

### 插件命令

#### vim-markdown-toc

* `:GenTocGFM` : 生成 [GFM](https://help.github.com/categories/writing-on-github/) (Github Flavored Markdown) 风格的目录
* `:GenTocRedcarpet` : 生成 [Redcarpet](https://github.com/vmg/redcarpet) 风格的目录
* `:GenTocGitLab` : 生成 [GitLab](https://docs.gitlab.com/ee/user/markdown.html) 风格的目录
* `:UpdateToc` : 手动更新目录
* `:RemoveToc` : 手动删除目录
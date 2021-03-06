# vim-lldb 瞎折腾

前几天突然心血来潮，想要在使用 vim 来写 C 语言程序，并在终端进行调试，Google 了下，可以使用 GDB 来进行调试，配合 vim 的插件有 [Clewn](http://clewn.sourceforge.net/install.html#clewn) 和 [vimGdb](http://clewn.sourceforge.net/install.html#vimgdb)。一番折腾，发现 Mac 对 gdb 的支持很不友好，果断放弃，后来发现，可以直接用 `lldb` 来 debug，于是又开始了一番折腾。

[LLDB Vim Frontend](https://github.com/llvm-mirror/lldb/tree/master/utils/vim-lldb) ：官方的 LLDB Vim Frontend，支持 MacVim。


[vim-lldb](https://github.com/gilligan/vim-lldb) ：LLDB Vim Frontend 的 Fork 项目，可直接使用vim插件管理工具安装。


[lldb.nvim](https://github.com/dbgx/lldb.nvim) : Neovim 的 lldb 调试工具。**本文使用的就是这个。**


### Prerequisites

* [Neovim](https://github.com/neovim/neovim)
* [Neovim python2-client](https://github.com/neovim/python-client)
* [LLDB](http://lldb.llvm.org/) (安装了Xcode就自带lldb了，不需要额外安装)

### 1. 安装 lldb.nvim

1. 使用vim插件管理工具[vim-plug](https://github.com/junegunn/vim-plug) 安装

		Plug 'dbgx/lldb.nvim'

2. 在 neovim 的配置文件 `~/.config/nvim/init.vim` 中加入以下配置：

		set rtp+=/path/to/lldb.nvim

3. 在 vim 中运行以下命令安装插件：

		:PlugInstall

4. 在 vim 中输入 `:help lldb` 能看到 lldb 的文档，就证明插件安装成功了三分之一。

**安装失败的情况**

* 如果提示异常 `Not an editor command: PlugInstall`，进行以下检查：
	* 检查 `vim-plug` 是否已经安装了
		1. vim : `~/.vim/autoload/plug.vim` 文件存在
		2. neovim : `~/.local/share/nvim/site/autoload/plug.vim` 文件存在
	* 检查当前使用的是 `vim` 还是 `neovim`
		1. vim : `~/.vimrc` 按照文档正确配置了 lldb.nvim
		2. neovim : `~/.config/nvim/init.vim` 按照文档正确配置了 lldb.nvim
* 如果提示异常 `Not an editor command: lldb`，有3种可能：
	* 只在原生 vim 中安装了 lldb，但是运行 `:help lldb` 时使用的是 neovim
	* 只在 neovim 中安装了 lldb，但是运行 `:help lldb` 时使用的 原生 vim
	* 需要重启终端

### 2. 安装 neovim 的 python 库 Pynvim

	$ pip2 install neovim

### 3. 解决 lldb 和 neovim python 版本不一致的问题

经过上述过程后，lldb.nvim 的安装还是未完全的，因为你会发现 vim-lldb  的那些命令没法使用，在 nvim 中输入 `:LLsession new` 会提示 `Not an editor command: LLsession`。

需要更新远程插件：

	:UpdateRemotePlugins

但是输入上面的命令会导致 python 的闪退：

```
function <SNR>14_UpdateRemotePlugins..<SNR>14_RegistrationCommands..remote#host#Require..<SNR>14_RequirePythonHost, line 15
Vim(if):Channel was closed by the client
function <SNR>14_UpdateRemotePlugins..<SNR>14_RegistrationCommands..remote#host#Require..<SNR>14_RequirePythonHost, line 22
Failed to load python host. You can try to see what happened by starting Neovim with the environment variable $NVIM_PYTHON_LOG_FILE set to a file and opening the generated log file. Also, the host stderr will be available in Neovim
 log, so it may contain useful information. See also ~/.nvimlog.
 Press ENTER or type command to continue
```

这是 lldb 和 neovim 中使用的 python 版本不一致导致的。

#### 分析

OSX 可以安装多个不同版本的python，这就可能会导致一些冲突。

我们在 python 中导入 neovim 的 module，可以正常运行

	$ python
	>>> import neovim
	>>>

而导入 lldb 的 module，会报以下错误：

	$ python
	>>> import lldb
	Fatal Python error: PyThreadState_Get: no current thread

这是因为我们尝试导入的模块链接的是其他不同版本的 python，而不是系统的 python 版本。

系统默认的 python 存在于

	/System/Library/Frameworks/Python.framework/Versions/2.7/bin/python

而 lldb 模块是来自于 `Xcode Developer Tools`，链接的 python 版本是

	/Applications/Xcode.app/Contents/SharedFrameworks/LLDB.framework/Resources/Python

#### 解决方法

neovim 中有一个 python plugin provider，使得能在 neovim 中运行 python 脚本，这个 provider 是可以指定的。只要 neovim 的 python provider 是 Xcode 下的 python 版本，就可以在 neovim 中运行 lldb了。

有两种选择：

1. 使用系统的 python 作为 neovim 的默认 python provider，然后使用脚本进行动态修改配置使得在 neovim 中能使用 lldb，可以参考这篇文章 [How to debug neovim python remote plugin](https://blog.rplasil.name/2016/03/how-to-debug-neovim-python-remote-plugin.html)。但是我没有成功，不知道是哪一步弄错了，因此我用了第二种方法。

2. 使用 Xcode 下的 python 版本作为 neovim 的 python provider，这样自然就能运行 lldb 了，但是这又会导致 `import neovim` 时闪退，因为最初安装的 neovim 的 python 库 Pynvim 使用的是系统的 python（见上面第二个步骤）。这是有办法解决的，[issue #15](https://github.com/dbgx/lldb.nvim/issues/15) 和 [issue #18](https://github.com/dbgx/lldb.nvim/issues/18) 有详细的讨论。

步骤如下：

1) 首先确保当前 python 的路径是 `/usr/bin`
	
	```terminal
	$ which python
	/usr/bin/python

2) 然后在 `.bash_profile` 中输入以下两行
	
	```
	export PATH="/usr/bin:${PATH}"
	export PYTHONPATH="/Applications/Xcode.app/Contents/SharedFrameworks/LLDB.framework/Resources/Python:${PYTHONPATH}"
	```

3) 卸载 pip

	```terminal
	$ sudo pip uninstall pip
	```

4) 重装 pip，因为指定了 $PYTHONPATH ，所以使用的是新的 python 来重装的
	
	```terminal
	$ url https://bootstrap.pypa.io/get-pip.py -o get-pip.py
	$ sudo python get-pip.py
	```

5) 重装 neovim 的 python 库

	```terminal
	$ which pip
	/usr/local/bin/pip

	$ sudo /usr/local/bin/pip install neovim

6) 更新远程插件

	```vim
	:UpdateRemotePlugins
	```

然后就可以同时导入 lldb 和 neovim 了

```terminal
$ python
>>> import lldb
>>> import neovim
```

此时在 nvim 中就可以使用 LL* 等命令了。

```c++
// main.cpp
#include <iostream>
using namespace std;

int func(int n) 
{ 
    int sum=0,i;
    for(i = 0; i < 7; i++) 
    {
        sum+=i; 
    } 
    return sum; 
} 

int main() 
{ 
    int i;
    int result = 0; 
    for(i = 1; i <= 10; i++)
    {
        result += i;  
    }

    cout << result << endl;
    func(10);
    return 0;
} 
```

```
$ clang++ main.cpp -0 main
$ nvim main.cpp
:LLsession new
:LLmode debug
```

![](http://os3yasu4i.bkt.clouddn.com/vim-lldb.png)

然而，费尽千辛万苦装完 lldb.nvim 后，发现居然不会用，即使有 [YouToBe](https://www.youtube.com/watch?v=aXSNhTH1Co4) 的教程也看不懂...，

纯属一场瞎折腾，总结一下吧。

**参考链接**

[How to debug neovim python remote plugin](https://blog.rplasil.name/2016/03/how-to-debug-neovim-python-remote-plugin.html)
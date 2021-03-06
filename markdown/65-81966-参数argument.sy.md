---
show: step
version: 1.0
enable_checker: true
---

# 参数argument

## 回忆上次

- 上次了解了 窗口 `window`
- 窗口是用来装缓冲`buffer`的
- `buffer`是在内存里面加载的硬盘文件
- 窗口的切分
	- `:sp[lit]` 水平切分
	- `:vsp[lit]` 垂直切分
- 窗口的切换
	- <kbd>ctr-w</kbd>再<kbd>k</kbd>切换到当前窗口上面的窗口
	- <kbd>ctrl-w</kbd>再<kbd>j</kbd>切换到当前窗口下面的窗口
	- <kbd>ctrl-w</kbd>再<kbd>h</kbd>切换到当前窗口左面的窗口
	- <kbd>ctrl-w</kbd>再<kbd>l</kbd>切换到当前窗口右面的窗口
- 窗口的隐藏和全屏
	- `:hid[e]`可以隐藏当前窗口
		- 隐藏的`window`中`buffer`不保存
		- 除非`autowrite`设置了
	- `:on[ly]`可以全屏当前窗口
		- 其他的窗口都进入`:hide`状态
- `laststatus` 可以设置状态栏
- `terminal` 可以开启终端
- 上次主要就是`window`,还挺方便
- 尤其多文件操作
- 这个还有什么可玩的吗？🤔

### 总结简化出窗口的全键盘操作

- 新建与退出
	- <kbd>ctr-w</kbd>再<kbd>s</kbd>  相当于`:sp[lit]` 上下分割
	- <kbd>ctrl-w</kbd>再<kbd>v</kbd>  相当于`:vsp[lit]` 左右分割
	- <kbd>ctrl-w</kbd>再<kbd>q</kbd>  相当于`:q[uit]`
	- <kbd>ctrl-w</kbd>再<kbd>o</kbd>  相当于`on[ly]`全屏
- 多窗口操作
	- 所有窗口都有
	- 全退出`:qall`
	- 全保存`:wall`
	- 全保存并退出`:wqall`
	- 强制退出`:qall!`
- 选择当前窗口
	- <kbd>ctrl-w</kbd>再<kbd>h</kbd>  选择左边的窗口
	- <kbd>ctrl-w</kbd>再<kbd>j</kbd>  选择下边的窗口
	- <kbd>ctrl-w</kbd>再<kbd>k</kbd>  选择上边的窗口
	- <kbd>ctrl-w</kbd>再<kbd>l</kbd>  选择右边的窗口  
- 调整宽度
	- <kbd>ctrl-w</kbd>再<kbd>=</kbd>  所有窗口尽量高度宽度都相等
	- <kbd>ctrl-w</kbd>再<kbd>-</kbd> 当前窗口高度降低
	- <kbd>ctrl-w</kbd>再<kbd>+</kbd> 当前窗口高度升高
	- <kbd>ctrl-w</kbd>再<kbd><</kbd> 当前窗口宽度降低
	- <kbd>ctrl-w</kbd>再<kbd>></kbd> 当前窗口宽度升高

### 同时打开三个文件

- 首先`man vi`查到打开三个文件的方式
	- `vi o1 o2 o3`
		- 命令是`vi`
		- `o1 o2 o3`是参数列表( `arguments` list)
		- 列表里有`3`个参数 `argument`
		- 我们可以在 `:ar[gs]` 查看所有参数

![图片描述](https://doc.shiyanlou.com/courses/uid1190679-20210204-1612441570438)

### 操作参数列表

- `arga` 
	- 添加到参数列表 
	- `argument` `add`
	- `:arga o4` 
		- 添加 `o4` 到参数列表
		- `:args` 可以看见 `o4` 进入了参数列表
	- `:arga ~/.vimrc` 添加一个实际存在的文件
		- `:args` 可以看见 `.vimrc` 进入了参数列表
		- `ls` 可以看到他也进入了 `buffer list`
		- `b .vimrc` 可以把当前 `window` 切换到 `.vimrc` 这个 `buffer` 
- `:argd` 
	- 从参数列表删除 
	- `argument delete`
	- `argd o4`
		- 从参数列表删除o4
		- `:args` 
		- 可以看见 `o4` 从参数列表消失
	- `argd /home/shiyanlou/.vimrc`
		- 从参数列表删除 `.vimrc`
		- `:args`
		- 可以看到`.vimrc`从参数列表消失
		- 但是 `buffer` 还在 
- 参数argument和缓存buffer之间什么关系？

### 参数argument和缓存buffer

- `arguements`是在打开`vim`时候打开的参数 `arguement` 文件列表
	- 一开始打开的文件进入参数列表
	- 在内存中加载成为一个个缓冲`buffers`
	- 也进入缓冲列表 `:buffers`
- 这个时候再新打开文件`:e o5`
	- `o5`会进入`buffers list`
	- 但是不会进入`arguments list`
- 如果想让他进入的话
	- 就需要`:arga o5`
- 想在`arguments list`删除的话
	- 就需要`:argd o5`
- 想在`buffers list`删除的话
	- 可以`bd3`或者`bd o2`
- 参数 `argument` 列表和缓存 `buffer` 列表 关系
	- 他们两个除了开始的时候是一致
	- 后来完全是两个列表
	- 需要分别维护
- 我们为什么理清这些东西呢
	- 因为以后可能会有针对 缓冲`buffers` 文件列表的批处理
	- 也会有针对 参数`arguments` 文件列表的批处理


### 多参数多窗口

- 参数多于`1`的时候可以直接打开多个窗口
	- 开关是`-o`
	- `vi -o o1 o2 o3`
	- 这样就可以横向打开`3`个`window`,每个`argument`对应一个
- 或者`vi -O o1 o2 o3` 
	- 纵向打开`3`个文件

![图片描述](https://doc.shiyanlou.com/courses/uid1190679-20210204-1612440221882)

### 在`vim`中打开多个文件

- 首先在`terminal`做准备
	- `ls -lah > oeasy.txt`
	- `cp oeasy.txt o2z.txt`
	- `vi`
	- `:arga *.txt` 
- 有没有进入参数argument列表
	- `:args`
- 有没有进入缓冲buffer列表？
	- `:buffers`
- 如果`:arga */*.txt`
	- 可以加载一层目录下面的`txt`文件
- 退出vim之后
- 再来观察
	- `e *.txt`不能执行
	- `e` 不支持通配符
	- `e o3z.txt`可以把文件加载到`buffer list`
	- 但不进入`argument list`



### 直接打开
- 多个文件作为`argments list`参数列表
- 在`terminal`中运行
		- `sudo find / -mindepth 3 -maxdepth 4 -name passwd`
		- 可以用`sudo`权限找到所有3层目录到4层目录中
		- 名字含有`passwd`的文件列表
- 这个文件列表可以交给`vi`作为`argments list`参数列表
	-  ` sudo find / -mindepth 3 -maxdepth 4 -name passwd | xargs vi`
-  这样打开之后 
	-  `argments list` 参数列表
	-  `buffers list` 缓冲列表 
	-  都自动加载好了
-  如果有不需要的
	-  可以`:bd4`删除`缓冲buffer`
	-  `:argd filename`来删除`参数argument`
-  如果有需要添加的
	-  `:e filename` 添加`缓冲buffer`　
	-  `:arga filename `添加 `参数argument`

![图片描述](https://doc.shiyanlou.com/courses/uid1190679-20210808-1628410458195)

## 总结

- 我们这次参数列表 `arguments list`
- 所谓参数列表指的是 `vim` 打开的 `参数列表`
- 参数会加载到内存中成为 `buffer`
- 参数的控制
	- `:arga filename `添加 `参数`
	- 此操作支持*可以打开多个文件
	- `:argd filename`来删除`参数`
	- `:args` 查询参数列表
- 缓冲的控制
	- `:bd filename`来删除`缓存`
	- `:e filename`来打开`缓存`
	- `ls`可以列出缓存列表
- 可以在`terminal`中配合`find`来找到文件
	- 然后作为参数给`vim`
	-  ` sudo find / -mindepth 3 -maxdepth 4 -name passwd | xargs vi`
- 精准地控制了参数列表或者缓冲列表
- 这两个可以怎么用呢？🤔
- 下次再说 👋







### 什么是dash shell
+ 大多数基于Debian的Linux发行版将bash shell用作普通登陆shell；而只将dash shell作为安装脚本的快速启动shell，用于安装发行版文件；

### dash shell的特性
#### dash命令行参数

|参数|描述|
|------|------|
|`-a`|导出分配给shell的所有变量|
|`-c`|从特定命令字符串中读取命令|
|`-e`|如果是非交互式shell，在有未经测试的命令失败时立即退出|
|`-f`|显示路径名通配符|
|`-n`|如果是非交互式shell，读取命令但不执行他们|
|`-u`|在尝试展开一个未设置的变量时，将错误消息写出到STDERR|
|`-v`|在读取输入时将输入写出到STDERR|
|`-x`|在执行命令时将每个命令写出到STDERR|
|`-I`|在交互式模式下，忽略输入中的EOF字符|
|`-i`|强制shell运行在交互式模式下|
|`-m`|启动作业控制（在交互式模式下默认开启）|
|`-s`|从STDIN读取命令（在没有指定文件参数时的默认行为）|
|`-E`|启动emacs命令行编辑器|
|`-V`|启动vi命令行编辑器|

#### dash环境变量
##### 默认环境变量
+ 需要在dash环境下；可以是终端使用dash脚本：`./bin/dash`；来运行dash shell，然后使用set命令就可以查看默认的环境变量：

![image](https://github.com/ningbaoqi/Shell/blob/master/gif/pic-1.jpg) 

##### dash的位置参数变量

|位置参数|描述|
|------|------|
|`$0`|shell的名称|
|`$n`|第n个位置参数|
|`$*`|含有所有参数内容的单个值，由IFS环境变量中的第一个字符分隔，没定义IFS的话，由空格分隔|
|`$@`|将所有的命令行参数展开为多个参数|
|`$#`|位置参数的总数|
|`$?`|最近一个命令的退出状态码|
|`$-`|当前选项标记|
|`$$`|当前shell的进程ID PID|
|`$!`|最近一个后台命令的PID|

##### 用户自定义的环境变量
+ 可以在命令行上用赋值语句来定义新的环境变量：`testing=10;export testing`；如果不用export命令，用户自定义的环境变量就只在当前shell或进程中可见；
### dash内建命令

|命令|描述|
|------|------|
|`alias`|创建代表文本字符串的别名字符串|
|`bg`|以后台模式继续执行指定的作业|
|`cd`|切换到指定的目录|
|`echo`|显示文本字符串和环境变量|
|`eval`|将所有参数用空格连起来|
|`exec`|用指定命令替换shell进程|
|`exit`|终止shell进程|
|`export`|导出指定的环境变量，供子shell使用|
|`fg`|以前台模式继续执行指定的作业|
|`getopts`|从参数列表中提取选项和参数|
|`hash`|维护并提取最近执行的命令及其位置的哈希表|
|`pwd`|显示当前工作目录|
|`read`|从STDIN读取一行并将其赋给一个变量|
|`readonly`|从STDIN读取一行并赋给一个只读变量|
|`printf`|用格式化字符串显示文本和变量|
|`set`|列出或设置选项标记和环境变量|
|`shift`|按指定的次数移动位置参数|
|`test`|测试一个表达式，成立返回0，不成立返回1|
|`times`|显示当前shell和所有shell进程的累计用户时间和系统时间|
|`trap`|在shell受到某个指定信号时解析并执行命令|
|`type`|解释指定的名称并显示结果（别名、内建、命令或关键字）|
|`ulimit`|查询或设置进程限制|
|`umask`|设置文件和目录的默认权限|
|`unalias`|删除指定的别名|
|`unset`|从导出的变量中删除指定的变量或选项标记|
|`wait`|等待指定的作业完成，然后返回退出状态码|

### dash脚本编程(默认存在)
#### 创建dash脚本
+ 在脚本中指定要用哪个shell，保证脚本是用正确的shell运行；可以在脚本的第一行指定：`#!/bin/dash`：还可以在这行指定shell命令行参数；
#### 不能使用的功能
+ 由于dash shell只是Bourne Shell的一个子集，bash shell脚本中的有些功能没法在dash shell中使用，下面介绍一下bash shell 中能使用但是在dash shell中不能使用的bash shell功能；
#### 算术运算
+ bash shell脚本中进行数学运算的方法：使用`expr`命令：`expr operation`；使用方括号：`$[ operation ]`；使用双圆括号：`$((operation))`；dash shell支持expr命令和双圆括号方法，但`不支持方括号`方法；在dash shell脚本中执行算术运算的正确格式是用双圆括号方法：

![image](https://github.com/ningbaoqi/Shell/blob/master/gif/pic-2.jpg) 

#### test命令
+ bash shell的test命令允许使用双等号`==`来测试两个字符串是否相等，但是，dash shell中的test命令不能识别用做文本比较的`==`符号，只能识别`=`号：

![image](https://github.com/ningbaoqi/Shell/blob/master/gif/pic-3.jpg) 

#### function命令
+ bash shell支持两种定义函数的方法：使用`函数名（）`语句；使用`function 函数名` 语句；dash shell不支持`function 函数名 `语句，在dash shell中，你必须用函数名和圆括号、语句定义函数：

![image](https://github.com/ningbaoqi/Shell/blob/master/gif/pic-4.jpg) 

### zsh shell（默认没有这个shell）
+ zsh shell的一些独特的功能：改进的shell选项处理；shell兼容性模式；可加载模块；zsh shell提供了一组核心内建命令，并提供了添加额外命令模块的能力，每个命令模块都为特定场景提供了另外一组内建命令；

### zsh shell 的组成
#### shell选项

|zsh shell命令行参数|描述|
|------|------|
|`-c`|只执行指定命令，然后退出|
|`-i`|作为交互式shell启动，提供了一个命令行交互提示符|
|`-s`|强制shell从STDIN读取命令|
|`-o`|指定命令行选项|

+ `-o`参数允许设置shell选项来定义shell功能，到目前为止，zsh shell是所有shell中可定制性最强的，你可以更改很多shell环境的特性：不同的选项可以分为以下几大类：

|选项分类|描述|
|------|------|
|更改目录|该选项用于控制cd命令和dirs命令如何处理目录更改|
|补全|该选项用于控制命令补全功能|
|扩展和扩展匹配|该选项用于控制命令中文件扩展|
|历史记录|该选项用于控制命令历史记录|
|初始化|该选项用于控制shell在启动时如何处理变量和启动文件|
|输入输出|该选项用于控制命令处理|
|作业控制|该选项用于控制shell如何处理作业和启动作业|
|提示|该选项用于控制shell如何处理命令行提示符|
|脚本和函数|该选项用于控制shell如何处理脚本和定义函数|
|shell仿真|该选项允许设置zsh shell来模拟其他类型shell行为|
|shell状态|该选项用于定义启动哪种shell的选项|
|zle|该选项用于控制zsh行编辑器的功能|
|选项别名|可以用作其他选项别名的特殊选项|

### 内建命令
#### 核心内建命令

|命令|描述|
|------|------|
|`alias`|为命令和参数定义一个替代性名称|
|`autoload`|将shell函数预加载到内存中以便快速访问|
|`bg`|以后台模式执行一个作业|
|`bindkey`|将组合键和命令绑定在一起|
|`builtin`|执行指定的内建命令而不是同样名称的可执行文件|
|`bye`|跟exit相同|
|`cd`|切换当前工作目录|
|`chdir`|切换当前工作目录|
|`command`|将指定命令当作外部文件执行而不是函数或内建命令|
|`declare`|设置变量的数据类型|
|`dirs`|显示目录栈的内容|
|`disable`|临时禁用指定的散列表元素|
|`disown`|从作业表中移除指定的作业|
|`echo`|显示变量和文本|
|`emulate`|用zsh来模拟另一个shell，比如Bourne、Korn和C shell|
|`enable`|使能指定的散列元素|
|`eval`|在当前shell进程中执行指定的命令和参数|
|`exec`|执行指定的命令和参数来替换当前shell进程|
|`exit`|退出shell并返回指定的退出状态码，如果没有指定，使用最后一行命令的退出状态码|
|`export`|允许在子shell进程中使用指定的环境变量名及其值|
|`false`|返回退出状态码1|
|`fc`|从历史记录中选择某范围内的命令|
|`fg`|以前台模式执行指定的作业|
|`float`|将指定变量设为保存浮点值的变量|
|`functions`|将指定变量设为保存浮点值的变量|
|`getln`|从缓冲栈中读取下一个值并将其放在指定的变量中|
|`getopts`|提取命令行参数中的下一个有效选项并将它放在指定变量中|
|`hash`|直接修改命令哈希表的内容|
|`history`|列出历史记录文件中的命令|
|`integer`|将指定变量设为整数类型|
|`jobs`|列出指定的作业信息，或分配给shell进程的所有作业|
|`kill`|向指定进程或作业发送信号（默认为SIGTERM）|
|`let`|执行算术运算并将结果赋给一个变量|
|`limit`|设置或显示资源限制|
|`local`|为指定变量设置数据属性|
|`log`|显示受watch参数影响的当前登陆到系统上的所有用户|
|`logout`|同exit，但只有shell是登录shell 时有效|
|`popd`|从目录栈中删除下一项|
|`print`|显示变量和文本|
|`printf`|用C风格的格式字符串来显示变量和文本|
|`pushd`|改变当前工作目录，并将上一个目录放在目录栈中|
|`pushln`|将指定参数放在编辑缓冲栈中|
|`pwd`|显示当前工作目录的完整路径名|
|`read`|读取一行，并用IFS变量将数据字段赋给指定变量|
|`readonly`|将值赋给不能修改的变量|
|`rehash`|重建命令散列表|
|`set`|为shell设置选项或位置参数|
|`setopt`|为shell设置选项|
|`shift`|读取并删除第一个位置参数，然后将剩余的参数向前移动一个位置|
|`source`|找到指定文件并将其内容复制到当前位置|
|`suspend`|挂起shell的执行，直到它收到SIGCONT信号|
|`test`|如果指定条件为true的话，返回退出状态码0|
|`times`|显示当前shell以及shell中所有运行进程的累计用户时间和系统时间|
|`trap`|阻断指定信号从而让shell无法处理，如果收到信号则执行指定命令|
|`true`|返回退出状态码0|
|`ttyct1`|锁定和解锁显示|
|`type`|显示shell会如何解释指定的命令|
|`typeset`|设置或显示变量的特性|
|`ulimit`|设置或显示shell或shell中运行程序的资源限制|
|`umask`|设置或显示创建文件和目录的默认权限|
|`unalias`|删除指定的命令别名|
|`unfunction`|删除指定的已定义函数|
|`unhash`|删除散列表中的指定命令|
|`unlimit`|取消指定的资源限制|
|`unset`|删除指定的变量特性|
|`unsetopt`|删除指定的shell选项|
|`wait`|等待指定的作业和进程完成|
|`whence`|显示指定命令会如何被shell解释|
|`where`|如果shell找到的话，显示指定命令的路径名|
|`which`|用csh风格的输出显示指定命令的路径名|
|`zcompile`|编辑指定的函数或脚本，加速自动加载|
|`zmodload`|对可加载zsh模块执行特定的操作|

#### 附加模块
+ 有大量的命令可以为zsh shell提供额外的内建命令，而且这个数量还在随着程序员不断增加新模块而不断增长;

|zsh模块|描述|
|------|------|
|`zsh/datetime`|额外的日期和时间命令及变量|
|`zsh/files`|基本的文件处理命令|
|`zsh/mapfile`|通过关联数组来访问外部文件|
|`zsh/mathfunc`|额外的科学函数|
|`zsh/pcre`|扩展的正则表达式库|
|`zsh/net/socket`|Unix域套接字支持|
|`zsh/stat`|访问stat系统调用来提供系统的统计状况|
|`zsh/system`|访问各种底层系统功能的接口|
|`zsh/net/tcp`|访问TCP套接字|
|`zsh/zftp`|专用FTP客户端命令|
|`zsh/zselect`|阻塞，直到文件描述符就绪才返回|
|`zsh/zutil`|各种shell实用工具|

+ zsh  shell模块涵盖了很多方面的功能，从简单的命令行编辑功能到高级网络功能，zsh shell的思想是提供一个基本的、最小化的shell环境，让在变成时再添加需要的模块；

#### 查看、添加、删除模块
+ zmodload命令是zsh模块的管理接口，可以在zsh shell会话中用这个命令查看、添加和删除模块；zmodload命令不加任何参数会显示zsh shell中当前已安装的模块；不同的zsh shell 实现在默认情况下包含了不同的模块，要添加新模块，只需在zmodload行上指定模块名即可；如：`zmodload zsh/zftp`；要删除已安装的模块使用-u参数和模块名：如：`zmodload -u zsh/zftp`；通常习惯将zmodload命令放进`$HOME/.zshrc`启动文件中，这样在zsh启动时常用的函数就会自动加载；

### zsh脚本编程
#### 数学运算
+ zsh shell在所有数学运算中都提供了对浮点型的全面支持；
#### 执行运算
+ zsh shell提供了执行数学运算的两种方法：`let命令`；`双圆括号`；在使用let命令时，应该在等式前后加上双引号，这样才能使用空格：`let value="4 * 5.1 / 3.2"`；使用浮点数会带来精度问题，通常要使用printf命令，并指定正确显示结果所需的小数点精度：`printf "%6.3f\n" $value1`；第二种方法是使用双圆括号，这个方法结合了两种定义数学运算的方法：`value=$(( 4 * 5.1 ))`；`(( value=4 * 5.1))`；
#### 数学运算
+ 添加了zsh/mathfunc模块就可以使用高级的运算：开方：`value=$(( sqrt(9) ))`；zsh中支持很多数学函数，要查看zsh/mathfunc模块提供的所有数学函数的清单，可以查看zsh模块的手册界面；
#### 结构化命令
+ zsh shell为shell脚本提供了常用的结构化命令；一、`if-then-else`；二、`for`；三、`while`；四、`until`；五、`select`；六、`case`；zsh中的每个结构化命令采用的语法都跟你熟悉的bash shell一样，zsh shell还包含了另外一个叫做`repeat`的结构化命令，`repeat`命令使用如下格式：
```
repeat param
do
       commands
done
```

+ `param`参数必须是一个数字或能算出一个数字的数学表达式，指定的就是循环的次数：如：

```
value=$(( 10 / 2 ))
repeat $value
do
       echo "this is a test"
done
```
#### 函数
+ zsh shell支持使用`function命令`或通用`圆括号`定义函数名的方式来创建自定义函数；跟bash shell一样；

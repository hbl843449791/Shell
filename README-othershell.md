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

![image](https://github.com/ningbaoqi/Java/blob/master/gif/pic-30.jpg) 

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

![image](https://github.com/ningbaoqi/Java/blob/master/gif/pic-30.jpg) 

#### test命令
+ bash shell的test命令允许使用双等号`==`来测试两个字符串是否相等，但是，dash shell中的test命令不能识别用做文本比较的`==`符号，只能识别`=`号：

![image](https://github.com/ningbaoqi/Java/blob/master/gif/pic-30.jpg) 

#### function命令
+ bash shell支持两种定义函数的方法：使用`函数名（）`语句；使用`function 函数名` 语句；dash shell不支持`function 函数名 `语句，在dash shell中，你必须用函数名和圆括号、语句定义函数：

![image](https://github.com/ningbaoqi/Java/blob/master/gif/pic-30.jpg) 

### zsh shell（默认没有这个shell）
+ zsh shell的一些独特的功能：改进的shell选项处理；shell兼容性模式；可加载模块；zsh shell提供了一组核心内建命令，并提供了添加额外命令模块的能力，每个命令模块都为特定场景提供了另外一组内建命令；


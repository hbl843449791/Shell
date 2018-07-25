### 命令行参数
+ 命令行参数允许在运行脚本时向命令行添加数据；如：`./test 10 30`（执行test脚本并且传递了两个命令行参数（10 和 30） 脚本会通过特殊的变量来处理命令行参数）；
#### 读取参数
+ 位置参数变量是标准的数字：$0 是程序名 $1是第一个参数，$2是第二个参数，以此类推，直到第九个参数$9；例如：
```
factorial=1
for(( number = 1 ; number <= $1; number++ ))
do
       factorial=$[ $factorial * $number ]
done
echo the factorial of $1 is $factorial
```
+ 执行该脚本时候使用：`./test 10`(可以在shell脚本中像使用其他变量一样使用$1变量，shell脚本会自动将命令行参数的值分配给变量)；
#### 如果需要输入更多的命令行参数如：
```
total=$[ $1 * $2 ]
echo the first parameter is $1
echo the second number is $2
echo the total is $total
```
+ 执行该脚本时候使用：`./test 10 20`；
#### 可以在命令行上用文本字符串作为参数如：
+ `echo Hello $1,glad to meet you`；执行该脚本时候使用：`./test ningbaoqi`；每个参数都是用空格分隔的，所以shell会将空格当作成两个值的分隔符，要是在参数值中包含空格，必须使用引号（双引号和单引号都可）,将文本字符串作为参数传递时，引号并非数据的一部分，他们只是表明数据的起始位置：如`./test "ning bao qi`；
#### 如果脚本需要的命令行参数不止9个，就需要使用`${10}`的形式来表示：如：
```
total=$[ ${10} * ${11} ]
echo the parameter is ${10}
echo the parameter is ${11}
echo the total is $total
```
+ 执行该脚本时候使用：`./test 1 2 3 4 5 6 7 8 9 10 11 12`；
### 读取脚本名
+ 可以使用`$0`参数获取shell在命令行启动的脚本名，这在编写多功能工具时很方便；如：`echo the zero parameter is set to ：$0`；注意在执行该脚本的时候使用：`bash test`；
#### `basename`命令会返回不包含路径的脚本名；如：
```
name = $(basename $0)
echo
echo the script name is : $name
```
+ 执行该脚本时候使用：`bash /home/ningbaoqi/test`；
### 测试参数
+ 在使用参数之前一定要检查其中是否存在数据；如：`if [ -n "$1" ]`；使用`-n`测试来检查命令行参数`$1`中是否有数据；

### 特殊参数变量
#### 参数统计
+ 特殊变量`$#`含有脚本运行时携带的命令行参数的个数，可以在脚本中任何地方使用这个特殊变量，就跟普通变量一样：`echo there are $# parameters supplied`；获取命令行中最后一个参数，完全不需要知道实际上到底用了多少个参数；`${!#}`；
#### 抓取所有数据
+ `$*`和`$@`变量可以轻松访问所有参数，这两个变量都能在单个变量中存储所有的命令行参数；`$* `变量会将命令行上提供的所有参数当作一个单词保存，这个单词包含了命令行中出现的每个参数值，基本上`$*`变量会将这些参数视为一个整体，而不是多个个体；`$@ `变量会将命令行上提供的所有参数当作同一个字符串中的多个独立的单词，这样你就能够遍历所有的参数值，得到每个参数，通常通过for完成；如：
```
count=1
for param in "$*"
do
       echo "\$* parameter #count=$param"
       count=$[ $count + 1 ]
done
count=1
for param in "$@"
do
       echo "\$@ parameter #$count = $param"
       count=$[ $count + 1 ]
done
```
+ 执行该脚本：`./test  one two three four `；

### 移动变量
+ `bash shell` 的`shift`命令能够用来操作命令行参数，`shift`命令会根据他们的相对位置来移动命令行参数；在使用`shift`命令时，默认情况下它会将每个参数变量向左移动一个位置，所以，变量`$3`的值会移动到`$2`中，变量`$2`的值会移动到`$1`中，而变量`$1`的值则会被删除（注意，变量`$0`的值，也就是程序名，不会改变）；这是遍历命令行参数的另一个好办法，尤其是在你不知道到底有多少参数时，你可以只操作第一个参数，移动参数，然后继续操作第一个参数；如：
```
count=1
while [ -n "$1" ]
do
       echo "parameter #$count = $1"
       count=$[ $count + 1 ]
       shift
done
```
+ 执行该脚本文件：`./test one two three four`；注意：使用`shift`命令的时候要小心，如果某个参数被移除，它的值就会被丢弃了，无法再恢复；可以一次性移动多个位置，只需要在shift命令提供一个参数，指明要移动的位置数就行了：如：
```
echo
echo "the original parameter is $*"
shift 2
echo "here's the new first parameter : $1"
```
+ 执行该脚本文件：`./test one two three four five`；

### 处理选项
+ 选项是跟当破折号后面对的单个字母，它能改变命令的行为；
#### 查找选项
##### 处理简单选项
```
echo
while [ -n "$1" ]
do
       case "$1" in
              -a) echo "found the -a option";;
              -b) echo "found the -b option";;
              -c) echo "found the -c option";;
               *) echo "$1 is not an option";;
       esac
       shift
done
```
+ 执行该脚本：`./test -a -b -c -d`；
##### 分离参数和选项
+ 分离命令中的参数和选项，Linux处理这个问题的标准方式是用特殊字符将二者分开，该字符会告诉脚本何时选项结束以及普通参数何时开始；对Linux来说，这个特殊字符是双破折号`--`，shell会用双破折号来表明选项列表结束，在双破折号之后，脚本就可以放心的将剩余的命令行参数当作参数，而不是选项来处理了；要检查双破折号，只要在case语句中加入一项就可以了如：
```
echo 
while [ -n "$1" ]
do
       case "$1" in
              -a) echo "found the -a option";;
              -b) echo "found the -b option";;
              -c) echo "found the -c option";;
              --) shift
                          break;;
              *) echo "$1 is not an option";;
       esac
       shift
done
count=1
for param in $@
do
       echo "parameter #count:$param"
       count=$[ $count + 1 ]
done
```
+ 遇到双破折号时，脚本用`break`命令来跳出`while`循环，由于过早的跳出了循环我们需要再加一条`shift`命令来将双破折号移除参数变量；
##### 处理带值的选项
+ 当命令行要求额外的参数时，脚本必须能检测到并正确处理：如：
```
echo 
while [ -n "$1" ]
do
       case "$1" in
              -a) echo "found the -a option";;
              -b) param="$2"
                    echo "found the -n option with parameter value $param"
                    shift;;
              -c) echo "found the -c option";;
              --) shift
                         break;;
              *) echo "$1 is not an option";;
       esac
       shift
done
count=1
for param in "$@"
do
       echo "parameter #$count:$param"
       count=$[ $count + 1 ]
done
```
+ 执行该脚本：`./test -a -b test1 -c`；
### 使用`getopt`命令
+ `getopt`命令是一个在处理命令行选项和参数时非常有用的工具，它能识别命令行参数，从而在脚本中解析他们非常方便；
#### 命令的格式
+ `getopt`命令可以接受一系列任意形式的命令行选项和参数，并自动将他们转换成适当的格式，它的命令格式如：`getopt optstring parameters`；`optstring` 定义了命令行有效的选项字母，还定义了哪些选项字母需要参数值，属于声明选项列表；`optstring`中列出你要在脚本中用到的每个命令行选项字母，然后，在每个需要参数值的选项字母后加一个冒号，`getopt`命令会基于你定义的`optstring`解析提供的参数；如：在命令行中使用：`getopt ab:cd -a -b test1 -cd test2 test3`；在终端就会输出：`-a -b test1 -c -d -- test2 test3`；`optstring`定义了四个有效选项字母，`a b c d`；冒号放在字母b的后面，因为b选项需要一个参数值，当`getopt`命令运行时，它会检查提供的参数列表`（-a -b test1 -cd test2 test3）`，并基于提供的`optstring`进行解析，注意，它会自动将`-cd`选项分成两个独立的选项，并插入双破折号来分隔行中的额外参数；如果指定了一个不在`optstring`中的选项，默认情况下，`getopt`命令还产生一条错误信息如果想忽略这条错误信息，可以在命令后加`-q`选项；如：`getopt -q ab:cd -a -b test1 -cde test2 test3`；
#### 在脚本中使用`getopt`
+ 在脚本中使用`getopt`来格式化脚本所携带的任何命令行选项或参数，用`getopt`命令生成的格式化后的版本来替换已有的命令行选项和参数，用`set`命令能够做到；`set`命令的选项之一就是双破折号`--`，它会将命令行参数替换成`set`命令的命令行值；然后该方法会将原始脚本的命令行参数传给`getopt`命令，之后再将`getopt`命令的输出传给`set`命令，用`getopt`格式化后的命令行参数来替换原始的命令行参数如：`set -- $(getopt -q ab:cd "$@")`；如：
```
set -- $(getopt -q ab:cd "$@")
echo 
while [ -n "$1" ]
do
       case "$1" in
              -a) echo "found the -a option";;
              -b) param="$2"
                            echo "found the -b option while parameters value $param"
                            shift;;
              -c) echo "found the -c option";;
              --) shift
                            break;;
              +) echo "$1 is not an option";;
       esac
       shift
done
count=1
for param in "$@"
do
       echo "parameter #$count:$param"
       count=$[ $count + 1 ]
done
```
+ 在`getopt`命令中隐藏一个小问题，`getopt`命令并不擅长处理带`空格`和`引号`的参数值，它会将`空格`当作`参数分隔符`，而不是根据双引号将二者当作一个参数，可以使用`getopts`；
### 使用更高级的`getopts`命令
+ 每次调用它时，它一次只处理命令行上检测到的一个参数，处理完所有参数后，它会退出并返回一个大于0的退出状态码，这让它非常适用于解析命令行所有参数的循环中；`getopts`命令的格式：`getopts optstring variable`；`optstring`值类似于`getopt`命令中的那个，有效的选项字母都会列在`optstring`中，如果选项字母要求有个参数值，就加上一个`冒号`，要去掉错误消息的话，可以使用`optstring`之前加一个`冒号`，`getopts`命令将当前参数保存在命令行中定义的`variable`中；`getopts`命令会用到两个环境变量，如果选项需要跟一个参数值，`OPTARG`环境变量会保存这个值，`OPTIND`环境变量保存了参数列表中`getopts`正在处理的参数位置，这样就可以处理完选项之后继续处理其他命令行参数了；如：
```
echo 
while getopts :ab:c opt
do
       case "$opt" in
              a) echo "found the -a option";;
              b) echo "found the -b option with value $OPTARG";;
              c) echo "found the -c option";;
              *) echo "unknow option:$opt";;
       esac
done
```
+ 执行该脚本：`./test -ab test1 -c`；`while`语句定义了`getopts`命令，指明了要查找哪些命令行选项，以及每次迭代中存储他们的变量名`opt`；`getopts`命令解析命令行选项时会移除开头的单破折号，所以在`case`定义中不用单破折号；执行脚本的时候可以在参数值中包含空格：`./test -b "test1 test2" -a`；另外一个好用的功能是将选项字母和参数值放在一起使用，而不用加空格：如：`./test -abtest1`（其中`test1`就是`-b`命令的参数）；`getopts`还能够将命令行输出上找到未定义的选项统一输出`问号`；`getopts`命令知道何时停止处理选项，并将参数留给你使用，在`getopts`处理每个选项时，他会将`OPTIND`环境变量加一，在`getopts`完成处理时，可以使用`shift`和`OPTIND`值来移动参数：如：
```
echo 
while getopts :ab:cd opt
do
       case "$opt" in
              a) echo "found the -a option";;
              b) echo "found the -b option with value $OPTARG";;
              c) echo "found the -c option";;
              d) echo "found the -d option";;
              *) echo "unknow option : $opt";;
       esac
done
shift $[ $OPTIND -1 ]
echo 
count=1
for param in "$@"
do
       echo "parameter $count:$param"
       count=$[ $count + 1 ]
done
```
+ 执行该脚本可以使用：`./ test -a -b test1 -d test2 test4 test4 test5`；

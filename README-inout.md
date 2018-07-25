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


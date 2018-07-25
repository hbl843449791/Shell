### 使用if-then语句
+ 最基本的结构化命令就是if-then语句；格式如下：
```
if command 会执行if后面的command，若该command的退出状态码为0将会执行后面的then部分的命令，如果该命令的退出状态码为其他值，then后面的command就不会被执行，如if grep $testuser /etc/password
then
       commands 可以使用不止一条命令，如echo "this is what";echo "this is sb"
fi 表示该if-then语句到此结束
```
+ 有时候也会看见这样的形式：
```
if command;then
       commands
fi
```

### 使用if-then-else语句
```
if command if语句中的命令返回退出状态码为0时，then部分的命令将会被执行，当if语句中的退出状态码为非0时，将会执行else部分的命令
then
       commands
else
       commands
fi
```

### 嵌套if
+ 有时你需要检查脚本中的多种条件，可以使用嵌套的if-then语句；普通的形式：
```
if command
then
       commands
else
       commands
       if command
       then
              commands
       fi
fi
```
+ 使用elif形式：
```
if command
then
       commands
elif command
then
       commands
else
       commands
fi
```

### test命令
+ test命令提供了在if-then语句中测试不同条件的途径，如果test命令中列出的条件成立，test命令就会退出并返回退出状态码0，如果条件不成立，test命令就会退出并返回非零的退出状态码；
#### test命令的格式
+ `test condition`：condition是test命令要测试的一系列参数和值，当用在if-then语句中，将会如下：
```
if test condition
then
       commands
fi
```
+ 如果不写test命令的condition部分，他将会以非零的退出状态码退出；当你加入一个条件时，test命令会测试该条件，例如：可以使用test命令确定变量中是否有内容：`if test $my_variable`；
#### bash shell 提供了另一种条件测试方法
+ 格式如下：方括号定义了测试条件，注意第一个方括号之后和第二个方括号之前必须加上空格，否则将会报错；
```
if [ condition ]
then
       commands
fi
```
#### test命令可以判断三类条件
##### 数值比较

|比较|描述|
|------|------|
|`n1 -eq n2`  |检查n1是否与n2相等；如：`if [ $value1 -eq $value2 ]`判断两个值是不是相等|
|`n1 -ge n2` | 检查n1是否大于或等于n2|
|`n1 -gt n2` | 检查n1是否大于n2；如：`if [ $value1 -gt 5 ] `测试变量value1的值是不是大于5|
|`n1 -le n2` | 检查n1是否小于或等于n2|
|`n1 -lt n2`  |检查n1是否小于n2|
|`n1 -ne n2`  |检查n1是否不等于n2|

+ bash shell只能处理整数，如果你只是通过echo语句来显示这个结果，那没问题，但是，在基于数字的函数中就不行了；
##### 字符串比较

|比较|描述|
|------|------|
|`str1 = str2`|  检查str1是否和str2相同|
|`str1 ！= str2` | 检查str1是否和str2不同|
|`str1 < str2 ` |检查str1是否比str2小 ；如：`if [ $value1 \> $var2 ]`|
|`str1 > str2`|  检查str1是否比str2大|
|`-n str1` | 检查str1的长度是否非0；`if [ -n $val1 ]`|
|`-z str1` |检查str1的长度是否为0 为‘’或者未设定长度都为0|

+ 比较字符串相等性时，比较测试会将所有的标点和大小写情况都考虑在内；字符串顺序：大于号和小于号必须转义，否则shell会把他们当作重定向符号，把字符串值当作文件名；大于和小于顺序和sort命令所采用的不同；
##### 文件比较

|比较|描述|
|------|------|
|`-d file `| 检查file是否存在并是一个目录；如果你打算将文件写入目录或是准备切换到某个目录，最好先检查一下 如：`if [ -d $directorypath ]`|
|`-e file`|  检查file是否存在；`if [ -e file ]`|
|`-f file `| 检查file是否存在并是一个文件；`if [ -f $filepath ]`|
|`-r file`| 检查file是否存在并可读；在读取文件之前最好先查看下；如 `if [ -r $filepath ]`|
|`-s file`|检查file是否存在并非空；`if [ -s $filepath ]`|
|`-w file`| 检查file是否存在并可写；`if [ -w file ]`|
|`-x file`| 检查file是否存在并可执行；`if [ -x filename ] `|
|`-O file` |检查file是否存在并属于当前用户所有；检查你是不是该文件的属主，如：`if [ -O /etc/passwd ]`|
|`-G file` |检查file是否存在并且默认组和当前用户相同；检查文件的默认属组，如果修改了属组，他比较的还是默认属组,如：`if [ -G $filepath ]`|
|`file1 -nt file2` |检查file1是否比file2新；在比较之前，需要确保两个文件存在|
|`file1 -ot file2`|  检查file1是否比file2旧；在比较之前，需要确保两个文件存在|

### 复合条件测试

|if-then语句允许你使用布尔逻辑来组合测试|
|------|
|`[ condition1 ] && [ condition2 ]`；与关系，要让then部分执行，两个条件都得满足；如：`if [ -d $HOME ] && [ -w $HOME/testing ]`|
|`[ condition1 ] \|\| [ condition2 ]`；或关系，任意一个条件为true，then将会执行 |

### if-then的高级特性

|bash shell提供了两项可以在if-then语句中使用的高级特性|
|------|
|用数学表达式的双括号（在双括号中不用符号转义，系统能判断）|
|用于高级字符串处理功能的双方括号|

#### 使用双括号
+ 双括号命令允许在比较过程中使用数学表达式，双括号命令提供了更多的数学符号；格式如下：`(( expression )) `； expression可以是任意的数学赋值或比较表达式；

|双括号命令符号|描述|
|------|------|
|`val++`| 后增|
|`val--`|后减|
|`++val`|先增|
|`--val`|先减|
|`！`|逻辑取反|
|`~ `|位取反|
|`**`|幂运算；`if (( $val1 ** 2 > 90))`判断对应的值的2次幂是不是大于90；`then ((val1 = $val2 ** 2))` 赋值|
|`<<`|左移位|
|`>>`|右移位|
|`&`|位与|
|`\|`|位或|
|`&&`|逻辑与|
|`\|\|`|逻辑或|

+ 注意，不需要将双括号中表达式里的大于号转义，这是双括号命令提供的另一个高级特性；
#### 使用双方括号
+ 双方括号命令提供了针对字符串比较的高级特性,不是所有的shell都支持双方括号，但是目前使用的shell是支持的；格式为：`[[ expression ]]` ；如：`if [[ $USER == r* ]]`；在模式匹配中，可以定义一个正则表达式来匹配字符串，判断是不是匹配此正则表达式；双等号右边的字符串r*视为一个模式，并应用模式匹配规则；

### case命令
+ `case`命令会将指定的变量与不同模式进行比较，如果变量和模式匹配，那么shell会执行为该模式指定的命令，可以通过竖线操作符在同一行中分隔多个模式，星号会获取所有与已知模式不匹配的值；格式如下：注意符号其中匹配的模式是可以多个的：
```
case variable in
patterm1 | patterm2) commands1;;
patterm3) command2;;
*)default commands;;
esac
```
+ 例如：

```
case $USER in
rich | vavava)
       echo "you rich huozhe vavava"
       echo "finish";;
ningbaoqi)
       echo "ningbaoqi hao niu bi";;
testing)
       echo "testing";;
*)
       echo "shenm tebushi";;
esac
```

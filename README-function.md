### 基本的脚本函数
+ 函数是一个脚本代码块，可以为其命名并在代码中任何位置重用，要在脚本中使用该代码块时，只要使用所起的函数名就行了（这个过程称为调用函数）；
### 创建函数
+ 有两种格式可以用来在bash shell脚本中创建函数；
#### 第一种格式：使用关键字`function`，后跟分配给该代码块的函数名
```
function name{
       commands
}
```
+ `name`属性定义了函数的唯一名称，脚本中定义的每个函数都必须有一个唯一的名称；`commands`是构成函数的一条或多条bash shell 命令，在调用该函数时，bash shell 会按命令在函数中出现的顺序依次执行，就像在普通脚本中一样；
#### 第二种格式
```
name(){
       commands
}
```
+ 函数名后的空括号表明正在定义的是一个函数；
### 使用函数
+ 要在脚本中使用函数，只需要像其他shell命令一样，在行中指定函数名就行了；如：
```
function func1{
       echo "this is an example of a function"
}
count=1
while [ $count -le 5 ]
do
       func1
       count=$[ $count + 1 ]
done
echo "this is end of the loop"
func1
echo "now this is the end of the script"
```
+ 每次引用函数名func1时，bash shell都会找到func1函数的定义并执行在那里定义的命令；函数定义不一定非得在shell脚本首先要做的事，但是，在函数被定义前使用函数，会收到命令未找到错误；函数名必须是唯一的，否则就会有问题，如果你重定义了函数，新定义会覆盖原来函数的定义，这一切不会产生错误信息；

### 返回值
+ bash shell 会将函数当作一个小型脚本，运行结束时，会返回一个退出状态码，有三种方法来为函数生成退出状态码；
#### 默认退出状态码
+ 默认情况下，函数的退出状态码是函数中最后一条命令返回的退出状态码，在函数执行结束后，可以用标准的`$?`来确定函数的退出状态码：
```
func1(){
       echo "trying to display a non-exittent file"
       ls -al badfile
}
echo "testing the function"
func1
echo "the exit status is : $?"
```
+ 函数的退出状态码是1，因为函数中的最后一条命令没有执行成功；但是你无法知道函数中其他命令中是否成功运行；
#### 使用return命令
+ bash shell使用return命令来退出函数（return之后的命令不会执行）并返回特定的退出状态码，return语句允许制定一个整数来定义函数的退出状态码，从而提供了一种简单的途径来编程设定函数退出状态码：如：
```
function db1{
       read -p "enter a value:" value
       echo "doubling the value"
       return $[ $value * 2 ]
}
db1
echo "the new value is $?"
```
+ 函数一结束就会取返回值；退出状态码必须是0~255范围内；`$? `变量会返回执行的最后一条命令的退出状态码；
### 使用函数输出
+ 可以用来获取任何类型的函数输出，并将其保存到变量中：`result=‘db1’`：这个命令会将db1函数的输出赋给$result变量：
```
function fun1{
       read -p "enter a value:" value
       echo $[ $value * 2 ]
}
result=$(db1)
echo "the new value is $result"
```
+ 函数会用echo语句来显示计算的结果，该脚本会获取db1函数的输出，而不是查看退出状态码；

### 在函数中使用变量
#### 向函数传递参数
+ 函数可以使用标准的参数环境变量来表示命令行上传给函数的参数，例如：函数名会在`$0`变量中定义，函数命令行上的任何参数都会通过`$1`、`$2`等定义，也可以用特殊变量`$#`来判断传给函数的参数数目；在脚本中使用函数时，必须将参数和函数放在同一行；如：
```
function addem{
       if [ $# -eq 0 ] || [ $# -gt 2 ]
       then
              echo -1
       elif [ $# -eq 1 ]
       then
              echo $[ $1 + $1 ]
       else
              echo $[ $1 + $2 ]
       fi
}
echo -n "Adding 10 and 15:"
value=$(addem 10 15)
echo $value
echo -n "lets try adding just one number:"
value=$(addem 10)
echo $value
echo -n "now trying adding no number"
value=$(addem)
echo $value
echo -n "finally try adding three numbers:"
value=$(addem 10 15 20)
echo $value
```
+ 该脚本首先会检查脚本传给函数的参数个数，如果没有任何参数或者参数多于两个，addem会返回-1，如果只有一个参数，addem会将参数与自身自加。如果有两个参数，addem会将他们进行相加；由于函数使用特殊参数环境变量作为自己的参数值，因此它无法直接获取脚本在命令行中的参数值，下面例子将会失败：
```
function badfunc1{
       echo $[ $1 + $2 ] 
}
if [ $# -eq 2 ]
then
       value=$(badfunc1)
       echo "the result -s $value"
else
       echo "usage badtest a b"
fi
```
+ 在命令行中使用脚本`./test`或`./test 10 20` 都会失败，尽管函数也使用了`$1`和`$2`变量，但他们和脚本主题中的`$1`、`$2`变量并不相同；要在函数中使用命令行中传入的值，必须在调用函数时手动将他们传过去，如：
```
function func1{
       echo $[ $1 + $2 ]
}
if [ $# -eq 2 ]
then
       value=$(func1 $1 $2)
       echo "the result is $value"
else
       echo "usage badtest a b"
fi
```
+ 使用`./test 1 2 `就可以得到执行了（通过将`$1`和`$2`变量传给函数，他们就能跟其他变量一样供函数使用）；
### 在函数中处理变量
#### 全局变量
+ 全局变量是在shell脚本中任何地方都有效的变量，如果你的脚本的主体部分定义了一个全局变量，那么可以在函数内读取它的值；类似的，如果你在函数内定义了一个全局变量，可以在脚本的主体部分读取它的值；默认情况下，你在脚本中定义的任何变量都是全局变量，在函数外定义的变量可以在函数内正常访问；
```
function db1{
       value=$[ $value * 2 ]
}
read -p "enter a value:" value
db1
echo "the new value is :$value"
```
+ `$value`变量在函数外定义并赋值，当函数被调用时没改变量及其值在函数中都依然有效，如果变量在函数内赋予了新值，那么在脚本中引用该变量时，新值也依然有效；
#### 局部变量
+ 函数内部使用的任何变量都可以被声明称局部变量，要实现这一点，只要在变量声明的前面加上`local`关键字就可以：如：`local temp`；也可以在变量赋值语句中使用`local`关键字：如：`local temp=$[ $value + 5 ]`；`local`关键字保证了变量只局限于在该函数中，如果脚本中在该函数之外有同样名字的变量，那么shell将会保持这两个变量的值是分离的：如：
```
function func1{
       local temp=$[ $value + 5 ]
       result=$[ $temp * 2 ]
}
temp=5
value=6
func1
echo "the result is $result"
if [ $temp -gt $value ]
then
       echo "temp us larger"
else
       echo "temp is smaller"
fi
```
+ 在func1函数中使用`$temp`变量时，并不影响在脚本主体中赋给`$temp`变量的值；

### 数组变量和函数
#### 向函数传数组参数
+ 将数组变量当作单个参数传递的话，他不会起作用：如：
```
function test{
       echo "the parameters are:$@"
       thisarray=$1
       echo "the received array is ${thisarray[*]}"
}
myarray=(1 2 3 4 5)
echo "the original array is : ${thisarray[*]}"
test $myarray
```
+ 如果试图将该数组变量作为函数参数，函数只会取数组变量的第一个值；要解决这个问题，必须将该数组变量的值分解成单个的值，然后将这些值作为函数参数使用；
#### 正确的将数组参数传递给函数
```
function fun1{
       local newarray
       newarray=($@)
       echo "the new array value is : ${newarray[*]}"
}
myarray=(1 2 3 4 5)
echo "the original array is ${myarray[*]}"
fun1 ${myarray[*]}
```
#### 正确的将数组参数传递给函数
```
function addarray{
       local sum = 0
       local newarray
       newarray=($(echo "$@"))
       for value in ${newarray[*]}
       do
               sum=$[ $sum + $value ]
       done
       echo $sum
}
myarray=(1 2 3 4 5)
echo "the original array is :${myarray[*]}"
arg1=$(echo ${myarray[*]})
result=$(addarray $arg1)
echo "the result is $result"
```
### 从函数返回数组
```
function arraydblr{
       local origarray
       local newarray
       local elements
       local i
       origarray=($(echo "$@"))
       newarray=($(echo "$@"))
       elements=$[ $# - 1 ]
       for(( i=1;i<=elements;i++)){
              newarray[$i] = $[ ${origarray[$i]} * 2 ]
       }
       echo ${newarray[*]}
}
myarray=(1 2 3 4 5)
echo "the original array is : ${myarray[*]}"
arg1=$(echo ${myarray[*]})
result=($(arraydblr $arg1))
echo "the new array is : ${result[*]}"
```
+ arraydblr函数使用echo语句来输出每个数组元素的值，脚本用arraydblr函数输出来重新生成一个新的数组变量；

### 函数递归
+ 函数可以调用自己来得到结果，通常递归函数都有一个最终可以迭代到的基准值；使用递归实现：`X! = X * ( X - 1 )!`计算阶乘；
```
function fun1{
       if [$1 -eq 1 ]
       then
              echo 1
       else
              local temp=$[ $1 - 1 ]
              local result=$(fun1 $temp)
              echo $[ $result * $1 ]
       fi
}
read -p "enter value:" value
result=$(fun1 $value)
echo "the fun1 of $value is : $result"
```


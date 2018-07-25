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


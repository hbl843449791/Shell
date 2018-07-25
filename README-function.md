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


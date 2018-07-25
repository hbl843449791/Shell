### 创建库
+ bash shell允许创建函数库文件，然后在多个脚本中引用该库文件；以下为实现创建库的步骤：
#### 第一步：创建一个包含脚本中所需函数的公共库文件，这里叫做myfuncs的库文件
```
function addem{
       echo $[ $1 + $2 ]
}
function multem{
       echo $[ $1 * $2 ]
}
function divem{
       if [ $2 -ne 0 ]
       then
              echo $[ $1 / $2 ]
       else
              echo -1
       fi
}
```
#### 第二步
+ 在用这些函数的脚本文件中包含myfuncs库文件，shell函数仅在定义它的shell会话内有效，如果你在shell命令行界面的提示符下运行myfuncs脚本，shell会创建一个新的shell并在其中运行该脚本，它会为那个新的shell定义这三个函数，但当你运行另外一个要用到这些函数的脚本时，他们是无法使用的；使用函数库的关键在于`source`命令，source命令会在当前shell上下文中执行命令，而不是创建一个新shell，可以用source命令来在shell脚本中运行库文件脚本，这样脚本就可以使用库中的函数了；source命令有个快捷键别名，称作点操作符，要在shell脚本中运行myfuncs库文件，只需要在使用库脚本文件的文件中添加：`. ./myfuncs`；这个例子嘉定myfuncs库文件是和shell脚本位于同一个目录，如果不是，你需要使用相应路径访问该文件；使用myfuncs库文件的脚本例子如下：该脚本成功的使用myfuncs库文件中定义的函数；
```
../myfuncs
value=10
value1=4
result=$(addem $value $value1)
```


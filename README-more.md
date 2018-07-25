### for命令
+ for命令的基本格式；
```
/*在list参数中，提供迭代中要用到的一系列值；在每次迭代中var会包含列表中的当前值，第一次迭代会使用列表中的第一个值，第二次迭代会使用列表中的第二个值，以此类推*/
/*list参数中有特殊字符时不能正确显示可以使用如下解决方案
     (1)使用转义字符反斜杠来将单引号转义for test in I don\'t know if "this'll" work
     (2)使用双引号来定义用到单引号的值for test in I don\'t know if "this'll" work
     (3)如果在单独的数据值中有空格，就必须使用双引号将这些值圈起来for test in " new york" "new sb"在每个值两边使用双引号时，shell并不会将双引号当成值的一部分
     (4)for循环假定每个值都是用空格分割的
*/
for var in list
do
       commands//可以是多条bash shell命令
done
```
#### for命令例子
##### 读取列表中的值
```
for test in one two three four five six seven eight nine ten
do
       echo the next state is $test
done
每次for命令遍历值列表，它都会将列表中的下个值赋给$test变量，$test变量的值会在shell脚本的剩余部分一直保持有效，它会一直保持最后迭代的值就算循环结束该值也可以使用
```
##### 从变量读取列表
```
list="one two three four five six seven"
list = $list" eight"向变量中添加值，这是向变量中存储的已有文本字符串尾部添加文本的一个常用方式
for state in $list
do
       echo "have you visited $state?"
done
```
##### 从命令读取值
```
file="states"
for state in $(cat $file)//states文件中不是以空格分割，for命令仍然以每次一行的方式遍历了cat命令的输出
do
       echo "visited beautiful $state"
done
```
### 更改字段分隔符
+ 特殊的环境变量`IFS`，叫做内部字段分隔符，IFS环境变量定义了bash shell用作字段分隔符的一系列字符，默认情况下， bash shell会将下列字符当作字段分隔符：`空格`；`制表符(Tab)`；`换行符`；可以在shell脚本中临时更改IFS环境变量来限制被bash shell当作字段分隔符的字符，例如：如果你想修改IFS的值，使其只能识别换行符，那就使用 `IFS=$'\n'`；
```
#一般情况下爱都会备份原来的数据以便恢复
IFS.OLD = $IFS
IFS=$'\n'
IFS = $IFS.OLD

file="states"
IFS=$'\n'
for state in $(cat $file)
do
       echo "visit beautiful $state"
done
```
+ 假定你要遍历一个文件中用冒号分隔的值（比如在`/etc/passwd`文件中），就需要把`IFS=：`；如果要指定多个IFS字符，只要将他们在赋值行串起来就可以；`IFS=$'\n'：；“`；这个赋值会将换行符、冒号、分号、双引号作为字段分隔符；
### 用通配符取得目录
+ 可以用for命令来自动遍历目录中的文件；
```
for file in /home/ningbaoqi/*
do
       if [ -d "$file" ]//在Linux中，目录名或文件名是可以使用空格的，要适应这种情况，应该将$file变量用“”圈起来，否则将会报错
       then
             echo "$file is a directory"
       elif [ -f "$file" ]
       then
              echo "$file is a file"
       fi
done
```

### C语言风格的for命令
#### C语言风格的for命令格式
```
//for (( variable assignment : condition ; iterator_process ))
for(( a = 1 ; a < 5 ; a++ ))
do
       echo "the next number is $a"
done
```

|for的规则|
|------|
|变量赋值可以有空格|
|条件中的变量不以美元符开头|
|迭代过程的算式未用expr命令模式|

+ 使用多个变量；`for (( a=1, b=10 ; a <= 10 ; b++ ,a++ ))`；

### while命令
#### while命令的基本格式
```
while test_command
do
       other_commands
done
```
#### 例子：
```
name=10
while [ $name -gt 0 ]
do
       echo $name
       name=$[ $name - 1 ]
done
```
#### 使用多个测试命令
+ 只有最后一个测试命令的退出状态码会被用来决定什么时候结束循环；注意，每个测试命令都出现在单独的一行上；
```
while echo $name
              [ $name -ge 0 ]
```

### until命令
+ until命令要求你指定一个通常返回非零退出状态码的测试命令，只有测试命令的退出状态码不为0，bash shell才会执行循环中列出的命令，一旦测试命令返回了退出状态吗0，循环就会结束；
#### until命令的格式：
```
until test_commands
do
       other_commands
done
```
#### 例子：
```
name=100
until [ $name -eq 0 ]
do
       echo $name
       name=$[ $name - 25 ]
done
```
+ 可以在until命令语句中放入多个测试命令，只有最后一个命令的退出状态码决定了bash shell是否执行已定义的other_commands；
```
until echo $name
              [ $name -eq 0 ]
```

### 循环处理文件数据
#### 处理/etc/passwd文件中的数据
```
IFS.OLD=$IFS
IFS=$'\n'
for entry in $(cat /etc/passwd)
do
       echo "values in $entry"
       IFS=:
       for value in $entry
       do
              echo "    $value"
       done
done
```

### 控制循环
#### break命令
+ `break`命令可以退出任意类型的循环；跳出单个循环：在shell执行break命令时，他会尝试跳出当前正在执行的循环：如：
```
for name in 1 2 3 4 5 6 7 8 9 10
do
       if [ $name -eq 5 ]
       then
              break
       fi
       echo "interator number : $name"
done
```
+ 使用break时只会终止所在循环；使用` break n`（任意的正整数）；`break 2`将会终止本次循环并且也会终止本循环所在的外层的一个循环：如：

```
for(( a = 1; a < 4 ; a++))
do
       echo "outer loop:$a"
       for(( b = 1; b<4 ; b++))
       do
              if[ $b -gt 4 ]
              then
                    break 2
              fi
              echo "       inner loop:$b"
       done
done
```
#### continue命令
+ 提前终止某次循环中的命令，但并不会完全终止整个循环，与Java中的含义相同；和break一样，continue命令也可以通过命令行参数指定要继续执行哪一级循环，`continue n`；结束本层循环并且结束包含本次循环的外层循环的本次循环，然后继续执行循环；如：

```
for(( name = 1 ; name < 12 ; name++))
do
       if [ $name -gt 5 ] && [ $name -lt 10 ]
       then
              continue
       fi
       echo "iterator number : $name"
done
```


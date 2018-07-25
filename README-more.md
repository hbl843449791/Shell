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

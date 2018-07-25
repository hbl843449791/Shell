### 理解输入和输出
#### 标准文件描述符
+ Linux系统将每个对象当作文件处理。这包括输入输出进程，Linux用文件描述符来识别每个文件对象，文件描述符是一个非负整数，可以唯一识别会话中打开的文件，每个进程一次最多可以有9个文件描述符面处于特殊目的，bash shell保留了前三个文件描述符`（0、1、2）`；

|文件描述符|缩写|描述|
|------|------|------|
|0| STDIN |STDIN文件描述符代表shell的标准输入，对终端界面来说，标准输入是键盘，在使用输入重定向符号` < `时，Linux会用到重定向指定的文件来替换标准输入文件描述符，它会读取文件并且提取数据，就如同他是键盘上输入的|
|1 |STDOUT |STDOUT文件描述符代表shell的标准输出，默认情况下，大多数bash命令会将输出导向STDOUT文件描述符，可以用输出重定向来改变，也可以将数据追加到某个文件，这可以用` >> `符号来完成，如：`who >> test2`；`ls -al badfile > test3 `当命令生成错误消息时，shell并未将错误消息重定向到输出重定向文件，shell对于错误消息的处理是跟普通输出分开的|
|2|STDERR|shell通过特殊的STDERR文件描述符来处理错误消息，shell或shell中运行的程序和脚本出错时生成的错误消息都会被发送到这个位置；默认情况下，错误消息也会输出到显示器输出中，STDERR不会随着STDOUT的重定向而发生改变|
#### 重定向错误
+ 只要是在使用重定向符号时定义STDERR文件描述符就可以了，有几种办法：
#### 只重定向错误
+ 将该文件描述符放在重定向符号前，该值必须紧紧的放在重定向符号前，否则不会工作：`ls -al badtest 2> test3`；STDERR和STDOUT消息混杂在同一个输出中的例子；`ls -al test test1 test2 2> test3`（ls命令的正常STDOUT输出仍然会发送到默认的STDOUT文件描述符，也就是显示器，由于该命令将文件描述符2的输出重定向到了一个输出文件test3，shell会将生成的所有错误消息直接发送到指定的重定向文件中）；
#### 重定向错误和数据
+ 如果想重定向错误和正常输出，必须用两个重定向符号，需要在符号前面放上待重定向数据所对应的文件描述符，然后指向用于保存数据的输出文件；`ls -al test test1 test2 test3 testbad 2> test6  1>test7`；如果愿意，也可以将STDERR和STDOUT的输出重定向到同一个输出文件，为此bash shell提供了特殊的重定向符号`&>`；`ls -al test test2 test3 test4 badtest &> test7`：当使用`&>`符时，命令生成的所有输出都会被发送到同一位置，包括数据和错误，不是按照顺序传入到文件的，先放STDERR数据，bash shell自动赋予了错误消息更高的优先级，这样你能够集中浏览错误信息；

### 在脚本中重定向输出
+ 两种方法在脚本中重定向输出：临时重定向行输出；永久重定向脚本中的所有命令；
#### 临时重定向
+ 如果有意在脚本中生成错误消息，可以将单独的一行输出重定向到STDERR，你所需要做的是使用输出重定向符来将输出消息重定向到STDERR文件描述符，在重定向到文件描述符时，你必须在文件描述符数字之前加一个`&`；`echo "this is an error message" >&2`：这行会在脚本的STDERR文件描述符所指向的位置显示文本，而不是通常的STDOUT；`echo "this is an error" >&2`；`echo "this is normal output"`；如果像平常一样运行脚本你可能看不出什么区别（记住：默认情况下，Linux会将STDERR导向STDOUT，但是，如果你在运行脚本时重定向了STDERR，脚本中所有导向STDERR的文本就会被重定向）；
#### 永久重定向
##### 你可以用exec命令告诉shell在脚本执行期间重定向某个特定文件描述符

```
echo 1>testout
echo "this is a test of redirecting all output"
echo "from a script to another file"
echo "without having to redicted every individual line"
```

+ `exec`命令会启动一个新shell并将STDOUT文件描述符重定向到文件，脚本中发送给STDOUT的所有输出都被重定向到文件；
##### 可以在脚本执行过程中重定向STDOUT

```
echo 2>testerror
echo "this is the start of the script"
echo "now redirecting all output to another location"
exec 1>testout
echo "this out should go to the testout file"
echo "but this should go to the testerror file" >&2
```

+ 一旦重定向了STDERR或STDOUT，就很难再将他们重定向回原来的位置，以后会记录；

### 在脚本中重定向输入
+ `exec`命令允许你将STDIN重定向到Linux系统上的文件中：`exec 0< testfile`；这个重定向只要在脚本需要输入时就会起作用；如：
```
exec 0< testfile
count=1
while read line
do
       echo "LINE #$count:$line"
       count=$[ $count + 1 ]
done
```

### 创建自己的重定向
+ 在shell中最多可以有9个打开的文件描述符，其他6个从3~8的文件描述符均可用作输入或输出重定向，你可以将这些文件描述符中的任意一个分配给文件，然后在脚本中使用它们；
#### 创建输出文件描述符
+ 可以用`exec`命令来给输出分配文件描述符，和标准的文件描述符一样，一旦将另一个文件描述符分配给一个文件，这个重定向就会一直生效，直到你重新分配：如：

```
exec 3>testout //将文件描述符3重定向到另一个文件
echo "this should display on the monitor"
echo "and this should be stored in the file" >&3 //被输出到文件中了
echo "then this should be back on the monitor"
```
+ 也可以不用创建新文件，而是使用`exec`命令来将输出追加到现有文件中：`exec 3>>testout`；
#### 重定向文件描述符
```
exec 3>&1   将文件描述符3重定向到文件描述符1的当前位置也就是STDOUT，这就意味着任何发送给文件描述符3的输出都将会显示在显示器上
exec 1>testout  将STDOUT重定向到文件，shell现在会将发送给STDOUT的输出直接重定向到输出文件中，但是文件描述符3仍然制定的是STDOUT原来的位置，也就是显示器，如果此时将输出数据发送到文件描述符3它仍然会出现在显示器上
echo "this should store in the output file"
echo "along with this line."
exec 1>&3
echo "now things should be back to normal"
```
#### 创建输入文件描述符
+ 在重定义到文件之前，先将STDIN文件描述符保存到另外一个文件描述符，然后在读取完文件之后再将STDIN恢复到它原来的位置；如：
```
exec 6<&0
exec 0<testfile
count=1
while read line
do
       echo "line #$count:$line"
       count=$[ $count + 1 ]
done
echo 0<&6
read -p "are you done now?" answer
case $answer in
       Y | y) echo "goodbye";;
       N | n) echo "sorry this is the end";;
esac
```
#### 创建读写文件描述符
+ 可以打来单个文件描述符来作为输出和输入，可以用同一个文件描述符对同一个文件进行读写，不过使用该方法时，一定要小心，由于你是对同一个文件进行数据读写，shell会维护一个内部指针，指明在文件中的当前位置，任何读或写都会从文件指针上次的位置开始：

```
exec 3<> testfile    exec命令将文件描述符3分配给文件testfile以进行文件读写
read line <&3   通过分配好的文件描述符，使用read命令读取文件中的第一行
echo "read:$line"  将读取的第一行显示在STDOUT上
echo "this is a test line" >&3  用exec语句将一行数据写入到同一个文件描述符打开的文件中
```
+ 以上脚本运行完毕后，查看testfile文件内容的话，将会发现写入文件中的数据覆盖了已有的数据，当脚本向文件中写入数据时，他是从文件指针所处的位置开始，read命令读取了第一行数据，所以它使得文件指针指向了第二行数据的第一个字符，在echo语句将数据输出到文件时，他会将数据放在文件指针的当前位置，覆盖了该位置的已有数据；
#### 关闭文件描述符
+ 如果你创建了新的输出或输入文件描述符，shell会在脚本退出时自动关闭他们，然而在希望脚本结束前手动关闭文件文件描述符，要关闭文件描述符，将它重定向到特殊符号`&-`；`exec 3>&-`；会关闭文件描述符3，不再在脚本中使用，除非重新的进行分配；如：

```
exec 3>testfile
echo “this is a test line of data” > &3
exec 3>$-
echo "this won't work" >&3  一旦关闭了文件描述符，就不能在脚本中向它写入任何数据，否则shell会生成错误信息
```

+ 在关闭文件描述符时还要注意；如果随后你在脚本中打开了同一个输出文件，shell会用一个新文件来替换已有文件，这就意味着如果你输出数据，就会覆盖已有文件：如：

```
exec 3>testfile
echo "this is a test line of data" >&3
exec 3>&-
cat testfile
exec 3>testfile
echo "this will be bad" >&3
```

### 列出打开的文件描述符
+ `lsof`命令会列出整个Linux系统打开的所有文件描述符，该命令会产生大量的输出，它会显示当前Linux系统上打开的每个文件的有关信息，这包括后台运行的所有进程以及登录到系统的任何用户；

|有大量的命令行参数和选项可以用来帮助过滤lsof的输出|描述|
|------|------|
|`-p ` |允许指定进程ID(PID)|
|`-d `|允许指定要显示的文件描述符编号|
|`\$\$` |显示进程的当前PID|
|`-a` |对其他两个选项的结果执行布尔and运算|

+ `lsof -a -p $$ -d 0，1，2`；

|`lsof`的默认输出列|描述|
|------|------|
|COMMAND  |  正在运行的命令名的前9个字符|
|PID |  进程的PID|
|USER  |  进程属主的登录名|
|FD  |  文件描述符号以及访问类型（r代表读，w代表写，u代表读写）|
|TYPE  |  文件的类型（CHR代表字符型，BLK代表块型，DIR代表目录，REG代表常规文件）|
|DEVICE   | 设备的设备号（主设备号和从设备号）|
|SIZE  |  表示文件的大小|
|NODE   | 本地文件的节点号|
|NAME   | 文件名|

### 阻止命令输出
+ 可以将STDERR重定向到一个叫做`null`文件的特殊文件，`null`文件里什么都没有，shell输出到`null`文件的任何数据都不会保存，全部都被丢掉了；在Linux系统上`null`文件的标准位置是`/dev/null`，重定向到该位置的任何数据都会被丢掉，不会显示。：`ls -al badfile test 2> /dev/null`；可以在输入重定向中将`/dev/null`作为输入文件，由于`/dev/null`文件不包含任何内容，程序员通常用它来快速清除现有文件中的数据，而不用先删除文件再重新创建：`cat /dev/null >testfile`；

### 记录消息
+ 将输出同时发送到显示器和日志文件，不用将重定向两次，只要使用`tee`命令即可；`tee`命令相当于管道的一个`T`型接头，它将从STDIN过来的数据同时发送两处`STDOUT`；`tee命令行所指定的文件名`；由于`tee`会重定向来自STDIN的数据，可以用它配合管道命令来重定向命令输出：`date | tee testfile`；默认情况下，`tee`命令会在每次使用时覆盖输出文件的内容，如果想将数据追加到文件中可以使用` -a `选项：`date | tee -a testfile`；


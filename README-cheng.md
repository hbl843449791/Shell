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


### 信号处理

|Linux利用信号与运行在系统中的进程进行通信|值|描述|
|------|------|------|
|1 |   SIGHUP  |  挂起进程|
|2 |   SIGINT  |  终止进程|
|3  |  SIGQUIT   | 停止进程|
|9   | SIGKILL |   无条件终止进程|
|15  |  SIGTERM  |  尽可能终止进程|
|17  |  SIGSTOP   | 无条件停止进程，但不是终止进程|
|18    |SIGTSTP    |停止或暂停进程，但不终止进程|
|19   | SIGCONT    |继续运行停止的进程|

+ 默认情况下，bash shell会忽略收到的任何`SIGQUIT`和`SIGTERM`信号（这样交互式shell不会被意外终止），但是bash shell会处理收到的`SIGHUP`和`SIGINT`信号；如果bash shell收到了`SIGHUP`信号（比如当要离开一个交互式shell，他就会退出，但在退出之前，他会将`SIGHUP`信号传给所有由该shell所启动的进程，包括正在运行的shell脚本）；通过`SIGINT`信号，可以中断shell，Linux内核会停止为shell分配CPU处理时间，这种情况发生时，shell会将`SIGINT`信号传给所有由它启动的进程；shell会将这些信号传给shell脚本程序来处理，而shell脚本的默认行为是忽略这些信号，他们可能会不利于脚本的运行，要避免这种情况，可以在脚本中加入识别信号的代码，并执行命令来处理信号；
#### 生成信号
+ bash shell 允许用键盘上的组合键生成两种基本的Linux信号，这种特性在需要停止和暂停失控程序是非常方便；
##### 中断进程
+ `CTRL+C`组合键会生成`SIGINT`信号，并将其发送给当前的shell中运行的所有进程；
#####暂停进程
+ 可以在进程运行期间暂停进程，而无需终止它，`CTRL+Z`组合键将会生成一个`SIGTSTP`信号，停止shell中运行的任何进程，停止进程跟终止进程不同：停止进程会将程序继续保存在内存中，并能从上次停止的位置继续运行；在`CTRL+Z`组合键被按的时候会提示一条`[1]+ Stopped    sleep 10`的信息；方括号中的数字是shell分配的作业号，shell将shell中运行的每个进程称为作业，并为每个作业分配唯一的作业号，他会给第一个作业分配作业号1,第二个作业号2，以此类推；如果在shell会话中有一个已停止的作业，在退出shell时，bash 会提醒你；`ps`命令将已停止作业的状态显示为`T`；如果在有已停止作业存在的情况下，你仍然想退出shell，输入两次`exit`命令即可，shell会退出，并且终止已停止作业，知道已停止作业的PID，也可以使用`kill`命令来发送一个`SIGKILL`信号来终止它；`kill -9 PID`；
#### 捕获信号
+ 可以不忽略信号，在信号出现时捕获他们并执行其他命令，`trap`命令允许你来指定shell脚本要监看并从shell中拦截的Linux信号，如果脚本收到了`trap`命令中列出的信号，该信号不再由shell处理，而是交给本地处理；`trap`命令的格式：`trap commands signals`：在`trap`命令行上，你只要列出想要shell执行的命令，以及一组用空格分开的待捕获的信号，你可以用数值或Linux信号名来指定信号；
##### 如：使用`trap`命令来忽略SIGINT信号，并控制脚本的行为；
```
trap "echo 'sorry! I have trapped ctrl-c'" SIGINT
echo this is a test script
count=1
while [ $count -le 10 ]
do
       echo "loop#$count"
       sleep 1
       count=$[ $count + 1 ]
done
echo "this is the end of the test script"
```
+ 用到了`trap`命令会在每次检测到`SIGINT`信号时显示一行简单的文本消息，捕获这些信号会阻止用户用bash shell 组合键`ctrl+c`来停止程序，每次使用`CTRL+C`组合键，脚本都会执行`trap`命令中指定的`echo`语句，而不是处理该信号并允许shell停止该脚本；
#### 捕获脚本退出
+ 除了在shell脚本中捕获信号，也可以在shell脚本退出时进行捕获，这是在shell完成任务时，执行命令的一种简便方法；要捕获shell脚本的退出，只要在`trap`命令后加上`EXIT`信号就行了：如：

```
trap "echo goodbye......" EXIT
count=1
while [ $count -le 5 ]
do
       echo "loop #$count"
       sleep 1
       count=$[ $count + 1 ]
done
```
+ 当脚本运行到正常的退出位置时，捕获就被触发了，shell会执行在`trap`命令行指定的命令，如果需要提前退出，同样能够捕获到`EXIT`信号；因为`SIGINT`信号并没有出现在`trap`命令的捕获列表中，当按下`Ctrl+c`组合键发送`SIGINT`信号时，脚本就退出了，但在脚本退出前捕获到了`EXIT`，于是shell执行了`trap`命令；
#### 修改或移除捕获
+ 要想在脚本中的不同位置进行不同的捕获处理，只需重新使用带有新选项的`trap`命令如：

```
trap "echo 'sorry......ctrl+x is trapped.'" SIGINT
count=1
while [ $count -le 5 ]
do
       echo "loop#$count"
       sleep 1
       count=$[ $count + 1 ]
done
trap "echo 'Im modified the trap!'" SIGINT
count=1
while [ $count -le 5 ]
do
       echo "second loop#$count"
       sleep 1
       count=$[ $count + 1 ]
done
```
+ 也可以删除已设置好的捕获，只需要在`trap`命令也希望恢复默认行为的信号列表之间加上两个破折号就行了：如：

```
trap "echo 'sorry ......ctrl+c is trapped.!'" SIGINT
count=1
while [ $count -le 5 ]
do
       echo "loop #$count"
       sleep 1
       count=$[ $count + 1 ]
done
trap -- SIGINT
echo "I just removed the trap"
count=1
while [ $count -le 5 ]
do
       echo "second loop #$count"
       sleep 2
       count=$[ $count + 1 ]
done
```
+ 可以在`trap`命令后使用`单破折号`来恢复信号的默认行为，`单破折号`和`双破折号`都可以正常发挥作用；移除信号捕获后，脚本按照默认行为来处理`SIGINT`信号，也就是终止脚本运行；

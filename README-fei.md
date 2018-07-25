### 在非控制台下运行脚本
+ 在终端会话中启动shell脚本，然后让脚本一直以后台模式运行到结束，即使你退出了终端会话，这可以用`nohup`命令来实现；`nohup`命令运行了另外一个命令来阻断所有发送给该进程的`SIGHUP`信号，这会在退出终端会话时阻止进程退出；`nohup`命令的格式如下：`nohup ./test &`；和普通后台进程一样，shell会给命令分配一个作业号，Linux系统会为其分配一个PID，区别在于，当你使用`nohup`命令时，如果关闭了该会话，脚本会忽略终端会话发过来的`SIGHUP`信号；由于`nuhup`命令会解除终端和进程的关联，进程也就不再同`STDOUT`和`STDERR`联系在一起，为了保存该命令产生的输出，`nohup`命令会自动将`STDERR`和`STDOUT`的消息重定向到一个名为`nohup.out`文件中；如果使用`nohup`运行了另一个命令，该命令的输出会被追加到已有的`nohup.out`文件中，当运行位于同一个目录中的多个命令时一定要当心，因为所有输出都会被发送到同一个`nohup.out`文件中，结果会让人摸不清头脑；
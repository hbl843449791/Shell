### 使用if-then语句
+ 最基本的结构化命令就是if-then语句；格式如下：
```
if command 会执行if后面的command，若该command的退出状态码为0将会执行后面的then部分的命令，如果该命令的退出状态码为其他值，then后面的command就不会被执行，如if grep $testuser /etc/password
then
       commands 可以使用不止一条命令，如echo "this is what";echo "this is sb"
fi 表示该if-then语句到此结束
```
+ 有时候也会看见这样的形式：
```
if command;then
       commands
fi
```

### 使用if-then-else语句
```
if command if语句中的命令返回退出状态码为0时，then部分的命令将会被执行，当if语句中的退出状态码为非0时，将会执行else部分的命令
then
       commands
else
       commands
fi
```

### 嵌套if
+ 有时你需要检查脚本中的多种条件，可以使用嵌套的if-then语句；普通的形式：
```
if command
then
       commands
else
       commands
       if command
       then
              commands
       fi
fi
```
+ 使用elif形式：
```
if command
then
       commands
elif command
then
       commands
else
       commands
fi
```


### MySQL数据库
+ shell脚本的问题之一是持久性数据，你可以将所有信息都保存在shell脚本变量中，但脚本运行结束后，这些变量就不存在
#### 使用MySQL
+ 通往MySQL数据库的门户是`mysql`命令行界面程序；
#### 连接到服务器
+ `mysql`客户端程序允许你通过用户账户和密码连到网络中任何地方的MySQL数据库服务器，默认情况下，如果你在命令行上输入`mysql`，且不加任何参数，他会试图用linux登录用户名连接运行在同一linux系统上的MySQL服务器；`mysql -u root -p`；

![image](https://github.com/ningbaoqi/Shell/blob/master/gif/pic-95.jpg) 

+ `-p`参数告诉mysql程序提示输入登录用户输入密码，输入root用户账户的密码，这个密码要么是在安装过程中，要么是使用mysqladmin工具获得的，一旦登录了服务器，就可以输入命令操作数据库了；
#### mysql命令
+ `mysql`程序使用两种不同类型的命令：特殊的mysql命令；标准SQL语句；

![image](https://github.com/ningbaoqi/Shell/blob/master/gif/pic-96.pg)

+ mysql程序实现了MySQL服务器支持的所有标准的SQL命令，mysql程序实现的一条很棒的SQL命令是`SHOW`命令，可以利用这条命令提取MySQL服务器的相关信息，比如创建的数据库和表：

![image](https://github.com/ningbaoqi/Shell/blob/master/gif/pic-96jpg)

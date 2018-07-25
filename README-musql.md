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

![image](https://github.com/ningbaoqi/Shell/blob/master/gif/pic-96.jpg)

+ mysql程序实现了MySQL服务器支持的所有标准的SQL命令，mysql程序实现的一条很棒的SQL命令是`SHOW`命令，可以利用这条命令提取MySQL服务器的相关信息，比如创建的数据库和表：

![image](https://github.com/ningbaoqi/Shell/blob/master/gif/pic-97.jpg)
![image](https://github.com/ningbaoqi/Shell/blob/master/gif/pic-98.jpg)
![image](https://github.com/ningbaoqi/Shell/blob/master/gif/pic-99.jpg)

+ 用SQL命令SHOW来显示当前在MySQL服务器上配置过的数据库，然后用SQL命令`USE`来连接到单个数据库，mysql会话一次只能连一个数据库；在使用每个命令后面都会添加一个分号，在mysql程序中，分号表明命令的结束，如果不用分号，他会提示输入更多的数据：

![image](https://github.com/ningbaoqi/Shell/blob/master/gif/pic-100.jpg)

+ 用大写字母来表示SQL命令，这已经成了编写SQL命令的通用方式，但mysql程序支持用大写或小写字母来指定SQL命令；

#### 创建数据库
+ MySQL服务器将数据组织成数据库，数据库通常保存着单个应用程序的数据，与用这个数据库服务器的其他应用互不相关，为每个shell脚本应用创建一个单独的数据库有助于消除混淆，避免数据混用；创建一个新的数据库要用如下SQL语句：`CREATE DATABASE name`；你必须拥有在MySQL服务器上创建新数据库的权限，最简单的办法就是作为root用户登录MySQL服务器；

![image](https://github.com/ningbaoqi/Shell/blob/master/gif/pic-101.jpg)

+ 现在数据库mytest已经创建完毕；可以创建一个新的用户账户来访问新数据库了；

#### 创建用户账户
+ 到目前为止，用root管理员账户连接到MySQL服务器，这个账户可以完全的控制所有的MySQL服务器对象；在普通应用中使用MySQL的root账户是及其危险的，如果有安全漏洞或有人弄到了root用户账户的密码，各种糟糕事情都可能发生在你的系统上；为了阻止这种情况发生，明智的做法是在MySQL上创建一个仅对应用中所涉及的数据库有权限的独立用户账户，可以用`GRANT sql`语句来完成；

![image](https://github.com/ningbaoqi/Shell/blob/master/gif/pic-102.jpg)

+ 第一部份定义了用户账户对数据库有哪些权限，这条语句允许用户查询数据库数据，插入新的数据记录以及删除和更新已有数据记录；`test.*`项定义了权限作用的数据库和表：通过该格式指定：`databse.table`；在指定数据库和表时可以使用通配符，这种格式会将指定的权限作用在名为test的数据库中的所有表上；最后，可以指定这些权限应用于哪些用户账户，`grant`命令的便利之处在于，如果用户账户不存在，会创建该账户，`identified by`部分允许你为新用户账户设定默认密码；可以直接在mysql程序中测试新用户账户：

![image](https://github.com/ningbaoqi/Shell/blob/master/gif/pic-103.jpg)

+ 第一个参数指定使用的默认数据库mytest，`-u`选项定义了登录的用户，`-p`用来提示输入密码，输入`test`用户账户的密码后（两个单引号中的字符串就是密码），就可以连到服务器；现在已经有了数据库和用户账户，可以为数据创建表了；
##### 创建数据表
+ MySQL是一种关系数据库，在关系数据库中，数据按照字段、记录和表进行组织；数据字段是信息的单个组成部分，比如员工的姓名或工资；记录是相关数据字段的集合，比如员工ID号、姓、名、地址和工资；每条记录都代表一组数据字段；表含有保存相关数据的所有记录； 要在数据库中新建一张表，需要使用SQL命令`CREATE TABLE`：

![image](https://github.com/ningbaoqi/Shell/blob/master/gif/pic-104.jpg)

+ 首先要注意：为了创建一张表，我们需要用root用户账户登陆在MySQL上，因为test用户没有创建表的权限，接下来，我们在mysql程序命令行上指定了mytest数据库，不这样做的话，就需要用SQL命令USE来连接到mytest数据库；在创建新表前，很重要的一点就是：要确保你使用了正确的数据库，另外还要确保使用管理员用户账户登陆来创建表；

|MySQL数据库支持很多的数据类型|描述|
|------|------|
|`char`|定长字符串值|
|`varchar`|变长字符串值|
|`int`|整数值|
|`float`|浮点值|
|`boolean`|布尔类型值|
|`date`|YYYY-MM-DD格式的日期值|
|`time`|HH-mm-ss格式的日期值|
|`timestamp`|日期和时间值的组合|
|`text`|长字符串值|
|`BLOB`|大的二进制值，比如图片或视频剪辑|

+ `empid`数据字段还指定了一个数据约束，数据约束会限制输入什么类型数据可以创建一个有效的记录，`not null`数据约束指明了每条记录必须有一个指定的`empid`值；最后，`primary key`定义了可以唯一标识每条记录的数据字段，这意味着每条记录中在表中都必须有一个唯一的`empid`值；创建新表后，可以用对应的命令来确保他创建成功了，在mysql中是用`show tables`命令，有了新表就可以保存一些数据了；

##### 插入和删除数据
+ 需要使用SQL命令INSERT来向表中插入新的记录，每条`INSERT`命令都必须指定数据字段值来供MySQL服务器接受该记录：INSERT的格式：`INSERT INTO table VALUES (..)`；每个数据字段的值都必须用逗号分开：

![image](https://github.com/ningbaoqi/Shell/blob/master/gif/pic-105.jpg)

+ `INSERT`命令会将指定的数据写入表中的数据字段里，如果试图添加另外一条包含相同的empid数据字段值的记录，就会得到一条错误消息；如果需要从表中删除数据，可以使用`DELETE`；`DELETE`命令的格式为：`DELETE FROM table`；其中table指定了要从中删除记录的表，要小心，他会删除该表中所有记录；要想删除其中一条或多条数据行，必须使用WHERE子句，WHERE子句允许创建一个过滤器来指定删除哪些记录，可以像：`DELETE FROM employees WHERE empid=2`；，这条命令只会删除empid值为2的所有记录，当你执行这条命令时，mysql程序会返回一条消息来说明有多少个记录符合条件：

![image](https://github.com/ningbaoqi/Shell/blob/master/gif/pic-106.jpg)

##### 查询数据
+ 所有查询都是用SQL命令SELECT来完成，格式为：`SELECT datafields FROM table`；`datafields`参数是一个用逗号分开的数据字段名称列表，指明了希望查询返回的字段；如果要提取所有数据字段值，可以使用星号通配符；必须指定要查询的表；

![image](https://github.com/ningbaoqi/Shell/blob/master/gif/pic-107.jpg)

+ 可以用一个或多个修饰符定义数据库服务器如何返回查询数据：`WHERE`：显示符合特定条件的数据行子集；`ORDER BY`：以指定顺序显示数据行；`LIMIT`：只显示数据行的一个子集；如：`SELECT * FROM emplyees WHERE salary > 4000`；
### 在脚本中使用数据库
#### 登录到服务器
+ `mysql mytest -u test1 -p test1`：就可以登录了；但是所有能够访问脚本的人都知道了密码；要解决这个问题，可以借助mysql程序所使用的一个特殊的配置文件，mysql程序使用`$HOME/.my.cnf`文件来读取特定的启动命令和设置，其中一项设置就是用户启动的mysql会话的默认密码；如果没有这个文件，可以创建；可以设置这个文件的权限，限制只能本人浏览；

![image](https://github.com/ningbaoqi/Shell/blob/master/gif/pic-108.jpg)

#### 向服务器发送命令
+ 在建立起到服务器的连接后，接着就可以向数据库发送命令进行交互；有两种方式可以实现：发送单个命令并退出；发送多个命令；要发送单个命令，必须将命令作为mysql命令行的一部分，对于mysql命令，可以用`-e`选项：

![image](https://github.com/ningbaoqi/Shell/blob/master/gif/pic-109.jpg)

+ 数据库服务器会将sql命令的结果返回给shell脚本，脚本会将他们显示在STDOUT中；如果需要发送多条SQL命令，可以利用文件重定向，要在shell脚本中重定向多行内容，就必须定义一个结束（`EOF`）字符串，结束字符串指明了重定向数据的开始和结尾：

![image](https://github.com/ningbaoqi/Shell/blob/master/gif/pic-110.jpg)

+ shell会将EOF分隔符之间的所有内容重定向给mysql命令，mysql命令会执行这些命令行，就像在提示符上输入一样，用了这种方法，可以根据需要向MySQL服务器发送任何多条命令，但是要注意：每条命令的输出之间没有任何分隔；当使用输入重定向时，mysql程序改变了默认的输出风格，mysql程序检测到了输入是重定向过来的，所以它只返回了原始数据而不是在数据两边加上ASCII符号框，这非常有利于提取个别的数据元素；可以在脚本中使用任何的SQL命令：

![image](https://github.com/ningbaoqi/Shell/blob/master/gif/pic-111.jpg)

+ 在指定结束字符串时，它必须是该行唯一的内容；并且该行必须以这个字符串开头；
#### 格式化数据
+ mysql命令的标准输出并不适合提取数据，如果要对提取到的数据进行处理，需要做一些特殊操作；提取数据库数据的第一步是将mysql命令的输出重定向到一个环境变量，这允许你在其他命令中使用输出信息如：

![image](https://github.com/ningbaoqi/Shell/blob/master/gif/pic-112.jpg)

+ `-B`选项指定mysql程序工作在批处理模式，`-s`选项用于禁用输出列标题和格式化符号；通过将mysql命令的输出重定向到一个变量，可以逐步输出每条返回记录里的每个值；mysql程序还支持另外一种叫作可扩展标记语言XML的流行格式了这种语言使用和HTML类似的标签来标识数据名和值：对于mysql程序，可以用`-X`命令行选项来输出：

![image](https://github.com/ningbaoqi/Shell/blob/master/gif/pic-113.jpg)

+ 通过使用XML，能够轻松标识出每条记录以及记录中的各个字段值，然后可以处理该数据了；

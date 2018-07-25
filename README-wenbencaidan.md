### 创建文本菜单
+ 通常菜单脚本会清空显示区域，然后显示可用的选项列表，用户可以按下与每个选项关联的字母或数字来选择选项；shell脚本菜单的核心就是`case`命令，`case`命令会根据用户在菜单上的选择来执行特定命令；
#### 创建菜单布局
+ 创建菜单的第一步是决定在菜单上显示哪些元素以及想要显示的布局方式；创建菜单前，首先要清空会话中显示的数据。运行`clear`命令即可；默认情况下，echo命令只显示可打印文本字符，在创建菜单选项时，非可打印字符通常也很有用，如：制表符、换行符，要在echo命令中包含这些字符，必须用`-e`选项：如：‘echo -e "1. \tDisplay disk space"’；将会输出：`1.      Display disk space`；`echo -en "\t\tEnter option:"`：`-en`选项会去掉末尾的换行符，让菜单看上去更专业一点，光标会一直在行尾等待用户的输入；创建菜单的最后一步是获取用户输入，这步用read命令，因为希望只有单字符的输入，所以read命令中用`-n`选项来限制只读取一个字符，这样用户只需要输入一个数字，也不用按回车键：`read -n 1 option`；
#### 创建菜单函数
+ 要为每个菜单选项创建独立的函数，创建shell菜单脚本的第一步是决定希望脚本运行哪些功能，然后将这些功能以函数的形式放在代码中：如：

![image](https://github.com/ningbaoqi/Shell/blob/master/gif/pic-80.jpg) 

+ 这样任何时候都能调用menu函数来重现菜单；
### 添加菜单逻辑
+ `case`命令应该根据菜单中输入的字符来调用相应的函数，用默认的case命令字符（星号）来处理所有不正确的菜单项：

![image](https://github.com/ningbaoqi/Shell/blob/master/gif/pic-81.jpg) 

### 整合shell脚本菜单

![image](https://github.com/ningbaoqi/Shell/blob/master/gif/pic-82.jpg) 

### 使用`select`命令
+ `select`命令只需要一条命令就可以创建出菜单，然后获取输入的答案并自动处理，select命令格式如下：

```
select variable in list
do
       commands
done
```
+ `list`参数是由空格分隔的文本选项列表，这些列表构成了整个菜单，select命令会将每个列表项显示成一个带编号的选项，然后为选项显示一个由`PS3`环境变量定义的特殊提示符；
#### select命令例子

![image](https://github.com/ningbaoqi/Shell/blob/master/gif/pic-83.jpg) 

+ select语句中的所有内容必须作为一行出现，运行脚本时，他会自动的生成如下菜单：

![image](https://github.com/ningbaoqi/Shell/blob/master/gif/pic-84.jpg) 
+ 在使用select命令时，记住，存储在变量中的结果值是整个文本字符串而不是跟菜单选项相关联的数字，文本字符串值才是你要在case语句中进行比较的内容；

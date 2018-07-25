### 什么是正则表达式
#### 定义
+ 正则表达式是你所定义的模式模版，Linux工具可以用它来过滤文本，Linux工具如sed、gawk等能够在处理数据时使用正则表达式对数据进行模式匹配，如果数据匹配模式，他就会被接受并进一步处理，如果数据不匹配模式，他就会被过滤掉；
#### 正则表达式的类型
+ 正则表达式是通过正则表达式引擎实现的，正则表达式引擎是一套底层软件，负责解释正则表达式模式并使用这些模式进行文本匹配；Linux中，有两种流行的正则表达式引擎：POSIX基础正则表达式（base regular expression，BRE）引擎；POSIX扩展正则表达式（extended regular expression,ERE）引擎；

### 定义BRE模式
+ 最基本的BRE模式是匹配数据流中的文本字符；
#### 纯文本
+ 第一条原则就是：正则表达式模式都区分大小写，这意味着他们只会匹配大小写也相符的模式；`echo "this is a Test" | sed -n '/this/p'`；
#### 特殊字符
+ 有些字符在正则表达式中有特殊的含义：`.*[]^${}\+?|()`； 如果要用某个特殊字符作为文本字符，就必须转义，在转义特殊字符时，需要在它前面加一个特殊字符来告诉正则表达式引擎应该将接下来的字符当作普通的文本字符，这个特殊字符就是反斜线`\`；
##### 例子
+ 转义美元符号；

![image](https://github.com/ningbaoqi/Shell/blob/master/gif/pic-10.jpg) 

+ 转义反斜线符号；

![image](https://github.com/ningbaoqi/Shell/blob/master/gif/pic-11.jpg) 

+ 尽管正斜线不是正则表达式的特殊字符，但它出现在sed编辑器或gawk编辑器程序的正则表达式中，会报错误：所以也需要将正斜线转义；

![image](https://github.com/ningbaoqi/Shell/blob/master/gif/pic-12.jpg) 

### 锚字符
+ 有两个特殊字符可以用来将模式锁定在数据流中的行首或行尾；
#### 锁定行首
+ 脱字符`^`定义从数据流中文本行的行首开始的模式，如果模式出现在行首之外的位置，正则表达式模式则无法匹配；要使用脱字符，就必须将它放在正则表达式中指定的模式前面：脱字符会在每行的行首检查；就是判断是不是已指定文本开头；

![image](https://github.com/ningbaoqi/Shell/blob/master/gif/pic-13.jpg) 

+ 如果将脱字符放在模式开头之外的其他位置，那么它就是一个普通字符，不再是特殊字符；

![image](https://github.com/ningbaoqi/Shell/blob/master/gif/pic-14.jpg) 

+ 注意：如果指定正则表达式模式时只用了脱字符，就不需要用反斜线来转义，但如果模式中先指定了脱字符，随后还有其他一些文本，那么必须将脱字符转义；

#### 锁定在行尾
+ 特殊字符美元符号`$`定义了行尾锚点，将这个特殊字符放在文本模式之后来指明数据行必须以该文本模式结尾；就是判断是不是已指定文本结尾；要想匹配，文本模式必须是行的最后一部分；

![image](https://github.com/ningbaoqi/Shell/blob/master/gif/pic-15.jpg) 

#### 组合锚点
+ 可以在同一行中将行首锚点和行尾锚点组合在一起使用没在第一种情况下：假定要查找只含有特定文本模式的数据行；

![image](https://github.com/ningbaoqi/Shell/blob/master/gif/pic-16.jpg) 

+ 第二种情况：将两个锚点直接结合在一起，之间不加任何的文本，这样过滤出数据流中的空白行：这是从文档中删除空白行的有效方法；

![image](https://github.com/ningbaoqi/Shell/blob/master/gif/pic-17.jpg) 

### 点号字符
+ 特殊字符`点号`用来匹配除换行符之外的任意单个字符，它必须匹配一个字符，如果在点号字符的位置没有字符，那么模式就不成立；

![image](https://github.com/ningbaoqi/Shell/blob/master/gif/pic-18.jpg) 

+ 第一行在`at`之间有空格，不能匹配，第二行能够匹配，第三行能够匹配，第四行因为`空格符也是字符`，所以匹配；第五行因为在`at`前没有字符，所以不匹配；第六行匹配；
### 字符组
+ 可以定义用来匹配文本模式中某个位置的一组字符，如果字符组中的某个字符出现在数据流中，那么它就匹配了该模式； 使用方括号来定义一个字符组，方括号中包含了所有你希望出现在该字符组中的字符，然后可以在模式中使用整个组：

![image](https://github.com/ningbaoqi/Shell/blob/master/gif/pic-19.jpg)

+ 在不太确定某个字符的大小写时，字符组非常有用：

![image](https://github.com/ningbaoqi/Shell/blob/master/gif/pic-20.jpg)

+ 可以在单个表达式中用多个字符组：

![image](https://github.com/ningbaoqi/Shell/blob/master/gif/pic-21.jpg)

+ 字符组不必只包含字母，也可以包含数字等所有有效的字符；
#### 匹配邮编的例子：如果要确保只匹配五位数，就必须将匹配的字符和其他字符分开要么使用空格，要么使用锚点

![image](https://github.com/ningbaoqi/Shell/blob/master/gif/pic-22.jpg)

### 排除型字符组
+ 可以寻找组中没有的字符，只要在字符组的开头加上脱字符；

![image](https://github.com/ningbaoqi/Shell/blob/master/gif/pic-23.jpg)

+ 通过排除型字符组，正则表达式模式会匹配c或h之外的任何字符以及文本模式，由于空格字符也属于这个范围，它会通过模式匹配，但即便是排除，字符组仍然必须匹配一个字符；
### 区间
+ 可以用单破折号在字符组中表示字符区间，只需要指定区间的第一个字符、单破折号以及区间的最后一个字符就行了（包含边缘），根据Linux系统采用的字符集，正则表达式会包括此区间内的任意字符；
#### 查找邮编的例子
![image](https://github.com/ningbaoqi/Shell/blob/master/gif/pic-24.jpg)

### 扩展正则表达式
+ POSIX ERE模式包括了一些可供Linux应用和工具使用的额外符号，gawk程序能够识别ERE模式，但sed编辑器不能；sed编辑器和gawk程序的正则表达式引擎之间是有区别的；
#### 问号
+ 问号表明前面的字符可以出现`0次或一次`；

![image](https://github.com/ningbaoqi/Shell/blob/master/gif/pic-30.jpg) 

+ 可以将问号和字符组一起使用：

![image](https://github.com/ningbaoqi/Shell/blob/master/gif/pic-31.jpg) 

#### 加号
+ 加号表明前面的字符可以出现`一次或多次`，但必须至少出现一次；

![image](https://github.com/ningbaoqi/Shell/blob/master/gif/pic-32.jpg) 

+ 加号可以与字符组一起使用：

![image](https://github.com/ningbaoqi/Shell/blob/master/gif/pic-33.jpg) 

#### 使用花括号
+ 花括号允许为可重复的正则表达式指定上限和下限，通常称为间隔；可以使用两种格式来指定区间：m：正则表达式准确出现m次；m，n：正则表达式至少出现m次，最多出现n次；根据这个特性可以精确的调整字符或字符集在模式中具体出现的次数； 默认情况下，gawk程序不会识别正则表达式间隔，必须指定gawk的`--re-interval`选项才能识别正则表达式间隔；

![image](https://github.com/ningbaoqi/Shell/blob/master/gif/pic-34.jpg) 

+ 间隔模式可以与字符组一起使用：

![image](https://github.com/ningbaoqi/Shell/blob/master/gif/pic-35.jpg) 

#### 管道符号
+ 管道符号`|`允许检查数据流时，用逻辑或方式指定正则表达式引擎要用到的两个或多个模式，如果任意一个模式匹配了数据流文本，文本就会通过测试，如果没有模式匹配，则数据流文本匹配失败；使用管道符号的格式为：`expr1|expr2|.......`：如：

![image](https://github.com/ningbaoqi/Shell/blob/master/gif/pic-36.jpg) 

+ 正则表达式和管道符号之间不能有空格，否则他们就会被认为是正则表达式模式的一部分；管道符号两侧的正则表达式可以采用任何正则表达式模式（包括字符组）来定义文本：

![image](https://github.com/ningbaoqi/Shell/blob/master/gif/pic-37.jpg) 

#### 表达式分组
+ 正则表达式模式可以使用圆括号`（）`进行分组，当将正则表达式模式分组时，该组会被视为一个标准字符，可以像使用其他字符一样给该组使用特殊字符；结尾的urday分组以及分号，使得模式能够完全匹配完整的Saturday或Sat；

![image](https://github.com/ningbaoqi/Shell/blob/master/gif/pic-38.jpg) 

+ 将分组和管道符号一起使用来创建可能的模式匹配组是常见的做法：`(c|b)a(b|t)`会匹配第一组中字母的任意组合以及第二组中字母的任意组合；

![image](https://github.com/ningbaoqi/Shell/blob/master/gif/pic-39.jpg) 

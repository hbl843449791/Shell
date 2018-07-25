### 制作窗户
+ 制作窗口需要使用其他的库，需要安装`dialog`；命令如：`sudo apt-get install dialog`；这条命令将会为你的系统安装dialog包以及需要的库；
#### dialog包
+ dialog命令使用命令行参数来决定生成哪种窗口部件，部件是dialog窗口元素类型的术语；

|dialog部件|描述|
|------|------|
|`calendar`|提供选择日期的日历|
|`checklist`|显示多个选项（其中每个选项都能打开或关闭）|
|`form`|创建一个带有标签以及文本字段（可以填写内容）的表单|
|`fselect`|提供一个文件选择窗口来浏览选择文件|
|`gauge`|显示完成的百分比进度条|
|`infobox`|显示一条消息，但不用等待回应|
|`inputbox`|提供一个输入文本用的文本表单|
|`inputmenu`|提供一个可编辑的菜单|
|`menu`|显示可选择的一系列选项|
|`msgbox`|显示一条消息，并要求用户选择OK按钮|
|`pause`|显示一个进度条来显示暂停期间的状态|
|`passwordbox`|显示一个文本框，但会隐藏输入的文本|
|`passwordform`|显示一个带标签和隐藏文本字段的表单|
|`radiolist`|提供一组菜单选项，但只能选择其中一个|
|`tailbox`|用tail命令在滚动窗口中显示文件的内容|
|`tailboxbg`|跟tailbox一样，但是在后台模式中运行|
|`textbox`|在滚动窗口中显示文件的内容|
|`timebox`|提供一个选择小时、分钟和秒数的窗口|
|`yesno`|提供一条带有yes和no按钮的简单消息|

+ 要在命令行指定某个特定的部件，需要使用双破折号格式：`dialog --widget paramters`；其中widget是上表部件名，paramters定义了部件窗口的大小以及部件需要的文本；每个dialog部件都提供了两种形式的输出：`使用STDERR；使用退出状态码`；可以通过dialog命令的退出状态码来确定用户选择的按钮，如果选择了yes或ok按钮，dialog命令会返回退出状态码0，如果选择了cancel或mo按钮，dialog命令返回退出状态码1；可以使用标准的$?变量来确定dialog部件中具体选择了哪个按钮；如果部件返回了数据，比如菜单选择，那么dialog命令会将数据发送到STDERR，可以用标准的bash shell方法来将STDERR输出重定向到另一个文件或文件描述符中；如：`dialog --inputbox "enter your age:" 10 20 2>age.txt`；这个命令会将文本框中输入的文本重定向到age.txt文件中；
##### msgbox部件
+ 他会在窗口中显示一条简单的消息，知道用户单击Ok按钮后才会消失‘；使用msgbox部件的格式：`dialog --msgbox text height width`；text参数是你想在窗口中显示的字符串，dialog命令会根据由heght和width参数创建的窗口大小来自动换行，如果想在窗口顶部放一个标题，可以使用`--title`参数，后接作为标题的文本，如：`dialog --title Testing --msgbox "This is a test"  10 20`；输入该命令后就会出现如下界面：

![image](https://github.com/ningbaoqi/Shell/blob/master/gif/pic-70.jpg) 

+ 可以点击OK按键来关闭对话框（但是只是在下面显示终端，不美观），也可以使用键盘命令来模拟单击动作--按下回车键；

##### yesno部件
+ yesno部件进一步扩展了msgbox部件的功能，允许用户对窗口中显示的问题选择yes或no，他会在窗口底部生成两个按钮，一个是yes，一个是no，用户可以使用鼠标、制表符或键盘方向键来切换按钮，要选择按钮的话，用户可以按下空格键或回车键；如：`dialog --title "please answer“ --yesno " is this thing on?" 10 20；echo $?`将会显示：

![image](https://github.com/ningbaoqi/Shell/blob/master/gif/pic-71.jpg) 

+ dialog命令的退出状态码会根据用户选择的按钮来设置，如果用户选择了no按钮，退出状态码是1；如果选择了yes按钮，退出状态码就是0；

##### inputbox部件
+ inputbox部件为用户提供了一个简单的文本框区域来输入文本字符串，dialog命令会将文本字符串的值发给STDERR，你必须重定向STDERR来获取用户输入；`dialog --inputbox "enter your age:" 10 20 2>age.txt；echo $?`：如：

![image](https://github.com/ningbaoqi/Shell/blob/master/gif/pic-72.jpg) 

##### textbox部件
+ textbox部件是在窗口中显示大量信息的极佳办法，他会生成一个滚动窗口来显示由参数所指定的文件中的文本；`dialog --textbox /etc/passwd 15 45`；`/etc/passwd`文件的内容会显示在可滚动的文本窗口中：

![image](https://github.com/ningbaoqi/Shell/blob/master/gif/pic-73.jpg) 

+ 可以用方向键来左右或上下滚动显示文件的内容，窗口底部的行会显示当前查看的文本处于文件中的哪个位置（百分比），文本框中包含一个用来选择退出部件的退出按钮；
##### menu部件
+ menu部件允许你来创建我们之前所制定的文本菜单的窗口版本，只要为每个选项提供一个选择标号和文本就行；命令：`dialog --menu "Sys Admin Menu" 20 30 10 1 "Display disk space" 2 "Display users" 3 "Display memory usage" 4 "Exit" 2>test.txt`；第一个参数定义了菜单的标题，之后的两个参数定义了菜单窗口的高和宽，而第四个参数则定义了在窗口中一次显示的菜单项都应该是唯一的，可以通过在键盘上按下对应的键来选择。第二个元素是菜单中使用的文本；

![image](https://github.com/ningbaoqi/Shell/blob/master/gif/pic-74.jpg) 

+ 如果用户通过按下标号对应的键选择了某个菜单项，该菜单项会高亮显示但不会被选定，直到用户用鼠标或回车键选择了OK按钮时，选项才会最终选定，dialog命令会将选定的菜单项文本发送到STDERR，可以根据需要重定向STDERR；
##### fselect部件
+ fselect部件在处理文件名时非常方便，不用强制用户键入文件名，你就可以用fselect部件来浏览文件的位置并选择文件；fselect部件的格式如：`dialog --title "select a file" --fselect $HOME/ 10 50 2>file.txt`；

![image](https://github.com/ningbaoqi/Shell/blob/master/gif/pic-75.jpg) 

+ fselect选项后的第一个参数是窗口中使用的起始目录位置，fselect部件窗口由左侧的目录列表，右侧的文件列表（显示了选定目录下的所有文件）和含有当前选定的文件或目录的简单文本框组成，可以手动在文本框键入文件名，或者用目录和文件列表来选定（使用空格键选择文件，并将其加入到文本框中）；

|dialog选项|描述|
|------|------|
|`--add-widget`|继续下个对话框，直到按下Esc或Cancel按钮|
|`--aspect ratio`|指定窗口宽度和高度的宽高比|
|`--backtitle title`|指定显示在屏幕顶部背景上的标题|
|`--begin x y`|指定窗口左上角的起始位置|
|`--cancel-label label`|指定Cancel按钮的替代标签|
|`--clear`|用默认的对话背景色来清空屏幕内容|
|`--cr-wrap`|在对话文本中允许使用换行符并强制换行|
|`--colors`|在对话文本中嵌入ANSI色彩编码|
|`--create-re file`|将示例配置文件的内容复制到指定的file文件中|
|`--defaultno`|将yes/no对话框的默认答案设置为no|
|`--default-item string`|设定复选列表、表单或菜单对话中的默认项|
|`--exit-label label`|指定exit按钮的替代标签|
|`--extra-button`|在Ok按钮和cancel按钮之间显示一个额外的按钮|
|`--extra-label label`|指定额外按钮的替代标签|
|`--help`|显示dialog命令的帮助信息|
|`--help-button`|在OK按钮和cancel按钮后显示help按钮|
|`--help-label label`|指定help按钮的替代标签|
|`--help-status`|当选定help按钮后，在帮助信息后写入多选列表，单选列表或表单信息|
|`--ignore`|忽略dialog不能识别的选项|
|`--input-fd fd`|指定STDIN之外的另一个文件描述符|
|`insecure`|在password部件中键入内容时显示星号|
|`--item-help`|在多选列表、单选列表或菜单中的每个标号在屏幕的底部添加一个帮助栏|
|`--keep-window`|不要清除屏幕上显示过的部件|
|`--max-inout size`|指定输入的最大字符串长度，默认为2048|
|`--nocancel`|隐藏cancel按钮|
|`--no-callapse`|不要将对话文本中的制表符转换成空格|
|`--no-kill`|将tailboxbg对话放在后台，并禁止该进程的SIGHUP信号|
|`--no-label label`|为no按钮指定替代标签|
|`--no-shadow`|不要显示对话窗口的阴影效果|
|`--ok-label label`|指定ok按钮的替代标签|
|`--output-fd fd`|指定除了STDERR之外的另一个输出文件描述符|
|`--print-maxsize`|将对话窗口的最大尺寸打印到输出中|
|`--print-size`|将每个对话框窗口的大小打印到输出中|
|`--print-version`|将dialog的版本号打印到输出中|
|`--separate-output`|一次一次的输出checklist部件的效果不使用引号|
|`--separate string`|指定用于分隔部件输出的字符串|
|`--separate-widget string`|指定用于分隔部件输出的字符串|
|`--shadow`|在每个窗口的右下角绘制阴影|
|`--single-quoted`|需要时对多列表的输出采用单引号|
|`--sleep sec`|在处理完对话窗口之后延迟指定的秒数|
|`--stderr`|将输出发送到STDERR（默认行为）|
|`--stdout`|将输出发送到STDOUT|
|`--tab-len n`|指定一个制表符占用的空格数（默认为8）|
|`--tab-correct`|将制表符转换成空格|
|`--timeout sec`|指定无用户输入时，sec秒后退出并返回错误代码|
|`--title title`|指定对话窗口的标题|
|`--trim`|从对话文本中删除前导空格和换行符|
|`--visit-items`|修改对话窗口中制表符的停留位置，使其包括选项列表|
|`--yes-label label`|为yes按钮指定替代标签|

### 在脚本中使用dialog命令
+ 必须记住：如果有cancel或no按钮，检查dialog命令的退出状态码；重定向STDERR来获得输出值；
#### 例子：使用dialog部件来生成系统管理菜单

![image](https://github.com/ningbaoqi/Shell/blob/master/gif/pic-76.jpg) 

+ 在脚本中使用while循环和一个真值常量创建了个无限循环来显示菜单对话，这意味着，执行完每个函数之后，脚本都会返回继续显示菜单。脚本用mktemp命令创建两个临时文件来保存dialog命令的数据，第一个临时文件`$temp`用于保存df和meminfo命令的输出，这样就能在textbox对话中显示它们了，第二个临时文件`$temp2`用来保存在主菜单对话中选定的值；运行结果：

![image](https://github.com/ningbaoqi/Shell/blob/master/gif/pic-77.jpg) 
![image](https://github.com/ningbaoqi/Shell/blob/master/gif/pic-78.jpg) 

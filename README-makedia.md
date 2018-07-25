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

### 使用图形
+ 本节描述的kdialog和zenity包，他们各自为KDE和GNOME桌面提供了图形化窗口部件；
#### KDE环境
+ KDE图形化环境默认包含kdialog包，kdialog包使用kdialog命令在KDE桌面上生成类似与dialog部件的标准窗口，生成的窗口能跟其他KDE应用窗口很好的融合，不会造成不协调的感觉，这样你就可以直接在shell脚本中创建能够和windows相媲美的用户界面了；你的Linux发行版使用KDE桌面并不代表他就默认安装了kdialog包，你需要从发行版本库中手动安装；
##### kdialog部件
+ 就像dialog命令，kdialog命令使用命令行选项来指定具体使用哪种类型的窗口部件kdialog命令的格式：`kdialog display-options window-options arguments：window-options`选项允许指定使用哪种类型的窗口部件，选项如下：

|选项|描述|
|------|------|
|`--checklist title [tag item status]`|带有状态的多选列表菜单，可以表明选项是否被选定错误消息框|
|`--error text`|错误消息框|
|`--inputbox text [init]`|输入文本框，可以用init值来指定默认值|
|`--menu title [tag item]`|带有标题的菜单选择框，以及用tag标识的选项列表|
|`--msgbox text`|显示指定文本的简单消息框|
|`--radiolist title [tag item status]`|带有状态的单选列表菜单，可以表明选项是否被选定|
|`--password text`|隐藏用户输入的密码输入文本框|
|`--separate-output`|为多选列表和单选列表菜单返回按行分开的选项|
|`--sorry text`|对不起消息框|
|`--textbox file [width] [height]`|显示file的内容的文本框，可以指定width和height|
|`--title title`|为对话窗口的titlebar区域指定一个标题|
|`--warningyesno text`|带有yes和no按钮的警告消息框|
|`--warningcontinuecancel text`|带有continue和cancel按钮的警告消息框|
|`--warningyesnocancel text`|带有yes no 和cancel按钮的警告消息框|
|`--yesno text`|带有yes和no按钮的提问框|
|`--yesnocancel text`|带有yes、no和cancel按钮的提问框|

+ checklist和radiolist部件允许你在列表中定义单独的选项以及他们默认是否选定；`kdialog --checklist "item i need" 1 "toothbrush" on 2 "toothpaste" off  3 "hair brush" on 4 "deodorant" off 5 "slippers" off`；指定为on的选项会在多选列表中高亮显示，要选择或取消多选列表中的某个选项，只要单击他就行了，如果选择了OK按钮，kdialog将会将标号值发送到STDOUT上；当按下回车键时，kdialog窗口就和选定选项一起出现，当单击ok或cancel按钮时，kdialog命令会将每个标号作为一个字符串值返回到STDOUT，脚本必须能解析结果值并将他们和原始值匹配起来；
##### 使用kdialog
+ 可以在shell脚本中使用kdialog窗口部件，方法类似与dialog部件，最大的不同是kdialog窗口部件用STDOUT来输出值，而不是STDERR；创建系统管理菜单（KDE应用）：

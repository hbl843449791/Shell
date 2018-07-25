### 使用电子邮件
+ 可用来从shell脚本中发送电子邮件的主要工具是Mailx程序，不仅可以用它交互地读取和发送消息，还可以 用命令行参数指定如何发送消息；在你安装包含Mailx程序的mailutils包之前，有些linux发行版本还会要求你安装邮件服务器包；Mailx程序发送消息的命令行的格式为：`mail [-elinv] [-a header] [-b addr] [-c addr] [-s subj] to-addr`；如下是它的参数：

|参数|描述|
|------|------|
|`-a`|指定额外的SMTP头部行|
|`-b`|给消息增加一个BCC：收件人|
|`-c`|给消息增加一个CC：收件人|
|`-e`|如果消息为空，不要发送消息|
|`-i`|忽略TTY中断信号|
|`-I`|强制mailx以交互模式运行|
|`-n`|不要读取/etc/mail.rc启动文件|
|`-s`|指定一个主题行|
|`-v`|在终端上显示投递细节|

+ 如何直接在命令行上创建和发送电子邮件消息：`echo "this is a test massage" | mailx -s "test message" ningbaoqi`；

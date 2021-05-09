fclone shell bot V2.0
教程FAQ 原作教程 脚本1.0手动教程

shellbot可以在TG上调动运行VPS命令，本脚本仅是shellbot的一种google drive转存应用! 当然转存工具很重要，fclone,400 fils/s，没错，速度论文件的，尽管还有其他优点，但是一个速度，已经能对得起它的名字fxxk clone，天下武功，为快不破，你用fclone，其他clone只能看到你的背影。

  

注意: 一键安装配置脚本暂时仅支持(Ubuntu/Debian),centos可手动安装，windowns不可安装！！！

安装步骤：
步骤一：克隆库/赋予脚本权限/运行一键安装脚本
cd /root && git clone https://github.com/cgkings/fclone_shell_bot.git && chmod -R 777 /root/fclone_shell_bot && mv /root/fclone_shell_bot/fcshell.sh /root && /root/fcshell.sh
步骤二：使用一键安装脚本
使用场景Ⅰ：完全安装
使用场景Ⅱ：部分安装
**以上，基本把安装的事说明白了，还不明白的话，建议去看原作者教程或者本脚本的上一版教程

使用说明：
/fq 极速转存
支持任务队列

/fsort 自动整理
生成jason文件： 对于要整理到的文件夹，比如说按番号，你到已经有的番号文件夹（道理相同，女优名字也一样），运行以下命令：
fclone lsjson 你的用户名:{文件夹ID} --fast-list --dirs-only --no-mimetype --no-modtime --max-depth 文件夹层数

得到类似如下信息：

{"Path":"S/SSNI","Name":"SSNI","Size":-1,"ModTime":"","IsDir":true,"ID":"10n2Vz5vdzwg_mgJSWAiT190xMkztnvRx"},
{"Path":"S/SSPD","Name":"SSPD","Size":-1,"ModTime":"","IsDir":true,"ID":"1mqNfuJUiTmwqaY9aC90YQFVFDJWji9WE"},
{"Path":"S/STAR","Name":"STAR","Size":-1,"ModTime":"","IsDir":true,"ID":"1nxBRq5Jg8gzR71wrAaI2up0IP-ucFh4z"},

因为本人学艺不精，所以这个jason信息还要处理一下，把它复制到excel然后分列显示，删除多余列，合并成这个格式：

SSNI:10n2Vz5vdzwg_mgJSWAiT190xMkztnvRx SSPD:1mqNfuJUiTmwqaY9aC90YQFVFDJWji9WE STAR:1nxBRq5Jg8gzR71wrAaI2up0IP-ucFh4z

把这些信息覆盖粘贴到\root\fclone_shell_bot\av_num.txt中（原始文件里是我的分类名称和文件夹ID）

2.运行fsort脚本

最关键的步骤是第1步，只要你第一步没错，脚本会让你输入需要整理的文件夹ID,然后脚本会进行以下操作：

⑴ 遍历需要整理的文件夹内文件名；
⑵ 与av_num.txt内关键字进行比对，如果文件名包含关键字，就会把这个文件移动到关键字的文件夹内；
⑶ 删除整理文件夹内的空文件夹；
⑷ ⑴——⑶步骤循环，直到文件夹内文件的文件名没有包含av_num.txt内关键字为止。

授权书
首次启动时，该漫游器将仅接受来自您的用户的消息。出于安全原因：您不希望任何人向您的计算机发出命令！
如果要允许其他用户使用该漫游器，请使用/token并为其提供结果链接。如果您想在网上论坛上使用此漫游器，/token则会向您发送一条消息，转发到网上论坛

最后的话
送君千语，终有一别，作为一个小白，能堂而皇之的在github上恬不知耻的发布，是因为github开放的开发氛围，更是因为TG上面各位开放而热心的中国技术大佬的无私帮助，在此感谢各位TG大神，排名不分先后：

fxxkrlab （专业冒险者） 不厌其烦的希望我们能多学点语言，还根据我们的需要编写了 转存bot教材,可惜，暂时没研究出来
aevlp （steve x） 转存脚本的鼻祖，无私的提供了使用mysql实现转存任务序列的转存bot，可惜,暂时没研究出来
Ip2N5M （Kali Aska） 小白福音，不给教材，不给案例，直接给答案和工具，感谢他提供的脚本核心代码以及 魔改版gclone，魔改了魔改rclone的gclone,体会一下
shine_y （shine） 我修改的一键转存脚本的原作者，非常感谢，地址
onekingen (oneking) 脚本魔改路上的小伙伴，自定义脚本做了很多，地址
GreatPanoan （Panoan）带我走上买鸡不归路的领路人，没有他，不会开始这段折腾之旅，不管怎样，祝高考顺利，小子！
另外，github上的 hrvstr ,他提供了shellbot上自定义命令的范本，非常感谢他
当然，少不了shellbot的原作者 Botgram
最后，如果你是位外国友人，很荣幸，孙贼，用用google翻译吧！

客服列表
教程FAQ
1#客服： @谷哥；
2#客服： @度娘；
3#客服 @TG群组机器人

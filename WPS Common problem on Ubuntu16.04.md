Ubuntu16.04使用WPS常见问题



不得不说WPS在开源上做的还是很好的。linux版的WPS经过了这么多年的修正已经变得很好用了。本

解决WPS字体缺失问题

1：WPS最新版   这是下载地址。建议下载Alpha版本。Beta版本基本不怎么更新。

2：Mint/Ubuntu/Debian直接安装下载的DEB包即可。

3：打开WPS后会提示什么什么字体缺失。

缺失的是一些微软的字体。安装以下字体包即可解决。

下载地址：WPS缺失字体

ubuntu 16.04在WPS下fcitx不能切换中文搜狗的解决办法

原因：环境变量未正确设置，以上可以直接针对wps设置。
打开终端输入：
$ sudo gedit /usr/bin/wps
可以看到文件内容（文字加粗的部分就是要补齐的,下同）
*******************************
#!/bin/bash
export XMODIFIERS="@im=fcitx"
export QT_IM_MODULE="fcitx"
gOpt=
#gOptExt=-multiply
gTemplateExt=("wpt" "dot" "dotx")
.......
************************
wps表格不能输入中文解决
$ sudo gedit /usr/bin/et
************************
#!/bin/bash
export XMODIFIERS="@im=fcitx"
export QT_IM_MODULE="fcitx"
gOpt=
#gOptExt=-multiply
........

************************
wps演示不能输入中文解决
$ sudo gedit /usr/bin/wpp
************************
#!/bin/bash
export XMODIFIERS="@im=fcitx"
export QT_IM_MODULE="fcitx"
gOpt=
#gOptExt=-multiply
........

修改完后保存，在打开相应的程序切换输入法就可以输入中文了。



Reference:

1:http://forum.ubuntu.org.cn/viewtopic.php?f=48&t=476938

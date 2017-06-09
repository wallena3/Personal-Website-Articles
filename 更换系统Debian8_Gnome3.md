由于昨天弄原生迅雷，装一堆依赖删了装装了删，把cinnamon弄崩了，重装也还是那样，所以干脆从Mint转到Debian了。首先衷心感谢findspace的博客文章，给了我很多帮助。

安装过程

安装过程我选择了链接网络镜像。但是在之后的选择安装什么组件时，只选择第一项Debian desktop  enviroment。因为debian安全服务器实在太慢了，选多了的话要1个多小时才能按完我试过。这样大概10分钟左右，装好搞定。

添加普通用户为sudoer

需要注意，Debian默认没有安装sudo ，apt-get install sudo安装一下。debian下默认新建的用户都是普通用户，不是sudoer，这个和ubuntu有很大区别。这意味着你无法使用sudo来暂时提升权限。

所以你在debian上用普通用户执行sudo命令时，会报如下错误：xxx is not in the sudoers file. This incident will be reported。
出现这个问题，是因为执行sudo命令的用户不在sudoers文件的列表中。可以通过编辑sudoers文件，来解决这个问题。

切换到root，修改/etc/sudoers。

你会看到

# User privilege specification
 root ALL=(ALL:ALL) ALL
比如你的用户名是walle，就再写一行，把root变成walle就可以了
当然如果你理解上面的原理后，可以直接输入如下命令解决此问题

su -echo 'xxx ALL=(ALL:ALL) ALL' >> /etc/sudoers  (其中xxx代表用户名)

关于源

Debian的源163网易和中科大的源都是比较稳定的。

下面给出两个源的地址。

USTC

网易

顺便说一句，中科大的源中有一个配置生成器，可以根据自己的需求来更改。我自己用的是中科大的源

语言问题

如果一开始使用的是英语或者汉语，现在想切换，那就运行

sudo dpkg-reconfigure locales
找到想要的语言，空格键选中，tab键切换到ok，然后移动光标到想要使用的主语言，tab键切到ok。

然后还要在设置中的`区域与语言`中把语言改成英文或中文。注销即可。

输入法

搜狗linux输入法。现在已经非常好用了，不折腾。注销后在fcitx configuration中加入搜狗即可

reference:http://www.findspace.name/res/1542

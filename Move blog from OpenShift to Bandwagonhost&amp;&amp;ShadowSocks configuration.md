迁移博客从OpenShift到Bandwagonhost以及SS的配置




Update:2017-03-23

Few days ago I just move my blog to Openshift,But in China,the net speed to Openshift is too slow also unstable. So I buy the Bandwagonhost's VPS.In the same time,I can also deploy the shadowsocks service in the same VPS.I choose the cheapest VPS which only cost 20$ each year,it highlights the value for money.The following will record the process.The full text description is based on Debian8.

1:First, you need to buy a Bandwagonhost account.After that,you can choose the VPS you want to buy.I advise you choose the cheapest one,$2.99/monthly or $19.99/annually.it is strong enough for a blog or shadowsocks server.

2:Like the other VPS provider,Bandwagonhost provide a VPS management dash board named kiwivm.We can do a lot of operation in this panel.Thus there are lots of tutorial in the Internet,I will not describe it.The VPS's memory is very small, so we choose the Debian operation system.

3: Now,we can build our wordpress site now.The process is similar with it was in Aliyun. You can reference my previous articles.

4:Build Shadowsocks service.After reference many people's experiences and practices,I find that Bandwagonhost is the best choice.

Shadowsocks has many editions.Nowadays,on the server site.the mainstream direction is:

shadowsocks-nodejs

shadowsocks-libev

shadowsocks-Python

shadowsocks-go

But on the small memory size VPS,we should use shadowsocks-libev to save memory.

shadowsocks-libev has the following advantages:First:the editon update in time,has many new features.The current version is 2.4.7.Second:Resources occupy very little, memory, CPU occupy very low, even the lowest 64M memory VPS can run it.Third:Deployment, debugging is very simple and intuitive.github address is here.



刚刚把博客迁移到Openshift,之后发现国内的访问速度可以用令人发指来形容，并且访问非常不稳定。故重新购买了Bandwagonhost的VPS，同时可以在上面部署ss服务，一举两得，选择乞丐版每年只需要20美刀，非常的划算，下面记录过程。全文描述都基于Debian8。
1：首先要在Bandwagonhost的官网注册账号，之后便可以进行购买。选择最便宜的$2.99/month的就可以。
2：像其他VPS提供商一样，Bandwagonhost为主机提供了一个管理面板kiwivm。在里面可以进行很多操作，网上的教程很多，这里不在赘述。由于VPS的空间比较小，所以选择安装Debian系统。
3：接下来可以进行wordpress的搭建，这点和在阿里云上的搭建基本一致，可以参考我以前的文章。
4：搭建ss服务。在参考过很多人的经验与实践后发现Bandwagonhost是最合适的。
ss有多个版本，目前服务端主流的是
ss-nodejs
ss-libev
ss-Python
ss-go
但是在小内存的VPS还是要尽量节省内存，所以推荐ss-libev。
ss-libev有如下特点：
其一：版本更新及时，新功能支持的较多，当前最新的版本为2.4.7。其二：资源占用极少，内存、CPU占用都非常低，即使是最低档64M内存的VPS都照跑不误。其三：部署、调试非常简单直观。github的地址在此。

apt方式安装ss

小内存自然要使用debian，下面介绍在debian系统中使用apt方式搭建ss。

1：更新与安装

apt-get update
apt-get install shadowsocks
2：配置shadowsocks

vim /etc/shadowsocks/config.json
{
 "server":"vps的ip",
 "server_port":8388, #服务器端口，与SSH端口不一样
 "local_port":1080,
 "password":"barfoo!", #认证密码
 "timeout":60,
 "method":"aes-256-cfb" #加密方式，推荐使用aes-256-cfb
 }
3：重启shadowsocks服务。

/etc/init.d/shadowsocks stop
/etc/init.d/shadowsocks start
编译方式安装ss

编译安装方式可以安装最新的ss版本。

如果你已经采用apt方式安装过ss，那么使用如下命令删除已安装的ss

dpkg -P shadowsocks
1：安装编译所需包

apt-get update
apt-get install build-essential autoconf libtool libssl-dev git
apt-get install asciidoc
apt-get install gawk debhelper dh-systemd init-system-helpers pkg-config
2：然后通过git下载源码

git clone https://github.com/shadowsocks/shadowsocks-libev.git
3:进入目录编译安装

cd shadowsocks-libev
dpkg-buildpackage -us -uc
cd ..
dpkg -i shadowsocks-libev*.deb
执行到上述第二个命令时可能会报错：
dpkg-buildpackage: dpkg-source: no upstream tarball found
解决办法：使用
dpkg-buildpackage -b -rfakeroot -us -uc
而不是
dpkg-buildpackage -us -uc
即可。

执行命令中还可能报类似错误：

perl: warning: Setting locale failed.
perl: warning: Please check that your locale settings
LANGUAGE = (unset),
LC_ALL = (unset),
LANG = "zh_CN.UTF-8"
are supported and installed on your system.
perl: warning: Falling back to the standard locale ("C").
解决办法：
这种情况一般是vps比较常见。这个问题其实很简单，摆明了就是没设置本地环境变量嘛，不过，我也没有设置lang为zh_CN。因为一般都是用ssh的方式连接到vps上的。sshd有个机制，会把客户机上的语言环境带到远程的机器上。客户机一般都会设置zh_CN.UTF-8语言，用来显示中文，而远端的vps一般就只有en_US.UTF-8。zh_CN.UTF-8一旦带过去就会报找不到的错误。

在ubuntu下面，可以通过：apt-get install language-pack-en-base来解决，但debian下没有这个命令，所以只能：

        export LANGUAGE=en_US.UTF-8
 	export LANG=en_US.UTF-8
 	export LC_ALL=en_US.UTF-8
 	locale-gen en_US.UTF-8
 	dpkg-reconfigure locales
然后会出来一个界面，你在里面选择en_US.UTF-8，然后一切就都解决了。很方便。

4：配置服务及配置文件

mkdir -p /etc/shadowsocks-libev
cp ./debian/shadowsocks-libev.init /etc/init.d/shadowsocks-libev
cp ./debian/shadowsocks-libev.default /etc/default/shadowsocks-libev
cp ./debian/config.json /etc/shadowsocks-libev/config.json
chmod +x /etc/init.d/shadowsocks-libev

到现在为止编译安装已经结束。

5：配置shadowsocks配置文件

vim /etc/shadowsocks-libev/config.json
{
"server":"vps的ip",
"server_port":8388, #服务器端口，与SSH端口不一样
"local_port":1080,
"password":"barfoo!", #认证密码
"timeout":60,
"method":"aes-256-cfb" #加密方式，推荐使用aes-256-cfb
}
6：启动shadowsocks服务。

/etc/init.d/shadowsocks-libev start
（/etc/init.d/shadowsocks-libev stop）
7:新的阿里云香港主机，还需要配置一下 安全组规则 中的 公网入方向。加入一条规则 端口范围为第五步中的
server_port 比如 8388/8388 。授权对象填0.0.0.0/0
Reference

1:http://www.tennfy.com/1477.html

2：https://cokebar.info/archives/767

3：http://askubuntu.com/questions/675154/dpkg-buildpackage-dpkg-source-no-upstream-tarball-found

4：http://www.neatstudio.com/show-2252-1.shtml

5：http://my.oschina.net/u/943306/blog/345923

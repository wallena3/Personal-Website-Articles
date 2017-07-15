
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

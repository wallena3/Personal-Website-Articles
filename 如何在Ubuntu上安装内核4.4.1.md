x.y.z

y双版本号通常代表稳定版本，奇数代表开发中版本。建议安装稳定版本.当前最新稳定版为4.4.1

内核4.4.1已经发布，它是迄今为止发行的最先进的稳定版内核。其中，它带来了驱动更新，修复以及代码优化。

NOTICE : TESTED--NOT compatible with Debian8.3.

RESULT : PC often crashes.

安装指导：

由于编译Linux内核极其困难，Canonical(Ubuntu创始人Mark Shuttleworth的公司)已经将所有的linux内核通过deb包发布。所有使用Ubuntu或者基于Ubuntu的系统的用户都可以通过kernel.ubuntu.com仓库取得deb包。

下面的命令对于Ubuntu，Linux Mint，Elementary OS或者其他所有的衍生版本系统均适用。

如何在32位Ubuntu及衍生系统上安装Linux内核4.4.1：

下载所需要的包:


$ cd /tmp
$ wget
kernel.ubuntu.com/~kernel-ppa/mainline/v4.4.1-wily/linux-headers-4.4.1-040401_4.4.1-040401.201601311534_all.deb
kernel.ubuntu.com/~kernel-ppa/mainline/v4.4.1-wily/linux-headers-4.4.1-040401-generic_4.4.1-040401.201601311534_i386.deb
kernel.ubuntu.com/~kernel-ppa/mainline/v4.4.1-wily/linux-image-4.4.1-040401-generic_4.4.1-040401.201601311534_i386.deb

安装内核:

$ sudo dpkg -i linux-headers-4.4*.deb linux-image-4.4*.deb

可选,删除内核:

$ sudo apt-get remove linux-headers-4.4* linux-image-4.4*

如何在64位Ubuntu及衍生系统上安装Linux内核4.4.1：

下载所需要的包:


$ cd /tmp
$ wget
kernel.ubuntu.com/~kernel-ppa/mainline/v4.4.1-wily/linux-headers-4.4.1-040401_4.4.1-040401.201601311534_all.deb
kernel.ubuntu.com/~kernel-ppa/mainline/v4.4.1-wily/linux-headers-4.4.1-040401-generic_4.4.1-040401.201601311534_amd64.deb
kernel.ubuntu.com/~kernel-ppa/mainline/v4.4.1-wily/linux-image-4.4.1-040401-generic_4.4.1-040401.201601311534_amd64.deb

删除与安装同32位系统。







Reference:http://linuxdaddy.com/blog/install-kernel-4-4-on-ubuntu/

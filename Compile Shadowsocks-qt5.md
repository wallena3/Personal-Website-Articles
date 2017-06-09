自行编译 Shadowsocks-qt5



You can mainly reference wiki on GitHub.To compile shadowsocks-qt5,you must compile libQtShadowsocks first.I will record a compile method ,which is using the deb package to install.To be a comparison with the make&&make install method on GitHub.

According to the Official Guide,many distributions now have ready-made binary package to install.Ubuntu has PPA to install rapidly,only support Ubuntu 14.04 or higher version.But if you use Debian,or want a newer version on Ubuntu,you need to compile it by yourself.You should compile the dependency named libQtShadowsocks first.

Download the source code,attention that you shouldn't use the code in GitHub code repository to compile,you need to download the release zip package.DO NOT use my command directly,the version number may change,please deal with flexibility.

#Follow the wiki guidance,install the dependency.The package qt5-default will change the qt version to qt5

#unzip the tar package ,and entry the directory.

#generate Makefile,ensure the qmake version is qt5

qmake&&make

#generate the deb package

dpkg-buildpackage -uc -us -b

#install the deb package.Location is ../    this way to install is easier to manage than make&&make install

sudo dpkg -i xxxxx.deb
Now let us compile the protagonist Shadowsocks-Qt5.Download the source code,and the procedure is similar with libqtshadowsocks.

#Follow the wiki guidance to install the dependency.

#unzip the tar package and entry the directory

qmake&&make

dpkg-buildpackage -us -uc -b

sudo dpkg -i ../shadowsocks-qt5xxxxx.deb
And now ,enjoy the shadowsocks!

主要可参考GitHub上的wiki。编译shadowsocks-qt5 首先要编译libQtShadowsocks。下面记录另一种编译方法，方法是生成deb包进行安装。以此和github上的make&&make install的方法做一个对照。

根据 官方指南，很多发行版都有现成的二进制安装包， Ubuntu有 PPA快速安装，仅支持 Ubuntu 14.04或更高版本。但是在 Debian 下，或者使用Ubuntu，但是想获得最新的版本，就得我们自己动手了，首先要编译 libQtShadowsocks 这个依赖。

下载 源码，需要注意的是 GitHub版本仓库中的代码并不能直接拿来编译，只能手动下载Release的 zip 源码包。这里的命令不要照搬，版本号有可能发生变更，请灵活处理。

#按照wiki安装好各个依赖 ，需要注意的是 qt5-default 这个包的作用是切换系统 qt 到 qt5
# 解压缩，然后进入目录
# 生成 Makefile，确保 qmake 版本为 qt5
qmake && make
# 生成 deb 包
dpkg-buildpackage -uc -us -b
# 安装编译生成的 deb 包，生成的位置在上层目录中。比起直接 make install 要方便管理
sudo dpkg -i xxxxxx.deb
接下来编译主角 Shadowsocks-Qt5 ，下载 源码 ，与编译 libqtshadowsocks 基本一致。

#按照wiki安装好各个依赖
# 解压缩，然后进入目录
qmake && make
dpkg-buildpackage -uc -us -b
sudo dpkg -i ../shadowsocks-qt5_2.4.1-1_amd64.deb
安装完毕之后就可以使用了。
Reference:

1:http://www.jianshu.com/p/0fac439b3d38
